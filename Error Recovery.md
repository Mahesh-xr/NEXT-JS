# Server-Side Error Recovery using `useRouter` and `startTransition`

In Next.js App Router, **server errors** (thrown in Server Components, data fetching, or Server Actions) are handled by **`error.tsx`**.  
Recovery can be implemented on the **client side** using:

- `useRouter()` from `next/navigation`
    
- `startTransition()` from `react`
    

---

## 1. Why Recovery Is Needed

When a server error occurs:

- Rendering stops for the affected route segment
    
- `error.tsx` is shown instead of the page
    
- Layouts above the segment remain mounted
    

Recovery allows you to:

- Retry rendering
    
- Navigate away safely
    
- Avoid a full page reload
    

---

## 2. Role of `error.tsx` in Recovery

`error.tsx` acts as a **React Error Boundary** and provides a `reset()` function.

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
      <button onClick={reset}>Try again</button>
    </div>
  );
}
```

- `reset()` re-renders the failed segment
    
- No full reload occurs
    

---

## 3. Using `useRouter()` for Recovery Navigation

Sometimes retrying is not enough (for example, invalid data or permission issues).  
In such cases, you can **navigate away from the error state**.

### Example: Redirect to a Safe Page

```tsx
"use client";

import { useRouter } from "next/navigation";

export default function Error({
  error,
}: {
  error: Error;
}) {
  const router = useRouter();

  return (
    <div>
      <h2>Failed to load data</h2>
      <p>{error.message}</p>

      <button onClick={() => router.replace("/")}>
        Go to Home
      </button>
    </div>
  );
}
```

### Why `replace()`?

- Prevents users from navigating back to the broken route
    
- Keeps navigation history clean
    

---

## 4. Why `startTransition()` Is Important

React’s `startTransition()` tells React:

> “This update is non-urgent; keep the UI responsive.”

It is recommended when:

- Retrying rendering
    
- Triggering navigation after an error
    
- Performing state updates that may cause re-rendering
    

---

## 5. Server Error Recovery using `startTransition`

### Example: Retry Rendering Safely

```tsx
"use client";

import { startTransition } from "react";

export default function Error({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  const handleRetry = () => {
    startTransition(() => {
      reset();
    });
  };

  return (
    <div>
      <h2>Server error occurred</h2>
      <p>{error.message}</p>

      <button onClick={handleRetry}>
        Retry
      </button>
    </div>
  );
}
```

### What Happens

- The error boundary retries rendering
    
- React keeps the UI responsive
    
- No blocking re-render
    

---

## 6. Combining `useRouter` + `startTransition`

This is useful when retry fails and navigation is needed.

```tsx
"use client";

import { useRouter } from "next/navigation";
import { startTransition } from "react";

export default function Error({
  error,
}: {
  error: Error;
}) {
  const router = useRouter();

  const handleRecovery = () => {
    startTransition(() => {
      router.replace("/dashboard");
    });
  };

  return (
    <div>
      <h2>Something went wrong</h2>
      <p>{error.message}</p>

      <button onClick={handleRecovery}>
        Go to Dashboard
      </button>
    </div>
  );
}
```

---

## 7. Example: Server-Side Error Trigger

### Server Component

```tsx
export default async function Page() {
  const res = await fetch("https://api.example.com/data");

  if (!res.ok) {
    throw new Error("Failed to fetch data");
  }

  const data = await res.json();
  return <div>{data.name}</div>;
}
```

- Error is thrown on the server
    
- Nearest `error.tsx` handles it
    
- Client recovery options become available
    

---

## 8. Component Hierarchy During Error

```
Root layout
 └── Route layout
      └── Error boundary (error.tsx)
           └── Error UI
```

- Only the failing segment is replaced
    
- Parent layouts remain interactive
    
- Navigation and recovery buttons still work
    

---

## 9. When to Use Each Approach

|Scenario|Recommended Action|
|---|---|
|Temporary server issue|`reset()` inside `startTransition`|
|Invalid data|`router.replace()`|
|Permission error|Redirect to login/home|
|Recoverable fetch error|Retry rendering|

---

## 10. Key Takeaways

- Server errors are caught by `error.tsx`
    
- `reset()` retries rendering without reload
    
- `useRouter()` enables safe navigation away from errors
    
- `startTransition()` ensures smooth, non-blocking recovery
    
- Layouts remain mounted during recovery
    

---

This approach gives **production-grade resilience**, allowing your Next.js app to **recover gracefully from server-side failures** while maintaining a smooth user experience.



# Why Use `startTransition()` in React / Next.js

## 1. What is `startTransition()`?

`startTransition()` is a React API used to mark certain updates as **non-urgent (low priority)**.

```ts
import { startTransition } from "react";
```

It tells React:

> “This update can happen in the background. Do not block the UI.”

---

## 2. The Problem `startTransition()` Solves

By default, **all state updates are urgent**.

If an update:

- Triggers heavy rendering
    
- Causes navigation
    
- Re-renders large components
    

React may **block the UI**, leading to:

- Frozen buttons
    
- Delayed clicks
    
- Janky navigation
    

---

## 3. What Happens Without `startTransition()`

```tsx
reset(); // or router.replace("/dashboard")
```

- React treats this as urgent
    
- UI can freeze during re-render
    
- User interaction is blocked temporarily
    

This is noticeable when:

- Retrying a failed server render
    
- Navigating after an error
    
- Re-rendering large layouts
    

---

## 4. What `startTransition()` Changes

```tsx
startTransition(() => {
  reset();
});
```

React now:

- Keeps the UI responsive
    
- Allows clicks, typing, scrolling
    
- Runs the update in the background
    

---

## 5. Why It Matters in Next.js

In Next.js:

- `reset()` re-renders a **server component tree**
    
- `router.push()` triggers **route transitions**
    
- Both can be **expensive operations**
    

Using `startTransition()` ensures:

- Smooth navigation
    
- No UI blocking
    
- Better perceived performance
    

---

## 6. Common Use Cases

### 1. Error Recovery

```tsx
startTransition(() => reset());
```

Prevents UI freeze when retrying after a server error.

---

### 2. Navigation

```tsx
startTransition(() => {
  router.replace("/dashboard");
});
```

Ensures navigation does not block the UI.

---

### 3. Large Re-renders

```tsx
startTransition(() => {
  setFilteredData(newData);
});
```

Filtering large datasets without freezing the page.

---

## 7. When NOT to Use `startTransition()`

Do **not** use it for:

- Form inputs
    
- Button hover states
    
- Typing events
    
- Immediate feedback actions
    

These must stay **urgent**.

---

## 8. Summary Table

|Without `startTransition()`|With `startTransition()`|
|---|---|
|UI may freeze|UI stays responsive|
|Updates are urgent|Updates are low priority|
|Poor UX in heavy renders|Smooth UX|
|Blocking navigation|Non-blocking navigation|

---

## 9. One-Line Summary

> **`startTransition()` improves user experience by allowing expensive updates to run in the background without blocking the UI.**

This is why it is recommended for **navigation, error recovery, and server re-renders** in modern React and Next.js applications.