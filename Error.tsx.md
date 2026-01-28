
# `error.tsx` in Next.js (App Router)

## 1. What is `error.tsx`?

`error.tsx` is a special file in the **Next.js App Router** used to handle **runtime errors** that occur during rendering, data fetching, or server actions.

It works by automatically creating a **React Error Boundary** around a route segment.

---

## 2. Key Characteristics of `error.tsx`

### 1. Automatic Error Boundary

- Next.js automatically wraps each route segment and its children in a **React Error Boundary**
    
- You do not need to manually use `ErrorBoundary` components
    

### 2. File-System Based Error Handling

- You can define **custom error UIs** for specific routes
    
- The nearest `error.tsx` in the folder hierarchy is used
    

### 3. Error Isolation

- Errors are **isolated to the segment** where they occur
    
- The rest of the application continues working normally
    

### 4. Recovery Without Full Reload

- Allows retrying or recovering from errors
    
- Does **not require a full page reload**
    

---

## 3. Where `error.tsx` Is Placed

```
app/
 ├── error.tsx                ← Global error UI
 ├── layout.tsx
 ├── page.tsx
 ├── products/
 │    ├── error.tsx           ← Product-specific error
 │    ├── page.tsx
 │    └── [id]/
 │         ├── error.tsx      ← Product detail error
 │         └── page.tsx
```

### Resolution Rule

- Next.js uses the **closest `error.tsx`** to the failing component
    
- If none exists, it falls back to the parent segment
    

---

## 4. Basic `error.tsx` Example

```tsx
"use client";

export default function Error({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  return (
    <div>
      <h2>Something went wrong</h2>
      <p>{error.message}</p>

      <button onClick={reset}>Try again</button>
    </div>
  );
}
```

---

## 5. Explanation of Props

|Prop|Description|
|---|---|
|`error`|The error object that was thrown|
|`reset()`|Re-renders the segment and retries|

---

## 6. How Error Recovery Works

1. A runtime error occurs in a route segment
    
2. Next.js catches it using an Error Boundary
    
3. The nearest `error.tsx` is rendered
    
4. Clicking `reset()` retries rendering the segment
    
5. No full page reload happens
    

---

## 7. Component Hierarchy with `error.tsx`

### Normal Rendering Flow

```
Root Layout
 └── Segment Layout
      └── Page
```

### When an Error Occurs

```
Root Layout
 └── Segment Layout
      └── Error Boundary (error.tsx)
           └── Error UI
```

- Only the **failing segment** is replaced
    
- Parent layouts remain intact
    

---

## 8. Example Scenario

### Error thrown in `/products/[id]/page.tsx`

```tsx
throw new Error("Product not found");
```

### Rendered Tree

```
Root Layout
 └── Products Layout
      └── Product Error UI (error.tsx)
```

- Navigation, headers, and sidebars remain usable
    
- Only product detail area shows error
    

---

## 9. Error Isolation Example

|Route|Behavior|
|---|---|
|`/products/1`|Error UI shown|
|`/products`|Works normally|
|`/about`|Works normally|

---

## 10. Common Use Cases

- API failures
    
- Database connection issues
    
- Invalid route data
    
- Authorization errors
    
- Server action failures
    

---

## 11. Summary of Benefits

- Automatic React Error Boundary
    
- Segment-level error handling
    
- Custom error UI per route
    
- No full page reload required
    
- Improved resilience and UX
    

---

## 12. Key Takeaways

- `error.tsx` is **mandatory to be a Client Component**
    
- It catches runtime errors in its route segment
    
- It isolates failures without crashing the entire app
    
- `reset()` enables retry logic
    

---

## Final Component Hierarchy Summary

```
layout.tsx
 └── (route segment)
      ├── page.tsx
      ├── loading.tsx
      └── error.tsx
```

This structure allows Next.js to **stream loading states**, **handle errors gracefully**, and **keep the app responsive and stable**.