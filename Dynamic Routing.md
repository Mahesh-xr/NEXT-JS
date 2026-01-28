## What is Dynamic Routing in Next.js

Dynamic routing in Next.js allows you to create pages that depend on parameters in the URL.  
For example:

```
/products/1
/products/2
/products/3
```

All these routes can be handled by a single file using a dynamic segment.

Example structure:

```
app/products/[productId]/page.js
```

Here, `[productId]` is a **dynamic route segment**, meaning that when you visit `/products/15`, the value of `productId` will be `"15"`.

---

## Folder Structure Example

```
app/
 ├── products/
 │    ├── [productId]/
 │    │     └── page.js
 │    └── page.js
 ├── layout.js
 └── page.js
```

- `/products` → Displays the list of products.
    
- `/products/10` → Displays the product with ID 10.
    

---

## Code Explanation

```javascript
import React from 'react'

export default async function page({ params }: { params: Promise<{ productId: string }> }) {
  var productId = (await params).productId;

  return (
    <div>
      <p>Product id {productId}</p>
    </div>
  )
}
```

### Explanation

1. The file path `/app/products/[productId]/page.js` tells Next.js to treat `[productId]` as a dynamic URL segment.
    
2. When a user visits `/products/25`, Next.js automatically passes:
    
    ```js
    params = { productId: '25' }
    ```
    
3. You then extract the parameter using:
    
    ```js
    (await params).productId
    ```
    
4. The page displays:
    
    ```
    Product id 25
    ```
    

---

## Recommended Correction

You do not need to use a `Promise` type for `params`.  
The standard version should be written as:

```javascript
export default function Page({ params }: { params: { productId: string } }) {
  return (
    <div>
      <p>Product id {params.productId}</p>
    </div>
  )
}
```

This is the preferred approach for dynamic routes in the Next.js App Router.

---

## Example with Data Fetching

A practical example using `fetch` to retrieve product data:

```javascript
export default async function Page({ params }: { params: { productId: string } }) {
  const res = await fetch(`https://fakestoreapi.com/products/${params.productId}`);
  const product = await res.json();

  return (
    <div>
      <h1>{product.title}</h1>
      <p>{product.description}</p>
      <p>Price: ${product.price}</p>
    </div>
  );
}
```

Visiting `/products/3` will fetch data for the product with ID 3 and render it dynamically.

---

## Key Points

|Concept|Description|
|---|---|
|`[productId]`|Dynamic folder segment capturing the route parameter|
|`params`|Object containing route parameters|
|`page.js`|File that renders the route content|
|`async function`|Enables data fetching before rendering|
|`params.productId`|Accesses the value from the URL|
