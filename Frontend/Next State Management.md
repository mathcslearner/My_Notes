# Next State Management

## State

In Next.js, there are two main types of state, client state and server state. This is intimately related to client/server components.

Client state lives in the browser. We can access/modify client state using typical React functionality such as useState, useContext, Zustand, etc. This is used for UI toggles, form input values, temporary interaction, etc. This can only be used in client components.

```js
"use client"
import { useState } from "react"

export default function Counter() {
  const [count, setCount] = useState(0)
  return <button onClick={() => setCount(count + 1)}>{count}</button>
}
```

 Server state lives in the server, alongside the database and APIs. In server components, we can fetch server state directly using fetch(). For example, we might want to use it for authentication, or API responses.

```js
async function getPosts() {
  const res = await fetch("https://api.example.com/posts")
  return res.json()
}

export default async function Page() {
  const posts = await getPosts()
  return <div>{posts.length}</div>
}
```

## Server Actions

Forms are handled in Next.js using Server Actions. We can think of Server Actions as async backend functions which run on the server, but are called directly from components without using an API route. For example, it might look like:

```js
export async function createPost(formData: FormData) {
  const title = formData.get("title")

  await db.post.create({
    data: { title: String(title) }
  })
}
```

and in the form we can use it as such:

```js
import { createPost } from "./actions"

export default function Page() {
  return (
    <form action={createPost}>
      <input name="title" />
      <button type="submit">Create</button>
    </form>
  )
}
```

Then when we click on the button, the action is automatically executed in the server. If we want the Server Action to return data which could be used as state in our component, we can use the useFormState() function. It is useful for example for handling validation. Here's an example:

```js
export async function createUser(prevState: any, formData: FormData) {
  const email = formData.get("email")

  if (!email) {
    return { error: "Email is required" }
  }

  await db.user.create({ data: { email: String(email) } })

  return { success: true }
}
```

and 

```js
"use client"
import { useFormState } from "react-dom"
import { createUser } from "./actions"

export default function Form() {
  const [state, formAction] = useFormState(createUser, null)

  return (
    <form action={formAction}>
      <input name="email" />
      {state?.error && <p>{state.error}</p>}
      <button type="submit">Submit</button>
    </form>
  )
}
```

Here, we adjust the component based on whether the input caused an error in the server or not.

Here's a summary of how the data flows with and without Server Actions, showcasing how they are simpler.

- Without: Client -> fetch -> API route -> DB -> return JSON -> update state
- With: Form -> Server Action -> DB -> revalidate -> UI updates

A mutation is any operation that changes data, for example create, update, delete, etc. In Next.js, mutations typically happen via Server Actions or route handlers.

Next caches fetches by default. After a mutation, it is important to revalidate the components.
## Cookies and Headers

There are special helpers on the server which can handle cookies and headers. These only work in server components.

To read cookies, we can do

```js
import { cookies } from "next/headers"

export async function getUser() {
  const cookieStore = cookies()
  const token = cookieStore.get("token")
}
```

To set cookies, we can do

```js
"use server"
import { cookies } from "next/headers"

export async function login() {
  cookies().set("token", "abc123")
}
```

To read headers, we can do

```js
import { headers } from "next/headers"

const headerList = headers()
const userAgent = headerList.get("user-agent")
```

To make the app feel instant, we can use optimistic updates to update the UI before the server confirms. Here's an example:

```js
"use client"
import { useOptimistic } from "react"
import { addTodo } from "./actions"

export default function TodoList({ todos }) {
  const [optimisticTodos, addOptimisticTodo] =
    useOptimistic(todos, (state, newTodo) => [...state, newTodo])

  async function action(formData: FormData) {
    const title = formData.get("title")
    addOptimisticTodo({ title })

    await addTodo(formData)
  }

  return (
    <>
      <ul>
        {optimisticTodos.map((t, i) => (
          <li key={i}>{t.title}</li>
        ))}
      </ul>

      <form action={action}>
        <input name="title" />
        <button>Add</button>
      </form>
    </>
  )
}
```

The UI updates immediately while the Server Action is running behind the scenes to process the data.
