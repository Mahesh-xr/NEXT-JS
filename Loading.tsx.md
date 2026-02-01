
# `loading.tsx` in Next.js (App Router)

## 1. What is `loading.tsx`?

`loading.tsx` is a special file in the **Next.js App Router** that defines a **loading UI** for a route segment while **server components are fetching data or awaiting async operations**.

It is automatically shown when:

- A page is waiting for data (`fetch`, database calls, async functions)
    
- A route segment is being streamed
    

---

## 2. Where `loading.tsx` Is Placed

```
app/
 ├── products/
 │    ├── loading.tsx
 │    ├── page.tsx
 │    └── [id]/
 │         ├── loading.tsx
 │         └── page.tsx
```

- `loading.tsx` applies to the **same folder level**
    
- Nearest `loading.tsx` is used automatically
    

---

## 3. Basic Example of `loading.tsx`

```tsx
export default function Loading() {
  return <p>Loading...</p>;
}
```

No imports or configuration are required.

---

## 4. How `loading.tsx` Works Internally

Next.js uses **React Suspense** under the hood.

- When a component **awaits a Promise**
    
- React pauses rendering
    
- `loading.tsx` is displayed
    
- Once the Promise resolves, the page replaces the loading UI
    

---

# Promises: `resolve` and `reject`

## 5. What is a Promise?

A **Promise** represents a value that will be available **in the future**.

A Promise can be in one of three states:

- **Pending**
    
- **Fulfilled (resolved)**
    
- **Rejected**
    

---

## 6. Creating a Promise

```ts
const myPromise = new Promise((resolve, reject) => {
  const success = true;

  if (success) {
    resolve("Data loaded successfully");
  } else {
    reject("Failed to load data");
  }
});
```

---

## 7. Using `resolve`

```ts
myPromise.then((result) => {
  console.log(result);
});
```

- `resolve(value)` → Promise is fulfilled
    
- `.then()` receives the value
    

---

## 8. Using `reject`

```ts
myPromise.catch((error) => {
  console.error(error);
});
```

- `reject(error)` → Promise is rejected
    
- `.catch()` receives the error
    

---

## 9. Promises with `async / await`

```ts
async function fetchData() {
  try {
    const result = await myPromise;
    console.log(result);
  } catch (error) {
    console.error(error);
  }
}
```

- `await` waits for `resolve`
    
- `catch` handles `reject`
    

---

# How Promises Relate to `loading.tsx`

In Next.js Server Components:

```tsx
export default async function Page() {
  const data = await fetch("https://api.example.com/data");
  return <div>{data}</div>;
}
```

- `fetch()` returns a Promise
    
- While it is **pending**, `loading.tsx` is shown
    
- When resolved, the page renders
    

---

# Benefits of `loading.tsx`

## 10. Benefits (Detailed)

### 1. Instant User Feedback

- Displays loading UI immediately when navigating
    
- Users know their action worked
    

### 2. Improves Perceived Performance

- App feels faster even if data takes time
    
- Reduces frustration during slow network calls
    

### 3. Keeps Shared Layouts Interactive

- Navigation bars, sidebars, and headers remain usable
    
- Only the loading segment is blocked
    

### 4. Automatic Code Splitting

- Loads only the required route
    
- Reduces initial bundle size
    

### 5. No Manual State Handling

- No need for `useState` or `useEffect`
    
- Next.js manages loading automatically
    

### 6. Built-in Suspense Support

- Uses React Suspense without manual setup
    
- Cleaner and more maintainable code
    

---

## 11. Summary Table

|Feature|Description|
|---|---|
|File name|`loading.tsx`|
|Purpose|Display loading UI during async operations|
|Trigger|Pending Promise|
|State handling|Automatic|
|Scope|Route segment|
|User experience|Faster and smoother|

---

## 12. Key Takeaways

- `loading.tsx` is shown while **Promises are pending**
    
- It works automatically with **Server Components**
    
- It improves UX without extra code
    
- Shared layouts remain interactive
    
- Promises (`resolve` / `reject`) control when loading ends
    

---

This makes `loading.tsx` a core feature for building **responsive, user-friendly Next.js applications**, especially when working with async data and APIs.