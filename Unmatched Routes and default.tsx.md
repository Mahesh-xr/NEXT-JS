
Unmatched routes occur in **parallel routes** when a slot does not have a matching route for the current URL.  
Next.js handles this situation using a special file called **`default.tsx`**.

---

Unmatched routes in parallel routing

In parallel routes, multiple slots are rendered at the same time.  
However, not every navigation includes content for every slot.

If a slot does **not** have a matching page for a given route, that slot becomes **unmatched**.

Without handling this, the slot would render nothing or cause layout issues.

---

What is `default.tsx`?

`default.tsx` provides **fallback UI** for a parallel route slot when no matching route exists.

It ensures the slot:

- Still renders something
    
- Keeps the layout stable
    
- Does not break the UI
    

---

Folder structure example

```
app/
 ├── dashboard/
 │    ├── layout.tsx
 │    ├── page.tsx
 │    ├── @users/
 │    │    ├── page.tsx
 │    │    └── default.tsx
 │    ├── @analytics/
 │    │    ├── page.tsx
 │    │    └── default.tsx
 │    └── @notifications/
 │         ├── page.tsx
 │         └── default.tsx
```

---

Dashboard layout

```tsx
export default function DashboardLayout({
  users,
  analytics,
  notifications,
}: {
  users: React.ReactNode;
  analytics: React.ReactNode;
  notifications: React.ReactNode;
}) {
  return (
    <div>
      <section>{users}</section>
      <section>{analytics}</section>
      <section>{notifications}</section>
    </div>
  );
}
```

---

Example scenario

Route visited:

```
/dashboard
```

All slots match:

- `@users/page.tsx`
    
- `@analytics/page.tsx`
    
- `@notifications/page.tsx`
    

---

Another route:

```
/dashboard/users
```

If:

- `@analytics/page.tsx` does not exist for this route
    
- `@notifications/page.tsx` does not exist for this route
    

Then:

- `default.tsx` inside those slots is rendered instead
    

---

Example `default.tsx`

`app/dashboard/@analytics/default.tsx`

```tsx
export default function AnalyticsDefault() {
  return <p>No analytics selected</p>;
}
```

`app/dashboard/@notifications/default.tsx`

```tsx
export default function NotificationsDefault() {
  return <p>No notifications available</p>;
}
```

---

How Next.js decides what to render

For each parallel slot:

1. If a matching route exists → render `page.tsx`
    
2. If no matching route exists → render `default.tsx`
    
3. If `default.tsx` is missing → render nothing
    

---

Why `default.tsx` is important

- Prevents empty or broken layouts
    
- Provides meaningful fallback UI
    
- Keeps parallel layouts consistent
    
- Improves user experience
    
- Required for predictable parallel routing behavior
    

---

Key points

- Unmatched routes occur only in **parallel routes**
    
- `default.tsx` is a fallback for missing slot routes
    
- It applies per slot, not globally
    
- It keeps layouts stable when routes do not match


## Conditional Route

we can also use the condition route to show the different content while maintaining the same url

```tsx

export default function DashboardLayout({
  users,
  analytics,
  notifications,
  login
}: {
  users: React.ReactNode;
  analytics: React.ReactNode;
  notifications: React.ReactNode;
}) {
  const isLoggedIn = false;
  return isLoggedIn ? (
    <div>
      <section>{users}</section>
      <section>{analytics}</section>
      <section>{notifications}</section>
    </div> : login;
  );
}```