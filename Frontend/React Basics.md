# React Basics

The core idea behind React is to create reusable UI components which can update and render independently of each other as data changes, allowing for good performance and user experience.

The building blocks of React is components, which are written as JS functions in JSX, which is an extension of HTML. We can import/export components from different files. An example of a component:

```JSX
const Greeting = () => {
	const name = 'Anna';
	return (
		<h1>Welcome, {name}!</h1>
	)
}
```

To use it, call <Greeting />. Notice that this component is not very re-usable right now since the name will always be Anna. To make this component be adaptable to different data, we can pass in data using props:

```JSX
const Greeting = ({props}) => {
	return (
		<h1>Welcome, {props.name}!</h1>
	)
}
```

Then we would call <Greeting {name: Anna} />. For code within components, just write like you would for HTML. For any component logic which uses JS functions, use {} to enclose the block. Note that in JSX, we use classname instead of class.
