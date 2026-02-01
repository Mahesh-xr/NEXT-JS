Query parameters and headers are handled in **route handlers** and **pages** using the **Web Fetch API**.  
They are available on the server and allow you to control request data and response metadata.

---

Query parameters handling

Query parameters are values passed after `?` in the URL.

Example URL:

```
/api/comments?postId=10&sort=latest
```

---

Accessing query parameters in route handlers

```ts
export async function GET(request: Request) {
  const { searchParams } = new URL(request.url);

  const postId = searchParams.get("postId");
  const sort = searchParams.get("sort");

  return Response.json({
    postId,
    sort,
  });
}
```

- `searchParams.get()` → single value
    
- Returns `string | null`
    

---

Handling multiple query values

```
/api/comments?tag=nextjs&tag=react
```

```ts
const tags = searchParams.getAll("tag");
```

---

Query parameters in Server Components (pages)

URL:

```
/products?category=books
```

```tsx
export default function ProductsPage({
  searchParams,
}: {
  searchParams: { category?: string };
}) {
  return <h1>Category: {searchParams.category}</h1>;
}
```

---

Query parameters in Client Components

```tsx
"use client";

import { useSearchParams } from "next/navigation";

export default function Filter() {
  const searchParams = useSearchParams();
  const sort = searchParams.get("sort");

  return <p>Sort: {sort}</p>;
}
```

---

Request header handling

Request headers contain metadata such as authentication tokens, content type, and client info.

---

Accessing request headers in route handlers

```ts
export async function GET(request: Request) {
  const auth = request.headers.get("authorization");
  const contentType = request.headers.get("content-type");

  return Response.json({
    auth,
    contentType,
  });
}
```

---

Accessing headers using `headers()` helper (Server Components)

```ts
import { headers } from "next/headers";

export default function Page() {
  const headerList = headers();
  const userAgent = headerList.get("user-agent");

  return <p>User Agent: {userAgent}</p>;
}
```

---

Response header handling

Response headers are sent back to the client along with the response.

---

Setting response headers in route handlers

```ts
export async function GET() {
  return new Response(JSON.stringify({ message: "OK" }), {
    headers: {
      "Content-Type": "application/json",
      "X-App-Version": "1.0",
    },
  });
}
```

---

Setting headers using `Response.json`

```ts
export async function GET() {
  return Response.json(
    { status: "success" },
    {
      headers: {
        "X-Source": "comments-api",
      },
    }
  );
}
```

---

Common response headers

- `Content-Type`
    
- `Cache-Control`
    
- `Set-Cookie`
    
- Custom headers (`X-*`)
    

---

Setting cookies (response header)

```ts
import { cookies } from "next/headers";

export async function GET() {
  cookies().set("token", "abc123");

  return Response.json({ success: true });
}
```

---

Key points

- Query parameters come from the URL
    
- Headers carry metadata
    
- Request headers are read-only
    
- Response headers are configurable
    
- Uses standard Web APIs
    
- Works in route handlers and server components

## ==Next Request==


Next.js provides **`NextRequest`** as an enhanced version of the standard `Request` object.  
It makes working with **query parameters**, **headers**, and **cookies** easier and more structured.

---

Using `NextRequest` to read query parameters

`NextRequest` exposes `nextUrl`, which contains parsed URL data.

Example URL:

```
/api/comments?postId=10&sort=latest
```

---

Route handler using `NextRequest`

```ts
import { NextRequest } from "next/server";

export async function GET(request: NextRequest) {
  const postId = request.nextUrl.searchParams.get("postId");
  const sort = request.nextUrl.searchParams.get("sort");

  return Response.json({
    postId,
    sort,
  });
}
```

- `request.nextUrl.searchParams` is a `URLSearchParams` object
    
- Cleaner than manually creating `new URL(request.url)`
    

---

Multiple query values with `NextRequest`

URL:

```
/api/comments?tag=nextjs&tag=react
```

```ts
const tags = request.nextUrl.searchParams.getAll("tag");
```

---

Dynamic route + query params together

```
/api/comments/5?expand=true
```

```ts
import { NextRequest } from "next/server";

export async function GET(
  request: NextRequest,
  { params }: { params: { id: string } }
) {
  const expand = request.nextUrl.searchParams.get("expand");

  return Response.json({
    id: params.id,
    expand,
  });
}
```

---

Reading request headers with `NextRequest`

```ts
export async function GET(request: NextRequest) {
  const auth = request.headers.get("authorization");
  const userAgent = request.headers.get("user-agent");

  return Response.json({
    auth,
    userAgent,
  });
}
```

---

Setting response headers

```ts
export async function GET() {
  return Response.json(
    { message: "Success" },
    {
      headers: {
        "X-API-Version": "1.0",
      },
    }
  );
}
```

---

Using cookies with `NextRequest`

```ts
import { NextRequest } from "next/server";

export async function GET(request: NextRequest) {
  const token = request.cookies.get("token")?.value;

  return Response.json({ token });
}
```

---

When to use `NextRequest`

- When reading query parameters frequently
    
- When working with middleware-style logic
    
- When handling cookies and headers together
    
- When you want structured URL access
    

---

Key points

- `NextRequest` extends the Web `Request`
    
- `nextUrl.searchParams` simplifies query handling
    
- Works only in route handlers and middleware
    
- Ideal for clean and readable server code