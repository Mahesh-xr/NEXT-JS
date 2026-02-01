Parallel interception routes combine **parallel routes (slots)** and **intercepting routes** to show content like a **modal** while keeping the current page mounted.

Use case:

- Base page: `/feed`
    
- Navigate to: `/feed-photo/123`
    
- Show photo **as a modal over the feed**
    
- URL changes
    
- Closing modal returns to `/feed`
    
- If modal slot is not active, nothing renders
    

---

Folder structure

```
app/
 ├── feed/
 │    ├── page.tsx
 │    └── @modal/
 │         ├── default.tsx
 │         └── (.)feed-photo/
 │              └── [id]/
 │                   └── page.tsx
 ├── feed-photo/
 │    └── [id]/
 │         └── page.tsx
```

Explanation:

- `@modal` is a parallel route slot
    
- `(.)feed-photo` intercepts a route at the same level
    
- `[id]` is the dynamic photo id
    
- `default.tsx` ensures modal slot renders nothing when inactive
    

---

Feed page

`app/feed/page.tsx`

```tsx
import Link from "next/link";

export default function FeedPage() {
  return (
    <div>
      <h1>Feed</h1>

      <Link href="/feed-photo/1">Open Photo 1</Link>
      <br />
      <Link href="/feed-photo/2">Open Photo 2</Link>
    </div>
  );
}
```

---

Modal slot default (no modal)

`app/feed/@modal/default.tsx`

```tsx
export default function ModalDefault() {
  return null;
}
```

- When no interception occurs, modal slot is empty
    
- Feed page renders normally
    

---

Intercepted modal route

`app/feed/@modal/(.)feed-photo/[id]/page.tsx`

```tsx
"use client";

import { useRouter } from "next/navigation";

export default function PhotoModal({
  params,
}: {
  params: { id: string };
}) {
  const router = useRouter();

  return (
    <div style={{ position: "fixed", inset: 0, background: "rgba(0,0,0,0.6)" }}>
      <div style={{ background: "white", margin: "10% auto", padding: 20 }}>
        <h2>Photo {params.id}</h2>

        <button onClick={() => router.back()}>
          Close
        </button>
      </div>
    </div>
  );
}
```

Behavior:

- Renders inside the `@modal` slot
    
- Feed page remains mounted
    
- `router.back()` closes modal and returns to `/feed`
    

---

Normal route (non-intercepted)

`app/feed-photo/[id]/page.tsx`

```tsx
export default function PhotoPage({
  params,
}: {
  params: { id: string };
}) {
  return <h1>Photo Page {params.id}</h1>;
}
```

Behavior:

- Direct visit to `/feed-photo/1`
    
- Full page navigation
    
- No feed or modal
    

---

How routing works

- From `/feed` → click `/feed-photo/1`
    
    - `(.)feed-photo` matches
        
    - Modal opens
        
    - URL becomes `/feed-photo/1`
        
- Direct visit `/feed-photo/1`
    
    - No interception context
        
    - Normal page renders
        

---

Why `default.tsx` is required

- Prevents empty slot errors
    
- Keeps layout stable
    
- Explicitly defines “no modal” state
    

---

Key points

- Parallel routes control **where** content renders
    
- Intercepting routes control **how** navigation behaves
    
- `(.)` intercepts same-level routes
    
- `@modal` isolates modal UI
    
- URL changes without losing context
    
- Ideal for image viewers, previews, overlays