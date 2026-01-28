In **Next.js (App Router)**, a file named **`layout.tsx`** defines the **UI structure** that wraps around your pages.  
It acts like a **template** that stays consistent for all child routes inside a folder.

Let’s go step by step.

---

## 1. What is `layout.tsx`?

A `layout.tsx` file in Next.js is used to **share common UI** across multiple pages (like a navigation bar, sidebar, or footer).  
Every folder under the `app/` directory can have its own `layout.tsx`, and it applies to **all routes inside that folder**.

---

## 2. Basic Example

### Folder structure

```
app/
 ├── layout.tsx
 ├── page.tsx
 ├── about/
 │    ├── page.tsx
 └── contact/
      ├── page.tsx
```

### Code: `app/layout.tsx`

```tsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>
        <header>My Website Header</header>
        <main>{ children }</main>
        <footer>© 2025 My Website</footer>
      </body>
    </html>
  );
}
```

- `children` represents the **page component** inside that layout.
    
- The `<header>` and `<footer>` remain the same for all pages (Home, About, Contact).
    
- Only the `<main>` content changes.
    

---

## 3. Nested Layouts

You can have multiple layouts for different route groups.

### Example structure:

```
app/
 ├── layout.tsx            → Root layout
 ├── page.tsx              → Home page
 ├── auth/
 │    ├── layout.tsx       → Auth layout (for login/signup)
 │    ├── login/
 │    │    └── page.tsx
 │    ├── signup/
 │    │    └── page.tsx
 │    └── forgot-password/
 │         └── page.tsx
```

### Code: `app/auth/layout.tsx`

```tsx
export default function AuthLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <section style={{ padding: "40px", textAlign: "center" }}>
      <h1>Authentication Portal</h1>
      <div>{children}</div>
    </section>
  );
}
```

Now:

- `/auth/login`
    
- `/auth/signup`
    
- `/auth/forgot-password`
    

All share this **auth layout**, separate from the main website layout.

---

## 4. Layout Hierarchy

When nested, layouts are **stacked**.

Example:

```
Root layout → Auth layout → Login page
```

This means:

- The **root layout** (header, footer, etc.) wraps the **auth layout**.
    
- The **auth layout** wraps the **login page**.
    

---

## 5. Advantages

|Benefit|Explanation|
|---|---|
|**Reusability**|Shared UI (navbar, sidebar, footer) only written once.|
|**Code Organization**|Each route group can have its own layout.|
|**Better Performance**|Layouts persist between route changes (don’t re-render fully).|
|**Nested Structure**|Supports multi-level layouts (e.g., Admin layout, Auth layout, etc.).|

---

## 6. Summary

|Concept|Description|
|---|---|
|`layout.tsx`|Defines shared UI for routes in that folder.|
|`children`|Represents the page content inside the layout.|
|Nested Layouts|You can have multiple levels of layouts.|
|Example Use|Global header/footer, dashboard layout, auth layout, etc.|

---

### Example Final Output

Visiting `/auth/login` renders:

```
<html>
  <body>
    <header>My Website Header</header>
    <section>
      <h1>Authentication Portal</h1>
      <div>Login Page Content</div>
    </section>
    <footer>© 2025 My Website</footer>
  </body>
</html>
```

