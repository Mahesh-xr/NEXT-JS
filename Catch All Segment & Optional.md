
## 1. Concept: Catch-all Segment

A **catch-all segment** in Next.js allows you to capture **multiple dynamic parts** of a URL into an **array**

There are two types:

| Type                   | Syntax        | Example URL                                       | `params` value                                   |
| ---------------------- | ------------- | ------------------------------------------------- | ------------------------------------------------ |
| **Catch-all**          | `[...slug]`   | `/docs/feature/concept`                           | `{ slug: ["feature", "concept"] }`               |
| **Optional Catch-all** | `[(...slug)]` | `/docs`, `/docs/feature`, `/docs/feature/concept` | `{ slug: undefined }` or `{ slug: ["feature"] }` |

folder name:

```
app/docs/[(...slug)]/page.tsx
```

means the route can match:

- `/docs`
    
- `/docs/feature`
    
- `/docs/feature/concept`
    
- `/docs/anything/else/...`
    

---

## 2. Correct TypeScript Code

Here’s a clean and working version of your code:

```tsx

export default async function Docs({
  params,
}: {
  params: { slug?: string[] };
}) {
  const { slug } = params;

  if (slug?.length === 2) {
    return (
      <h1>
        Viewing docs for feature "{slug[0]}" and concept "{slug[1]}"
      </h1>
    );
  } else if (slug?.length === 1) {
    return <h1>Viewing docs for feature "{slug[0]}"</h1>;
  } else {
    return <h1>Docs home page</h1>;
  }
}

```

---

## 3. Explanation

1. The folder name `[(...slug)]` tells Next.js this route is **optional catch-all**.
    
    - `slug` might be **undefined**, **one value**, or **multiple values**.
        
2. The `params.slug` is automatically an **array** of strings.
    
3. You can check its length to determine which kind of page to display.
    

---

## 4. Example URLs and Outputs

|URL|`params.slug`|Output|
|---|---|---|
|`/docs`|`undefined`|Docs home page|
|`/docs/react`|`["react"]`|Viewing docs for feature "react"|
|`/docs/react/hooks`|`["react", "hooks"]`|Viewing docs for feature "react" and concept "hooks"|

---

## 5. Summary

|Concept|Description|
|---|---|
|`[...slug]`|Catch-all segment (requires at least one segment)|
|`[(...slug)]`|Optional catch-all segment (can be empty)|
|`params.slug`|Array of path segments|
|`slug?.length`|Used to check how many segments exist|
|Example|`/docs/react/hooks` → `slug = ["react", "hooks"]`|
