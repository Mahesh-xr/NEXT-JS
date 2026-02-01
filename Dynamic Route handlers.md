Dynamic route handlers allow you to build **REST-style APIs** where part of the URL is dynamic (for example, a comment ID).  
They are implemented using **`route.ts`** with dynamic folders like `[id]`.

---

Dynamic route handler concept

- URL contains a variable segment
    
- That value is accessed using `params`
    
- Same handler supports multiple HTTP methods
    
- Runs only on the server
    

---

Use case: Comments API

Operations:

- View a comment
    
- Add a comment
    
- Update a comment
    
- Delete a comment
    

---

Folder structure

```
app/
 └── api/
      └── comments/
           ├── route.ts
           └── [id]/
                └── route.ts
```

---

Base route: `/api/comments`

Used for **adding** and **listing** comments.

---

GET – view all comments

`app/api/comments/route.ts`

```ts
export async function GET() {
  return Response.json([
    { id: 1, text: "First comment" },
    { id: 2, text: "Second comment" },
  ]);
}
```

---

POST – add a new comment

```ts
export async function POST(request: Request) {
  const body = await request.json();

  return Response.json(
    {
      message: "Comment added",
      comment: body,
    },
    { status: 201 }
  );
}
```

Triggered by:

```
POST /api/comments
```

---

Dynamic route: `/api/comments/[id]`

Used for **view**, **update**, and **delete** a specific comment.

---

GET – view a single comment

`app/api/comments/[id]/route.ts`

```ts
export async function GET(
  request: Request,
  { params }: { params: { id: string } }
) {
  return Response.json({
    id: params.id,
    text: `Comment ${params.id}`,
  });
}
```

Triggered by:

```
GET /api/comments/1
```

---

PATCH – update a comment (partial update)

```ts
export async function PATCH(
  request: Request,
  { params }: { params: { id: string } }
) {
  const body = await request.json();

  return Response.json({
    message: "Comment updated",
    id: params.id,
    updatedData: body,
  });
}
```

Triggered by:

```
PATCH /api/comments/1
```

---

DELETE – delete a comment

```ts
export async function DELETE(
  request: Request,
  { params }: { params: { id: string } }
) {
  return Response.json({
    message: "Comment deleted",
    id: params.id,
  });
}
```

Triggered by:

```
DELETE /api/comments/1
```

---

How `params` works

- `[id]` captures the dynamic value from the URL
    
- `/api/comments/5` → `params.id = "5"`
    
- Available in all HTTP methods
    

---

Key points

- `route.ts` defines backend endpoints
    
- `[id]` enables dynamic URLs
    
- One file can handle multiple HTTP methods
    
- Uses Web Fetch API (`Request`, `Response`)
    
- Ideal for CRUD operations
    

---

Summary

- `/api/comments`
    
    - `GET` → list comments
        
    - `POST` → add comment
        
- `/api/comments/[id]`
    
    - `GET` → view comment
        
    - `PATCH` → update comment
        
    - `DELETE` → delete comment
        

This pattern is the standard way to build **dynamic REST APIs** in Next.js App Router.