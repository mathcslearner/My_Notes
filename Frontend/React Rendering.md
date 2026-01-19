Rendering is when React calls a component function to determine what the UI should look like. Then, it commits the changes to the real DOM, updating the UI which the user sees on the screen.

When a component is first created/mounted, there is an initial render.

Then, for the following conditions, the component will re-render:

- The state changes
- The props change
- The parent component re-renders
- Context value changes

To prevent unnecessary re-renders, we can use hooks such as useMemo and useCallback which store values.

Lists are rendered by mapping data to React elements. To identify each item between renders, it is critical to use keys.

```JSX
items.map(item => (
	<Item key={item.id} data={item} />
))
```

One useful trick we can use is conditional rendering, so we can show/hide/switch components based on certain conditions. For example, there is a popup we only want to show when it was clicked. We can do 

```JSX
{isOpen && <Modal />}
```

Then, when isOpen is false, the component will not render. We can also use ternary operators for this:

```JSX
{isLoading ? <Spinner /> : <Content />}
```

