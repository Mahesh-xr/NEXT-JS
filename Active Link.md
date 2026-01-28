
### **Explanation**

In a Next.js app, navigation between pages is handled using the `<Link>` component from `next/link`.  
However, sometimes you want to **highlight the active link** (the one corresponding to the current route).

To do that, you can:

1. Use the `usePathname()` hook from **next/navigation**.
    
2. Compare the current path with the link’s path.
    
3. Apply a special CSS class (e.g., `text-blue-500 font-bold`) when the paths match.
    

---

### **Example Code**

#### **ActiveLink.tsx**

```tsx
"use client";

import Link from "next/link";
import { usePathname } from "next/navigation";

interface ActiveLinkProps {
  href: string;
  label: string;
}

export default function ActiveLink({ href, label }: ActiveLinkProps) {
  const pathname = usePathname();

  const isActive = pathname === href;

  return (
    <Link
      href={href}
      className={`px-4 py-2 rounded-md transition-colors duration-200 ${
        isActive
          ? "text-white bg-blue-600 font-semibold"
          : "text-gray-700 hover:text-blue-600"
      }`}
    >
      {label}
    </Link>
  );
}
```

---

#### **Usage Example (in Navbar.tsx)**

```tsx
import ActiveLink from "./ActiveLink";

export default function Navbar() {
  return (
    <nav className="flex space-x-4 p-4 bg-gray-100 shadow-md">
      <ActiveLink href="/" label="Home" />
      <ActiveLink href="/about" label="About" />
      <ActiveLink href="/contact" label="Contact" />
    </nav>
  );
}
```

---

### **Explanation of Code**

- `usePathname()` — gets the current URL path.
    
- `isActive` — checks if the current path equals the link’s href.
    
- If active → apply blue background (`bg-blue-600`) and white text.
    
- If not active → gray text that turns blue when hovered.
    
