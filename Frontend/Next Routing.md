# Next Routing

These notes will mainly be for the App Router. The app/ directory is the router for the overall application. The folder structure is the URL structure. In other words, the filesystem is the routing configuration.

In app/, here are some common files:

```
app/
  layout.tsx
  page.tsx
  loading.tsx
  error.tsx
  not-found.tsx
```

Let's say we have the file app/dashboard/settings/page.tsx, then the URL would just be /dashboard/settings. We see here that we are nesting the routes.

Here's what the different files do:

- pages.tsx defines the actual UI for the page
- layout.tsx defines a persistent UI across all routes, and it wraps all child routes. This is used for sidebar, page header, etc. where we might want to keep the same one for all pages in a particular section, for example. This does not re-render on navigation.
- template.tsx is the same as layout.tsx, but re-renders

The next 3 files give you automatic loading, error, and 404 states per route segment. They are automatically toggled by Next.

- loading.tsx: This is what is shown while a route is loading. Just like layout.tsx, it wraps all child routes as well. Typically we might put a spinner or a skeleton screen.
- error.tsx: This is what is shown when there is a runtime error for rendering, data fetching, or any other effects. Note this must be a client component. Typically we might put an error message or a retry button.
- not-found.tsx: This is what is shown when the route called does not exist. Let's say there are invalid dynamic route params, or unauthorized access. Then we trigger not found and show a 404 code.

We can also set up dynamic routes, for example some page might be slightly different for every product we have in our application. To do so, we can insert a square bracket \[] to indicate the dynamic segment.

For example: app/posts/\[id]/page.tsx

id becomes a route param which is accessible via props

We can also create catch-all routes which match for all routes of a certain format: app/docs/\[...slug]/page.tsx would match /docs/a, /docs/b, etc.

We can use route groups to organize files without affecting URLS. To do so, use parentheses: (). For example, we might have the following two folder routes and URL routes:

- app/(auth)/login/page.tsx -> /login
- app/(auth)/register/page.tsx -> /register

The idea is that we can group these two pages which have related functionalities to make it easier for us to write the code and oversee the system as the developer, without showing this to the user.
