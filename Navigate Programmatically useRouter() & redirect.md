
# `useRouter` and Navigation in Next.js (App Router)

Next.js provides two main ways to navigate **programmatically** (without clicking a `<Link>`):

1. **`useRouter()`** → Client-side navigation
    
2. **`redirect()`** → Server-side redirection
    

Both are imported from **`next/navigation`**.

---

## 1. What is `useRouter()`?

`useRouter()` is a **Client Component hook** that allows navigation through code (for example, after form submission or button click).

### Import

```ts
import { useRouter } from "next/navigation";
```

### Important

- Can be used **only in Client Components**
    
- Requires `"use client"` at the top
    

---

## 2. Basic Programmatic Navigation (`router.push`)

### Example: Navigate after button click

```tsx
"use client";

import { useRouter } from "next/navigation";

export default function Home() {
  const router = useRouter();

  const goToProducts = () => {
    router.push("/products");
  };

  return <button onClick={goToProducts}>Go to Products</button>;
}
```

### Explanation

- `router.push("/products")` → navigates to `/products`
    
- Adds a new entry to browser history
    

---

## 3. `router.replace()` (Replace Current Page)

### Example

```tsx
router.replace("/login");
```

### Difference Between `push` and `replace`

|Method|Behavior|
|---|---|
|`push()`|Adds new history entry|
|`replace()`|Replaces current entry|

### Use Case

After login, users **should not go back** to the login page:

```tsx
router.replace("/dashboard");
```

---

## 4. Backward and Forward Navigation

### Go Back

```tsx
router.back();
```

### Go Forward

```tsx
router.forward();
```

These behave like browser back/forward buttons.

---

## 5. Navigating with Query Parameters

```tsx
router.push("/products?sort=price&order=asc");
```

or using object syntax:

```tsx
router.push({
  pathname: "/products",
  query: { sort: "price", order: "asc" }
});
```

---

## 6. Using `redirect()` (Server Components)

`redirect()` is used in **Server Components**, **Route Handlers**, or **Server Actions**.

### Import

```ts
import { redirect } from "next/navigation";
```

---

### Example: Redirect in Server Component

```tsx
import { redirect } from "next/navigation";

export default function Dashboard({ user }: { user: any }) {
  if (!user) {
    redirect("/login");
  }

  return <h1>Dashboard</h1>;
}
```

### Important Notes

- `redirect()` stops execution immediately
    
- Works only on the server
    
- Does not require `"use client"`
    

---

## 7. `redirect()` vs `router.replace()`

|Feature|`redirect()`|`router.replace()`|
|---|---|---|
|Environment|Server|Client|
|Hook required|No|Yes|
|Stops execution|Yes|No|
|Use case|Auth, permissions|UI-based navigation|

---

## 8. Example: Login Flow (Client + Server)

### Client Login Page

```tsx
"use client";

import { useRouter } from "next/navigation";

export default function Login() {
  const router = useRouter();

  const handleLogin = () => {
    // authentication logic
    router.replace("/dashboard");
  };

  return <button onClick={handleLogin}>Login</button>;
}
```

### Server Protected Page

```tsx
import { redirect } from "next/navigation";

export default function Dashboard({ isAuth }: { isAuth: boolean }) {
  if (!isAuth) {
    redirect("/login");
  }

  return <h1>Dashboard</h1>;
}
```

---

## 9. Summary Table

|Feature|Usage|
|---|---|
|`useRouter()`|Client-side navigation|
|`router.push()`|Navigate and keep history|
|`router.replace()`|Navigate and remove history|
|`router.back()`|Go to previous page|
|`router.forward()`|Go to next page|
|`redirect()`|Server-side redirect|

---

## 10. Key Takeaways

- Use **`<Link>`** for normal navigation
    
- Use **`useRouter()`** for navigation triggered by logic or events
    
- Use **`redirect()`** for server-side access control
    
- Prefer **Server Components** for authentication and routing decisions
    

