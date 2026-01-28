
## 1. What is Route Grouping?

In **Next.js (App Router)**, _route grouping_ lets you organize routes **without affecting the URL structure**. 
You can group related routes inside parentheses `( )` — this is especially useful for authentication, dashboards, or admin sections.

The folder name inside parentheses **does not appear in the actual URL**.

---

### Example (Auth Section)

Folder structure:

```
app/
 ├── (auth)/
 │    ├── login/
 │    │     └── page.tsx
 │    ├── signup/
 │    │     └── page.tsx
 │    └── forgot-password/
 │          └── page.tsx
 ├── layout.tsx
 └── page.tsx
```

---

## 2. URL Mapping

Even though all your auth-related files are inside `(auth)`,  
the URLs will look like this:

|Folder Path|Actual URL|
|---|---|
|`app/(auth)/login/page.tsx`|`/login`|
|`app/(auth)/signup/page.tsx`|`/signup`|
|`app/(auth)/forgot-password/page.tsx`|`/forgot-password`|

So the **folder name `(auth)`** is only for _organization_ — it does not appear in the path.

---

## 3. Why Use Route Grouping?

|Benefit|Description|
|---|---|
|**Organize related routes**|Keeps authentication pages separated from the rest of your app.|
|**Separate layouts**|Each group can have its own `layout.tsx` file (for different designs).|
|**Clean URLs**|Grouping does not affect the final URL.|
|**Easier maintenance**|Keeps large apps more readable and structured.|

---

## 4. Example with Custom Layout for Auth Routes

You can define a `layout.tsx` inside the `(auth)` folder to apply a specific layout (like a centered login form) for all auth pages.

```
app/
 ├── (auth)/
 │    ├── layout.tsx
 │    ├── login/
 │    │     └── page.tsx
 │    ├── signup/
 │    │     └── page.tsx
 │    └── forgot-password/
 │          └── page.tsx
 ├── (main)/
 │    ├── dashboard/
 │    │     └── page.tsx
 │    ├── profile/
 │    │     └── page.tsx
 │    └── layout.tsx
 ├── layout.tsx
 └── page.tsx
```

### `app/(auth)/layout.tsx`

```tsx
export default function AuthLayout({ children }: { children: React.ReactNode }) {
  return (
    <div style={{ display: "flex", justifyContent: "center", alignItems: "center", height: "100vh" }}>
      <div style={{ width: "400px", padding: "20px", border: "1px solid #ccc", borderRadius: "10px" }}>
        { children } 
      </div>
    </div>
  );
}
```

All auth pages (`/login`, `/signup`, `/forgot-password`) will share this layout automatically.

---

## 5. Example of a Login Page

`app/(auth)/login/page.tsx`

```tsx
export default function LoginPage() {
  return (
    <div>
      <h1>Login</h1>
      <form>
        <input type="email" placeholder="Email" /><br /><br />
        <input type="password" placeholder="Password" /><br /><br />
        <button type="submit">Login</button>
      </form>
    </div>
  );
}
```

---

## 6. Example of a Signup Page

`app/(auth)/signup/page.tsx`

```tsx
export default function SignupPage() {
  return (
    <div>
      <h1>Sign Up</h1>
      <form>
        <input type="text" placeholder="Name" /><br /><br />
        <input type="email" placeholder="Email" /><br /><br />
        <input type="password" placeholder="Password" /><br /><br />
        <button type="submit">Create Account</button>
      </form>
    </div>
  );
}
```

---

## 7. Example of a Forgot Password Page

`app/(auth)/forgot-password/page.tsx`

```tsx
export default function ForgotPasswordPage() {
  return (
    <div>
      <h1>Forgot Password</h1>
      <form>
        <input type="email" placeholder="Enter your registered email" /><br /><br />
        <button type="submit">Send Reset Link</button>
      </form>
    </div>
  );
}
```

---

## 8. Summary

| Concept            | Description                                                 |
| ------------------ | ----------------------------------------------------------- |
| `(auth)`           | A route group folder (does not appear in the URL)           |
| `layout.tsx`       | Shared layout for all pages in that group                   |
| `/login`           | Handled by `app/(auth)/login/page.tsx`                      |
| `/signup`          | Handled by `app/(auth)/signup/page.tsx`                     |
| `/forgot-password` | Handled by `app/(auth)/forgot-password/page.tsx`            |
| Purpose            | To group related pages and give them a unique design/layout |

---

By using **route grouping**, you can separate your authentication UI (login/signup/forgot password) from the rest of your site while still keeping clean URLs like `/login` and `/signup`.