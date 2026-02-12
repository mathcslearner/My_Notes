# Next Rendering and Data Fetching

As a reminder, here are the differences between server and client components in Next.js:

- Server components are the default kind of components. They can directly access databases, backend APIs, filesystem, etc. The downside is that, as the name indicates, you cannot use many of the interactive "client-side" features such as useState, useEffect, event handlers, etc. Server components are mainly used for data fetching and performance.

- Client components are components which run in the browser. You must include "use client" at the top. With these, you can use client-side features from React such as useState, useEffect, etc. The idea is to only use client components when you need the user to interact with it.

One reason to use server components for data fetching is that it is optimized using caching when in server components. That is, when we run

```js
await fetch(...)
```

the result is cached, which makes performance better. If we want to control how the data is cached, we can use revalidation, which triggers a background update once in a while using Next. Here's the syntax:

```js
await fetch(url, {next: {revalidate: 60}})
```

There are two different kinds of component rendering in Next.js, static vs dynamic:

- Static Rendering, also known as SSG, is when the component is generated at build time or during revalidation. This is triggered when we don't have dynamic data, or we are using cached fetch. The pros of this is that it is fast and great for SEO.

- Dynamic Rendering, also known as SSR, is when the component is generated on every request. This is usually triggered when we have dynamic data, or we are not caching the fetch result. The pros of this is that it is always fresh data, but the cons is that it is slower than static rendering.

Next.js pages can render in a streaming format, where instead of waiting for all the data to come in, the server sends HTML in chunks and the page loads progressively based on what data it currently has. This leads to better user experience.

To render pages in a streaming format, either use a loading.tsx page or use Suspense. Here's an example of how to use a Suspense component to make the UI stream progressively.

```js
import { Suspense } from "react"
import Posts from "./Posts"

export default function Page() {
  return (
    <>
      <h1>Dashboard</h1>

      <Suspense fallback={<p>Loading posts...</p>}>
        <Posts />
      </Suspense>
    </>
  )
}
```
