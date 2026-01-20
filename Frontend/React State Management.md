Talked about this a bit in [[React Hooks]], but here is more detail about state. As said before, state is essentially data which changes over time, and which affects what the UI renders. When state changes, React re-renders the component.

The first type of state is **local state**, which exists in a single component. It is managed with useState. For example, form inputs, toggle states and local counters. 

One important thing to note about useState is to never mutate it directly. Rather, create a new object. For example:

```JSX
state.count++; // This is bad

setState(prev => ({...prev, count: prev.count + 1})) // This is good
```

The second type of state is shared state, where the same piece of data might be used by multiple components. Then, the convention is to lift the state up to common parent. Then, the data is passed down through the children props, as well as the handlers. This is known as prop drilling.

For example, the <Parent /> component might have \[value, setValue] as state. Then, we would pass it down like 

```JSX
<Child value={value} onChange={setValue} />
```

To avoid prop drilling, the solution is to use global state through the Context API, as explained previously.

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

There are two commonly used libraries for global state management: Redux and Zustand.

Redux is based on having a global store where state is read-only. To update state, you must dispatch actions, which run a reducer to return a new state to make the UI re-render. Here's an example:

```JSX
const counterSlice = createSlice({
  name: "counter",
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1
    }
  }
})
```

Zustand is a more-minimal hook-based global state store. You create a store and use it in components. Here's an example:

```JSX
//Create the store
const useCounterStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
}))

//Use the store
const count = useCounterStore((state) => state.count)
```

