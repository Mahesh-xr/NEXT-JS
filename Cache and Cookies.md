

## Cookies

Cookies are handled on the **server** using helpers from `next/headers`.

### Reading cookies (Server Components / Route Handlers)

```ts
import { cookies } from "next/headers";

export default function Page() {
  const cookieStore = cookies();
  const token = cookieStore.get("token")?.value;

  return <p>Token: {token}</p>;
}
```

---

### Setting cookies (Route Handlers)

```ts
import { cookies } from "next/headers";

export async function POST() {
  cookies().set("token", "abc123", {
    httpOnly: true,
    path: "/",
  });

  return Response.json({ success: true });
}
```

---

### Deleting cookies

```ts
cookies().delete("token");
```

---

## Caching in Next.js

Next.js caches **data fetches and pages** by default when possible.

---

## `revalidate` (Incremental Static Regeneration)

Used to **revalidate cached data after a time interval**.

### Page-level revalidation

```ts
export const revalidate = 60;
```

- Page is cached
    
- Revalidated every 60 seconds
    
- New request after 60s triggers regeneration
    

---

### Fetch-level revalidation

```ts
await fetch("https://api.example.com/posts", {
  next: { revalidate: 30 },
});
```

- Only this fetch is revalidated
    
- Other data remains cached
    

---

### Revalidation behavior

- Cached response is served immediately
    
- Revalidation happens in the background
    
- New data replaces old cache
    

---

## `force-static`

Forces a route to be **fully static**, even if dynamic APIs are used.

```ts
export const dynamic = "force-static";
```

Behavior:

- Page is generated at build time
    
- No per-request rendering
    
- Cookies, headers, auth logic not allowed
    

Used for:

- Marketing pages
    
- Documentation
    
- Blog posts
    

---

## Disabling cache (dynamic rendering)

```ts
export const dynamic = "force-dynamic";
```

Or at fetch level:

```ts
await fetch(url, { cache: "no-store" });
```

Used for:

- User-specific data
    
- Auth-based pages
    
- Dashboards
    

---

## Cache control summary

|Option|Behavior|
|---|---|
|`revalidate = n`|Rebuild after n seconds|
|`force-static`|Build-time only|
|`force-dynamic`|Every request|
|`cache: "no-store"`|No caching|

---

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