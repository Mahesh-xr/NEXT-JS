

```
/products/1/reviews/10
```


## Folder Structure (App Router)

Your project structure should look like this:

```
app/
 ├── products/
 │    ├── [productId]/
 │    │     ├── reviews/
 │    │     │     ├── [reviewId]/
 │    │     │     │     └── page.tsx
 │    │     │     └── page.tsx
 │    │     └── page.tsx
 │    └── page.tsx
 ├── layout.tsx
 └── page.tsx
```

Explanation:

- `/products` → Lists all products.
    
- `/products/[productId]` → Shows a specific product.
    
- `/products/[productId]/reviews` → Shows all reviews for that product.
    
- `/products/[productId]/reviews/[reviewId]` → Shows a specific review for that product.
    

---

## Correct Code for `/products/[productId]/reviews/[reviewId]/page.tsx`

```tsx
export default async function ProductReview({
  params,
}: {
  params: { productId: string; reviewId: string };
}) {
  const { productId, reviewId } = params;

  return (
    <div>
      <h1>Review {reviewId} for product {productId}</h1>
    </div>
  );
}
```

---

## Explanation

1. **Dynamic Folders:**
    
    - `[productId]` → captures product ID from the URL.
        
    - `[reviewId]` → captures review ID from the URL.
        
2. **Params Object:**  
    Next.js automatically provides the `params` object based on your folder names.
    
    ```js
    params = {
      productId: '1',
      reviewId: '10'
    }
    ```
    
3. **TypeScript Typing:**  
    The `params` object type is explicitly defined:
    
    ```tsx
    { params: { productId: string; reviewId: string } }
    ```
    
4. **Rendering Output:**  
    If you visit `/products/5/reviews/2`, the page will display:
    
    ```
    Review 2 for product 5
    ```
    

---

## Optional Example with Data Fetching

You can also fetch data dynamically based on both parameters:

```tsx
export default async function ProductReview({
  params,
}: {
  params: { productId: string; reviewId: string };
}) {
  const { productId, reviewId } = params;

  const response = await fetch(
    `https://fakestoreapi.com/products/${productId}`
  );
  const product = await response.json();

  return (
    <div>
      <h1>{product.title}</h1>
      <p>Showing Review {reviewId} for this product.</p>
    </div>
  );
}
```

---

## Summary

|Concept|Description|
|---|---|
|`[productId]`|Dynamic route for product|
|`[reviewId]`|Nested dynamic route for review|
|`params`|Contains both dynamic values|
|`page.tsx`|File that renders route content|
|`fetch`|Can be used to load API data dynamically|

---

This setup allows you to handle nested routes like `/products/1/reviews/3` easily, with clean and organized folder structures.**