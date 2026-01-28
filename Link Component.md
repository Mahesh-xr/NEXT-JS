In **Next.js**, the **`Link`** component is used for **client-side navigation** between pages — similar to how `<a>` tags work in normal HTML, but much faster and optimized.

Let’s go through everything clearly.

---

## 1. What is `Link` in Next.js?

`Link` is a **built-in component** from Next.js used to navigate between routes **without reloading the entire page**.

It comes from: 

```tsx
import Link from "next/link";
```


When you use `<Link>`, Next.js:

- Prefetches the next page in the background (for faster navigation).
    
- Updates the URL and renders the new page **instantly**, without a full reload.
    
- Maintains the **layout state** (like keeping a navbar fixed while only changing content).
    

---

## 2. Basic Example

### Folder structure:

```
app/
 ├── page.tsx
 ├── about/
 │    └── page.tsx
```

### Code: `app/page.tsx`

```tsx
import Link from "next/link";

export default function Home() {
  return (
    <div>
      <h1>Home Page</h1>
      <Link href="/about">Go to About Page</Link>
    </div>
  );
}
```

### Code: `app/about/page.tsx`

```tsx
import Link from "next/link";

export default function About() {
  return (
    <div>
      <h1>About Page</h1>
      <Link href="/">Back to Home</Link>
    </div>
  );
}
```

✅ Output:

- Clicking on “Go to About Page” navigates to `/about` **without reloading the browser**.
    

---

## 3. Example with Dynamic Routes

If you have a dynamic route like:

```
app/products/[productId]/page.tsx
```

You can use:

```tsx 
import Link from "next/link";

export default function Products() {
  const productId = 5;
  return (
    <div>
      <h1>All Products</h1>
      <Link href={`/products/${productId}`}>View Product {productId}</Link>
    </div>
  );
}
```

This takes you to `/products/5`.

---

## 4. Opening in a New Tab

`Link` behaves like a normal `<a>` tag, so you can use:

```tsx
<Link href="/about" target="_blank">Open About in new tab</Link>
```

---

## 5. Styling a Link

You can style `<Link>` like normal text or buttons:

```tsx
<Link href="/contact" className="text-blue-500 hover:underline">
  Contact Us
</Link>
```

---

## 6. Prefetching Behavior

- By default, Next.js **automatically prefetches** linked pages when they appear in the viewport.
    
- You can **disable prefetching** if needed:
    

```tsx
<Link href="/about" prefetch={false}>About</Link>
```

---

## 7. Why Use `<Link>` Instead of `<a>`?

|Feature|`<a>` tag|`<Link>` (Next.js)|
|---|---|---|
|Page reload|Full reload|No reload (client-side navigation)|
|Speed|Slower|Faster|
|Prefetch|❌|✅ Automatically prefetches pages|
|Layout preservation|❌|✅ Layout remains intact|
|SEO friendly|✅|✅ (uses `<a>` internally)|

---

## 8. Example: Navigation Bar

```tsx
import Link from "next/link";

export default function Navbar() {
  return (
    <nav>
      <Link href="/">Home</Link> |{" "}
      <Link href="/about">About</Link> |{" "}
      <Link href="/contact">Contact</Link>
    </nav>
  );
}
```

✅ You can include this in your `layout.tsx` so it appears on every page.

---

## 9. Summary

|Concept|Description|
|---|---|
|**Import**|`import Link from "next/link";`|
|**Purpose**|For client-side navigation between pages|
|**Dynamic Routes**|Works with URLs like `/products/[id]`|
|**Prefetching**|Automatically loads next page in background|
|**Faster Navigation**|Avoids full page reloads|
|**SEO Friendly**|Uses `<a>` internally for SEO support|
