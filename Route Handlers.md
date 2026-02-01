Route handlers are used in Next.js to build **backend APIs** inside the `app` directory.  
They replace `pages/api/*` and are built on top of the **Web Fetch API**.

---

Route handler basics

- File name must be `route.ts` or `route.js`
    
- Placed inside a route segment
    
- Each HTTP method is exported as a function
    
- Runs on the server only
    

---

Folder structure

```
app/
 ├── api/
 │    └── users/
 │         └── route.ts
```

The endpoint becomes:

```
/api/users
```

---

Supported HTTP methods

- `GET`
    
- `POST`
    
- `PUT`
    
- `PATCH`
    
- `DELETE`
    
- `HEAD`
    
- `OPTIONS`
    

Each method is optional.  
If a method is not exported, it returns **405 Method Not Allowed**.

---

GET – read data

```ts
export async function GET() {
  return Response.json({
    message: "Users fetched successfully",
  });
}
```

Triggered by:

```
GET /api/users
```

---

POST – create data

```ts
export async function POST(request: Request) {
  const body = await request.json();

  return Response.json(
    {
      message: "User created",
      data: body,
    },
    { status: 201 }
  );
}
```

Triggered by:

```
POST /api/users
```

---

PUT – replace entire resource

```ts
export async function PUT(request: Request) {
  const body = await request.json();

  return Response.json({
    message: "User updated completely",
    data: body,
  });
}
```

Used when the full object is replaced.

---

PATCH – update partial data

```ts
export async function PATCH(request: Request) {
  const body = await request.json();

  return Response.json({
    message: "User updated partially",
    data: body,
  });
}
```

Used for partial updates (preferred in most APIs).

---

DELETE – remove data

```ts
export async function DELETE() {
  return Response.json({
    message: "User deleted",
  });
}
```

Triggered by:

```
DELETE /api/users
```

---

HEAD – metadata only (no response body)

```ts
export async function HEAD() {
  return new Response(null, {
    headers: {
      "X-App-Status": "OK",
    },
  });
}
```

- Same as GET
    
- No response body
    
- Used for health checks, caching, metadata
    

---

OPTIONS – allowed methods (CORS / preflight)

```ts
export async function OPTIONS() {
  return new Response(null, {
    headers: {
      Allow: "GET, POST, PUT, PATCH, DELETE, HEAD, OPTIONS",
    },
  });
}
```

Used by browsers for:

- CORS preflight
    
- Capability discovery
    

---

Accessing headers

```ts
export async function GET(request: Request) {
  const auth = request.headers.get("authorization");

  return Response.json({ auth });
}
```

---

Dynamic route handler example

```
app/api/users/[id]/route.ts
```

```ts
export async function GET(
  request: Request,
  { params }: { params: { id: string } }
) {
  return Response.json({
    userId: params.id,
  });
}
```

Endpoint:

```
GET /api/users/123
```

---

Key rules

- Route handlers use **Web Fetch API**
    
- No `req`, `res` like Express
    
- One file handles multiple HTTP methods
    
- Secure by default (server-only)
    
- Ideal for REST APIs, auth, webhooks
    

---

Summary

- `route.ts` defines backend endpoints
    
- Methods map directly to HTTP verbs
    
- `GET` → read
    
- `POST` → create
    
- `PUT` → replace
    
- `PATCH` → partial update
    
- `DELETE` → remove
    
- `HEAD` → headers only
    
- `OPTIONS` → allowed methods / CORS
    

This is the recommended API pattern in modern Next.js applications.