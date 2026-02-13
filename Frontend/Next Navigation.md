# Next Navigation

Navigation is basically just how users move through the app. In Next.js, navigation is client-side by default. It is fast because there is no full page reload.

For client-side navigation between routes, we can use the <Link /> component, like this:

```js
import Link from "next/link"

<Link href="/dashboard">Go to Dashboard</Link>
```

This prevents full page reload, prefetches routes automatically and makes navigation feel instant. We can add props such as prefetch (preloads page in background), scroll, etc.

useRouter() is a client hook for programmatic navigation. We can use it for example after form submission, conditional redirects, navigation after login/logout, etc.. Here's an example of how to use it:

```js
"use client";
import { useRouter } from "next/navigation";

const router = useRouter();

router.push("/dashboard");
router.replace("/login");
router.back();
router.refresh();
```

We can see that push() adds to history, replace() replaces the current history entry, back() goes back, and refresh() re-fetches server components.

To obtain the current route pathname, we can use the usePathname() function. This can be helpful if we want to condition the layout based on which pathname the webpage is at. To obtain the current query parameters, we can use useSearchParams(). For example:

```js
"use client";
import { useSearchParams } from "next/navigation";

const searchParams = useSearchParams();
const id = searchParams.get("id");
```

When the URL is /products?id=123, id=123.

In server components or route handlers, we can automatically redirect the user using the redirect() function. For example, we might want to redirect the user immediately after authentication.

If we have an invalid dynamic route, we can trigger the 404 page using notFound(), which will render the app/not-found.jsx page.

We can render multiple pages in the same layout by using parallel routes. For example, the folder could look like

```
app/
 ├─ @team/
 ├─ @analytics/
 └─ layout.js
```

and our layout.js page could look like

```js
export default function Layout({ team, analytics }) {
  return (
    <>
      {team}
      {analytics}
    </>
  );
}
```

We can use parallel routes for dashboards, split-screen layouts, side-by-side panels, modals that persist alongside content, etc.
