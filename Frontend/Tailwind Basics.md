# Tailwind Basics

Tailwind is a utility-first CSS framework for frontend. You apply style directly in the HTML using small, single-purpose classes. This means you don't need to write out CSS separately, which greatly simplifies things as the number of pages increases.

Here's a basic example:

```HTML
<button class="bg-blue-500 text-white px-4 py-2 rounded">
  Click me
</button>
```

We can see from the example that each class does one thing, for example padding, centering, color, etc. Another thing we note is that Tailwind allows us to directly see what styles are applied to each div, rather than have to switch between tabs.

Here are some common Tailwind classes(although most of the time just searching it up is enough):

- Layout: flex, grid, flex-row, flex-col, items-center, justify-between, gap-4

- Spacing: p-4, x-6, m-2, mt-4

- Sizing: w-full, h-screen, max-w-md

- Typography: text-sm, font-bold, text-gray-700, leading-relaxed

- Colors: bg-blue-500, text-red-600, border-gray-300

- Responsiveness: sm, md, lg for screen sizes

- For states, use prefixes like hover: , focus: , active:

- Border: rounded, border, shadow

- Dark mode: dark: prefix

If we really want to have a deeper level of CSS customization, we can modify the config file (tailwind.config.js). For example:

```JS
theme: {
  extend: {
    colors: {
      brand: '#1e40af'
    }
  }
}
```

Finally, I will conclude by giving an example of a card component built in traditional HTML + CSS vs using Tailwind.

Version 1 (HTML + CSS):

```HTML
<div class="card">
  <h2 class="card-title">Card Title</h2>
  <p class="card-text">
    This is a simple card built with HTML and CSS.
  </p>
  <button class="card-btn">Learn More</button>
</div>
```

```CSS
.card {
  background-color: white;
  padding: 16px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  width: 300px;
}

.card-title {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 8px;
}

.card-text {
  color: #555;
  margin-bottom: 12px;
}

.card-btn {
  background-color: #3b82f6;
  color: white;
  padding: 8px 16px;
  border-radius: 6px;
  border: none;
  cursor: pointer;
}

.card-btn:hover {
  background-color: #2563eb;
}
```

Version 2 (HTML + Tailwind):

```HTML
<div class="bg-white p-4 rounded-lg shadow-md w-72">
  <h2 class="text-xl font-bold mb-2">Card Title</h2>
  <p class="text-gray-600 mb-3">
    This is a simple card built with Tailwind.
  </p>
  <button class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-md">
    Learn More
  </button>
</div>
```

We can see that it is much less cumbersome to use Tailwind.
