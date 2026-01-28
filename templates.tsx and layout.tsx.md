
---

# `layout.tsx` vs `template.tsx` in Next.js (App Router)

In the App Router, **both `layout.tsx` and `template.tsx` wrap pages**, but they behave **very differently** with respect to **state, inputs, and re-rendering**.

---

## 1. Core Difference (One Line)

- **`layout.tsx`** → **persists UI and state** across navigation
    
- **`template.tsx`** → **creates a fresh UI instance** on every navigation
    

This difference is critical for **forms** like login and register.

---

## 2. Auth Route Group Structure

```
app/
 ├── (auth)/
 │    ├── layout.tsx
 │    ├── template.tsx   (optional – choose one, not both)
 │    ├── login/
 │    │    └── page.tsx
 │    ├── register/
 │    │    └── page.tsx
 │    └── forgot-password/
 │         └── page.tsx
 ├── layout.tsx
 └── page.tsx
```

- `(auth)` is a **route group** (not part of the URL)
    
- URLs remain:
    
    - `/login`
        
    - `/register`
        
    - `/forgot-password`
        

---

## 3. Using `layout.tsx` (State is Preserved)

### `app/(auth)/layout.tsx`

```tsx
"use client";

import { useState } from "react";

export default function AuthLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  const [email, setEmail] = useState("");

  return (
    <div>
      <h1>Auth Layout</h1>

      <input
        type="email"
        placeholder="Email (shared)"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />

      <hr />

      {children}
    </div>
  );
}
```

### Pages

#### `login/page.tsx`

```tsx
export default function Login() {
  return <h2>Login Page</h2>;
}
```

#### `register/page.tsx`

```tsx
export default function Register() {
  return <h2>Register Page</h2>;
}
```

### Behavior

1. Type email in the input
    
2. Navigate from `/login` → `/register`
    
3. **Input value remains**
    

### Why?

- `layout.tsx` **does not unmount**
    
- React state is preserved
    
- Only `{children}` changes
    

---

## 4. Using `template.tsx` (Fresh Copy Every Time)

Now replace `layout.tsx` with `template.tsx`.

### `app/(auth)/template.tsx`

```tsx
"use client";

import { useState } from "react";

export default function AuthTemplate({
  children,
}: {
  children: React.ReactNode;
}) {
  const [email, setEmail] = useState("");

  return (
    <div>
      <h1>Auth Template</h1>

      <input
        type="email"
        placeholder="Email (resets)"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />

      <hr />

      {children}
    </div>
  );
}
```

### Behavior

1. Type email in the input
    
2. Navigate from `/login` → `/register`
    
3. **Input value resets**
    

### Why?

- `template.tsx` **remounts on every navigation**
    
- React state is destroyed
    
- New instance is created
    

---

## 5. Key Technical Difference

|Feature|`layout.tsx`|`template.tsx`|
|---|---|---|
|Remounted on navigation|No|Yes|
|State preserved|Yes|No|
|Input values preserved|Yes|No|
|New React tree created|No|Yes|
|Use case|Shared UI|Fresh UI per route|

---

## 6. When to Use `layout.tsx`

Use **`layout.tsx`** when:

- You want **form inputs preserved**
    
- You want shared state across pages
    
- You want persistent UI (auth shells, dashboards)
    

### Common Uses

- Auth flows (login → register)
    
- Dashboards
    
- Sidebars, headers
    
- Multi-step forms
    

---

## 7. When to Use `template.tsx`

Use **`template.tsx`** when:

- You want a **clean reset on every route**
    
- State must not leak between pages
    

### Common Uses

- Documentation pages
    
- Wizard steps that must restart
    
- Pages with isolated animations
    
- SEO-driven static content
    

---

## 8. Important Rule

> **Do not use `layout.tsx` and `template.tsx` together in the same folder.**

- If both exist, `template.tsx` takes precedence for remount behavior
    
- Choose **one**, based on state needs
    

---

## 9. Real Auth Recommendation

For **auth pages (login, register, forgot password)**:

**Use `layout.tsx`**

Reason:

- Preserves input values
    
- Better user experience
    
- Allows shared validation, branding, and form state
    

---

## Final Summary

- `layout.tsx` → persistent wrapper, state preserved
    
- `template.tsx` → fresh wrapper, state reset
    
- Auth flows should use **layout**
    
- Stateless pages can use **template**
    
- Route groups `(auth)` help organize without changing URLs
    

This distinction is critical for building correct **form-based applications** in Next.js.