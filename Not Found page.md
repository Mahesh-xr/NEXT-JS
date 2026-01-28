
## 1. What is a `not-found` page in Next.js?

In Next.js (App Router), a file named `not-found.tsx` or `not-found.jsx` defines **what to show when a page or data cannot be found** — similar to a 404 page.

Example:

```
app/
 ├── products/
 │    ├── [productId]/
 │    │     ├── reviews/
 │    │     │     ├── [reviewId]/
 │    │     │     │     ├── page.tsx
 │    │     │     │     └── not-found.tsx
 │    │     │     └── page.tsx
 │    │     └── page.tsx
 │    └── page.tsx
 ├── layout.tsx
 └── page.tsx
 └── not-found.tsx
```

Here:

- If the URL `/products/10/reviews/999` is invalid or data doesn’t exist,  
    the **`not-found.tsx`** inside `[reviewId]` will render.
    

---

## 2. Basic Example of `not-found.tsx`

```tsx

export default function NotFound() {
  return (
    <h1>Review not found for this product.</h1>
  );
}

```

If you want a **specific not-found message** that depends on the URL (for example, showing which product/review is missing), you can use the **`usePathname()`** hook.

---

## 3. Using `usePathname()` for Dynamic Route 404

Here’s the correct way to write your code:

```tsx

"use client";

import { usePathname } from "next/navigation";

export default function NotFound() {
  const pathname = usePathname();

  // Example pathname: /products/5/reviews/10
  const parts = pathname.split("/"); // ["", "products", "5", "reviews", "10"]

  const productId = parts[2];
  const reviewId = parts[4];

  return (
    <div>
      <h1>
        Review {reviewId} not found for product {productId}
      </h1>
    </div>
  );
}
```

---

## 4. Explanation

1. `usePathname()` gives you the **current URL path**.
    
    ```js
    const pathname = "/products/5/reviews/10";
    ```
    
2. `pathname.split("/")` splits the path into segments:
    
    ```
    ["", "products", "5", "reviews", "10"]
    ```
    
3. `parts[2]` → productId (`5`)  
    `parts[4]` → reviewId (`10`)
    
4. Then you can display a meaningful not-found message.
    

---

## 5. Example Behavior

|URL|Output|
|---|---|
|`/products/7/reviews/15`|Review 15 not found for product 7|
|`/products/2/reviews/99`|Review 99 not found for product 2|

---

## 6. Triggering `not-found.tsx` Automatically

Inside your `page.tsx`, if you detect missing data, you can **manually trigger** the not-found page using `notFound()` from `next/navigation`:

```tsx
import { notFound } from "next/navigation";

export default async function Page({ params }: { params: { productId: string; reviewId: string } }) {
  const { productId, reviewId } = params;

  const res = await fetch(`https://fakestoreapi.com/products/${productId}`);
  if (!res.ok) {
    notFound(); // Redirects to not-found.tsx
  }

  const product = await res.json();
  const review = null; // assume missing review
  if (!review) {
    notFound(); // Triggers your custom not-found message
  }

  return <div>Review {reviewId} for product {productId}</div>;
}
```

---

## Summary

| Concept               | Description                                                           |
| --------------------- | --------------------------------------------------------------------- |
| `not-found.tsx`       | A page rendered when a route or data is not found                     |
| `usePathname()`       | Retrieves the current URL path                                        |
| `pathname.split("/")` | Splits the URL to extract IDs                                         |
| `notFound()`          | Function to programmatically trigger the not-found page               |
| Placement             | The nearest `not-found.tsx` in the folder hierarchy handles the error |
|                       |                                                                       |

---

This way, you can display a **custom, dynamic 404 message** that reflects which product or review was not found, providing a better user experience.