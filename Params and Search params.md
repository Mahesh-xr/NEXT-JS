

---

# `params` and `searchParams` in Next.js (App Router)

In the Next.js **App Router**, route information is accessed mainly through:

- **`params`** → for **dynamic route segments**
    
- **`searchParams`** → for **query string values**
    

---

## 1. What is `params`?

`params` contains values extracted from **dynamic route segments** defined using square brackets (`[ ]`) in the folder structure.

### Example Route

```
/products/10
```

### Folder Structure

```
app/
 └── products/
     └── [productId]/
         └── page.tsx
```

Here:

- `[productId]` is a dynamic segment
    
- `params.productId` will be `"10"`
    

---

## 2. What is `searchParams`?

`searchParams` contains values from the **query string** in the URL.

### Example URL

```
/products/10?sort=price&order=asc
```

Here:

- `sort = "price"`
    
- `order = "asc"`
    

These values are **not part of the route path**, but part of the URL query.

---

## 3. `params` vs `searchParams`

|Feature|`params`|`searchParams`|
|---|---|---|
|Source|Dynamic route segments|Query string|
|URL example|`/products/10`|`?sort=price`|
|Defined by|Folder names `[id]`|URL parameters|
|Type|`string` or `string[]`|`string \| string[] \| undefined`|
|SEO friendly|Yes|Less important for SEO|

---

## 4. Using `params` and `searchParams` in a **Server Component**

By default, **`page.tsx` is a Server Component**.

### Example

```
/products/10?sort=price
```

### Code: `app/products/[productId]/page.tsx`

```tsx
interface PageProps {
  params: { productId: string };
  searchParams: { sort?: string };
}

export default function ProductPage({ params, searchParams }: PageProps) {
  const { productId } = params;
  const { sort } = searchParams;

  return (
    <div>
      <h1>Product ID: {productId}</h1>
      <p>Sort by: {sort}</p>
    </div>
  );
}
```

### Explanation

- `params.productId` → comes from `[productId]`
    
- `searchParams.sort` → comes from `?sort=price`
    
- No hooks are needed in Server Components
    

---

## 5. Using `params` in a **Client Component**

Client Components **cannot directly receive `params` automatically**.  
You must either:

- Pass `params` from a Server Component
    
- Or read values from the URL using hooks
    

---

### Option 1: Pass `params` from Server → Client (Recommended)

#### Server Component

```tsx
import ProductClient from "./ProductClient";

export default function Page({ params }: { params: { productId: string } }) {
  return <ProductClient productId={params.productId} />;
}
```

#### Client Component

```tsx
"use client";

export default function ProductClient({ productId }: { productId: string }) {
  return <h1>Product ID: {productId}</h1>;
}
```

---

### Option 2: Access Route Params Using `useParams()` (Client)

```tsx
"use client";

import { useParams } from "next/navigation";

export default function ProductClient() {
  const params = useParams();
  const productId = params.productId as string;

  return <h1>Product ID: {productId}</h1>;
}
```

---

## 6. Using `searchParams` in a **Client Component**

Use the `useSearchParams()` hook.

```tsx
"use client";

import { useSearchParams } from "next/navigation";

export default function FilterComponent() {
  const searchParams = useSearchParams();
  const sort = searchParams.get("sort");

  return <p>Sort by: {sort}</p>;
}
```

---

## 7. Summary Table

|Scenario|How to Access|
|---|---|
|Dynamic route value (Server)|`params.id`|
|Query string (Server)|`searchParams.key`|
|Dynamic route value (Client)|`useParams()`|
|Query string (Client)|`useSearchParams()`|
|Best practice|Read in Server, pass to Client|

---

## 8. Key Takeaways

- `params` comes from **folder structure**
    
- `searchParams` comes from **URL query string**
    
- Server Components can access both directly
    
- Client Components must use hooks or receive data via props
    
- Prefer Server Components for data fetching and routing logic
    

---

