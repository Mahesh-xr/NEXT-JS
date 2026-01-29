Parallel routes allow multiple route segments to be rendered **at the same time** within the same layout.  
They are useful when a page needs to show **independent sections** that load and update separately.

Parallel routes are defined using **named slots** prefixed with `@`.

---

Parallel routes example: Dashboard with Users, Analytics, and Notifications

Folder structure:

```
app/
 ├── dashboard/
 │    ├── layout.tsx
 │    ├── page.tsx
 │    ├── @users/
 │    │    └── page.tsx
 │    ├── @analytics/
 │    │    └── page.tsx
 │    └── @notifications/
 │         └── page.tsx
```

- `@users`, `@analytics`, and `@notifications` are **parallel route slots**
    
- All three render simultaneously inside the dashboard layout
    

---

Dashboard layout using slots:

```tsx
export default function DashboardLayout({
  children,
  users,
  analytics,
  notifications,
}: {
  children: React.ReactNode;
  users: React.ReactNode;
  analytics: React.ReactNode;
  notifications: React.ReactNode;
}) {
  return (
    <div>
      <h1>Dashboard</h1>

      <div style={{ display: "grid", gridTemplateColumns: "2fr 1fr" }}>
        <section>{users}</section>
        <section>{notifications}</section>
      </div>

      <section>{analytics}</section>

      {children}
    </div>
  );
}
```

- Each slot is received as a prop
    
- Slots render independently but share the same layout
    

---

Users slot:

`app/dashboard/@users/page.tsx`

```tsx
export default function Users() {
  return <div>Users List</div>;
}
```

---

Analytics slot:

`app/dashboard/@analytics/page.tsx`

```tsx
export default function Analytics() {
  return <div>Analytics Data</div>;
}
```

---

Notifications slot:

`app/dashboard/@notifications/page.tsx`

```tsx
export default function Notifications() {
  return <div>Notifications Panel</div>;
}
```

---

How it works:

- Visiting `/dashboard` renders:
    
    - Users
        
    - Analytics
        
    - Notifications
        
- Each section is independent
    
- One slot can load or error without affecting others
    

---

Benefits of parallel routes:

- Independent loading states per section
    
- Independent error handling per section
    
- Better performance and responsiveness
    
- Complex layouts without deeply nested routes
    
- Clear separation of concerns
    

---

Example with loading per slot:

```
app/dashboard/@analytics/loading.tsx
```

```tsx
export default function Loading() {
  return <p>Loading analytics...</p>;
}
```

Only the analytics section shows loading while others remain interactive.

---

Summary:

- Parallel routes render multiple route segments together
    
- Defined using `@slotName`
    
- Slots are injected into layouts as props
    
- Ideal for dashboards with multiple independent panels