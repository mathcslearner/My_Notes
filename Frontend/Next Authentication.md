# Next Authentication

Authentication is the process of verifying user identity (login with email/password, OAuth, etc.). Authorization is the process of determining the user's access level. For example, are they an admin, a simple user, an editor, etc. We can handle these in both client-side and server-side components.

## Auth Patterns

Here are some common auth patterns to use:

- Cookie-based: When the user logs in, the server sets an HTTP-only cookie. When the user sends requests, the cookie is sent automatically and the server verifies the session. This is good because it is secure and there is no token handling on client.
- JWT-based: The server returns a JWT (JSON web token), which is stored in an HTTP-only cookie. This token is sent via an Authorization header. This is good mainly for APIs and microservices.
- OAuth: You delegate the authentication to a 3rd party such as Google, Github, etc.
- Server Actions: You can validate the user session inside server actions and reject unauthorized users.

## Middlewares

Middlewares are frequently used in authentication. A middleware is essentially code that runs before a request reaches a page/API route, which allows us to intercept, modify, redirect or block the request based on the token/header sent. Here's an example:

```js
import { NextResponse } from "next/server"

export function middleware(request) {
  const token = request.cookies.get("session")

  if (!token) {
    return NextResponse.redirect(new URL("/login", request.url))
  }

  return NextResponse.next()
}

// Matcher config
export const config = {
  matcher: ["/dashboard/:path*"]
}
```

To run the middleware, we need a matcher config, which is a configuration objection telling Next.js which routes we want to run it on.

## Route Protections

Based on user type, we might want to restrict access to certain pages. This is where route protections come in. There are three main levels to protecting routes in Next.js:

- Client-side protection: We can check the session status using useSession from next-auth. This is pretty bad since it only hides the UI. It can be easily bypassed.
- Server-side protection: We can check the session status using getServerStation from next-auth. This is good because it's secure and runs before rendering, ensuring route protection.
- Middleware Protection: This is best for entire sections, role-based routing, and multi-tenant apps. The session status will be checked whenever a request is sent.

Here are examples:

```js
// Client side protection
"use client"
import { useSession } from "next-auth/react"

if (!session) return <Login />
```

and 

```js
import { getServerSession } from "next-auth"

export default async function Page() {
  const session = await getServerSession()

  if (!session) redirect("/login")
}
```

## Sessions and headers

A session usually contains an id, a role, and when it expires. It is then encrypted inside cookie. Here's an example:

```json
{
  "userId": "123",
  "role": "admin",
  "expires": "2026-01-01"
}
```

For any HTTP request, there is a header attached. A header will contain a JWT token to authorize the user, a content-type to indicate the type of data being sent, etc. This header is then used to check for authentication/authorization.
## Common libraries

Here are some common libraries for working with auth in Next:

- NextAuth.js: The features include OAuth providers, email + credentials login, JWT sessions, etc. This is nice for quick setup.
- Clerk: The features include a hosted UI, a user management dashboard, webhooks, etc. This is nice for production-level applications.
