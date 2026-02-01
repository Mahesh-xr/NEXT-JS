Intercepting routes allow you to **load a route inside the current layout instead of navigating away**, while the **URL changes**.  
They are commonly used for **modals, previews, overlays**, and **context-preserving navigation**.

---

Intercepting routes concept

- A route is intercepted and rendered in the **current layout**
    
- The browser URL updates
    
- Closing the intercepted UI returns to the previous view
    
- The page underneath remains mounted
    

---

Intercepting routes conventions

Next.js uses special folder naming conventions to decide **which route to intercept**.

---

`(.)` — same level interception

Intercepts a route at the **same segment level**.

Folder structure:

```
app/
 ├── feed/
 │    ├── page.tsx
 │    └── (.)photo/
 │         └── page.tsx
```

Behavior:

- From `/feed`, navigating to `/feed/photo`
    
- `photo` renders **inside feed layout**
    
- Feed page stays visible
    

---

`(..)` — one level above interception

Intercepts a route **one level above** the current segment.

```
app/
 ├── feed/
 │    ├── page.tsx
 │    └── (..)profile/
 │         └── page.tsx
 ├── profile/
 │    └── page.tsx
```

Behavior:

- From `/feed`, navigating to `/profile`
    
- Profile renders inside feed layout
    

---

`(..)(..)` — two levels above interception

Intercepts a route **two levels above**.

```
app/
 ├── dashboard/
 │    ├── settings/
 │    │    ├── page.tsx
 │    │    └── (..)(..)help/
 │    │         └── page.tsx
 ├── help/
 │    └── page.tsx
```

Behavior:

- From `/dashboard/settings`, navigating to `/help`
    
- Help renders inside settings layout
    

---

`(e..)` — intercept from root (`app/`)

Intercepts a route starting from the **root app directory**, regardless of nesting.

```
app/
 ├── dashboard/
 │    ├── page.tsx
 │    └── (e..)login/
 │         └── page.tsx
 ├── login/
 │    └── page.tsx
```

Behavior:

- From `/dashboard`, navigating to `/login`
    
- Login renders inside dashboard layout
    

---

Modal example (common use case)

```
app/
 ├── feed/
 │    ├── page.tsx
 │    ├── @modal/
 │    │    └── (.)photo/
 │    │         └── page.tsx
 │    └── photo/
 │         └── page.tsx
```

- `/feed/photo` normally navigates to photo page
    
- When intercepted, photo renders as a modal over feed
    
- Closing modal returns to feed without reload
    

---

How Next.js decides interception

1. Navigation happens
    
2. Next.js checks for intercepting route folders
    
3. If match found → render intercepted version
    
4. If not → perform normal navigation
    

---

Key points

- Intercepting routes preserve the current UI
    
- URL updates without losing context
    
- `(.)`, `(..)`, `(..)(..)`, `(e..)` define interception scope
    
- Often combined with parallel routes for modals
    
- Ideal for overlays, previews, and drill-down views