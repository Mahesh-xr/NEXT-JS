
## Middleware

Middleware runs **before a request is completed**, at the **edge**.

Common uses:

- Authentication
    
- Redirects
    
- Header modification
    
- Blocking routes
    

File location:

```
middleware.ts
```

Runs on:

- Page routes
    
- API routes
    
- Static assets (unless excluded)
    

---

## Middleware example (auth check)

```ts
import { NextRequest, NextResponse } from "next/server";

export function middleware(request: NextRequest) {
  const token = request.cookies.get("token");

  if (!token) {
    return NextResponse.redirect(new URL("/login", request.url));
  }
}
```

---

## Activating middleware: Approach 1 — `matcher`

Limits middleware to specific routes.

```ts
export const config = {
  matcher: ["/dashboard/:path*", "/profile/:path*"],
};
```

Behavior:

- Middleware runs only on `/dashboard` and `/profile`
    
- Skips all other routes
    

---

## Activating middleware: Approach 2 — Conditional logic

Middleware runs for **all routes**, but logic decides when to act.

```ts
export function middleware(request: NextRequest) {
  const pathname = request.nextUrl.pathname;

  if (pathname.startsWith("/dashboard")) {
    const token = request.cookies.get("token");

    if (!token) {
      return NextResponse.redirect(
        new URL("/login", request.url)
      );
    }
  }
}
```

Behavior:

- Middleware executes everywhere
    
- Logic selectively applies rules
    

---

## Matcher vs Conditional logic

|Aspect|Matcher|Conditional|
|---|---|---|
|Performance|Better|Slightly slower|
|Scope|Limited|Global|
|Control|Static|Dynamic|
|Recommended|Yes|When complex logic needed|

---

## Combining cookies, cache, and middleware

- Middleware reads cookies for auth
    
- Authenticated routes use `force-dynamic`
    
- Public routes use `force-static` or `revalidate`
    
- API routes control cache via fetch options
    

---

## Key points

- Cookies are server-only
    
- `revalidate` enables ISR
    
- `force-static` guarantees static output
    
- Middleware runs before rendering
    
- Use `matcher` for scoped middleware
    
- Use conditionals for fine-grained control