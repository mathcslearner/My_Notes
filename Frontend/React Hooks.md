React hooks are special functions which allow you to use React features inside function components. They help keep related logic together.

Hooks can only be called at the top level: that is, it shouldn't be in the return part of the component.

Here are the most commonly used React hooks:

useState():

Components might rely on certain data. For example, for a component on the price of an item, the data is the price. Therefore, if that data changes, we would want the component to re-render. To do so, we use state variables to store and update local component data. The syntax is 

```JSX
const [price, setPrice] = useState(0)
```

useEffect():

Often times there might be code we want to run after rendering, for example fetching data through an API or subscribing to events. Since these are effects, we want to separate them cleanly using this hook. The syntax is

```JSX
useEffect(() => {
	setup()
}, [])
```

The first argument is called the setup function, which says what is the side effect we want to run after the render. We can optionally return a **cleanup function**, which undoes the effect the next time the setup function runs. This is important to ensure the effect is not duplicated.

The second argument is called the dependency array, which states when the setup function should be run. For example, \[props] would mean it runs whenever the data in props changes.

useContext():

This hook is used to access global data. If there's data which many components need and which won't change, it's quite complicated to pass down the data at each step. By storing global shared data in Context, we can make it simpler to access it. The syntax:

Create a context:

```JSX
const ThemeContext = React.createContext();
```

Provide the context to components:

```JSX
<ThemeContext.Provider value="dark">
	<App />
</ThemeContext.Provider>
```

Consume the context:

```JSX
const theme = useContext(ThemeContext)
```

useMemo():

When the component re-renders, all the functions inside re-run, which can be expensive computationally, especially if the values didn't change. useMemo allows you to memoize the computed value so that it is cached between renders. Note there is memory overhead so it's best to use this for really expensive logic.

```JSX
const memoizedValue = useMemo(() => {
	return expensiveCalculation(data);
}, [data])
```

Just like useEffect, the second argument is a dependency array which states when the value should be recomputed.

Custom hooks:

We can build custom hooks to share reusable logic across components:

```JSX
function useAuth() {
	const [user, setUser] = useState(null);
	return {user, setUser};
}
```

