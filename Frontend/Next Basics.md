# Next Basics

Recall that React is a UI library where we build components and manage state. In particular, everything runs in client-side rendering.

Next.js is a full-stack React framework which essentially gives us backend functionalities to our app, such as routing, server-side rendering, static generation, API routes, and performance optimizations.

Next.js currently has two routers. One is the Pages Router, which does file-based routing using a pages/ folder. Each file represents a route. This is kind of old.

The new router is the App Router, which uses an app/ folder. This router is built around server components, and there are strong layout, streaming, and data-fetching features.

The thing about routing is that the URL will mirror directly the folder name, for example /blog/post will be the URL for a file at /blog/post. This makes it quite convenient when you are designing your overall system.

There are two kinds of components in Next: server vs client. By default components are server-side.

- Server components run only on the server. They can access databases, read files, call private APIs, etc. They load faster and have better SEO.
- Client components run on the browser. These are used whenever we need interactivity, browser APIs, etc. We have to include "use client" for this.

Note we can render client components in server components.
