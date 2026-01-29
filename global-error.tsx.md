## What is `global-error.tsx`?

`global-error.tsx` is a **special error-handling file** in Next.js used to catch **errors that occur at the highest level of the application**, especially errors that **cannot be caught by normal `error.tsx` boundaries**.

It acts as the **final fallback error UI** for the entire app.

---

## Why `global-error.tsx` is Needed

Normally, errors are handled by:

- `error.tsx` inside a route segment
    
- Error boundaries wrapping pages and layouts
    

However, there is a limitation:

- Errors occurring inside **`layout.tsx` of a segment** cannot be caught by that same segment’s `error.tsx`
    
- The **root layout** has **no parent segment**
    
- So there is **no higher error boundary** to catch root-level failures
    

This is where `global-error.tsx` comes in.

---

## Purpose of `global-error.tsx`

- Handles **catastrophic, app-level errors**
    
- Acts as the **last line of defense**
    
- Prevents the entire app from crashing silently
    
- Displays a fallback UI when recovery is not possible at lower levels
    

---

## File Location

```
app/
 ├── global-error.tsx
 ├── layout.tsx
 └── page.tsx
```

- Must be placed **directly inside the `app` directory**
    
- Only one `global-error.tsx` is allowed
    

---

## Important Characteristics

### 1. Catches Root-Level Errors

- Handles errors from:
    
    - Root `layout.tsx`
        
    - Critical framework-level failures
        
- Errors that **no other error boundary can catch**
    

---

### 2. Works Only in Production Mode

- `global-error.tsx` is **not triggered in development**
    
- In development, Next.js shows its own error overlay
    
- To test it properly, run:
    
    ```bash
    npm run build
    npm start
    ```
    

---

### 3. Requires `html` and `body` Tags

Unlike `error.tsx`, `global-error.tsx` **must render the full HTML document**.

This is required because:

- The root layout has failed
    
- Next.js cannot rely on existing layout structure
    

---

## Basic Example of `global-error.tsx`

```tsx
"use client";

export default function GlobalError({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  return (
    <html>
      <body>
        <h1>Application Error</h1>
        <p>Something went wrong at the application level.</p>
        <p>{error.message}</p>

        <button onClick={reset}>Try again</button>
      </body>
    </html>
  );
}
```

---

## Explanation of Props

|Prop|Description|
|---|---|
|`error`|The error that caused the failure|
|`reset()`|Attempts to re-render the entire app|

---

## Component Hierarchy When `global-error.tsx` Triggers

```
Application Root
 └── global-error.tsx
```

- No layouts or pages are rendered
    
- Entire app is replaced by the global error UI
    

---

## Comparison: `error.tsx` vs `global-error.tsx`

|Feature|`error.tsx`|`global-error.tsx`|
|---|---|---|
|Scope|Route segment|Entire app|
|Placement|Any route folder|Root `app/` only|
|Catches layout errors|No (same segment)|Yes (root)|
|Requires html/body|No|Yes|
|Works in dev|Yes|No|

---

## Key Takeaways

- `global-error.tsx` is for **unrecoverable, root-level errors**
    
- It is the **final fallback** error UI
    
- Must be placed in the root `app/` directory
    
- Requires `html` and `body` tags
    
- Only active in **production mode**
