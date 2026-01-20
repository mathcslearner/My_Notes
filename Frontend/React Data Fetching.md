Data fetching is the process of retrieving data from an external source. Usually, this will be to an external API or to localStorage. The fetched data usually represents server state, not UI state. Typically, it is done in useEffect, since it's a side effect.

The most basic way to do data fetching is with fetch:

```JSX
const [data, setData] = useState(null);
const [loading, setLoading] = useState(true);
const [error, setError] = useState(null);

useEffect(() => {
  fetch("/api/data")
    .then(res => res.json())
    .then(data => {
      setData(data);
      setLoading(false);
    })
    .catch(err => {
      setError(err);
      setLoading(false);
    });
}, []);
```

As seen in the example above, it's good to make sure to keep track of loading and error when we do data fetching, and maybe change the component look based on the status. We can add a cleanup function to prevent memory leaks:

```JSX
useEffect(() => {
  const controller = new AbortController();
  const signal = controller.signal;

  fetch("/api/data", { signal })
    .then(res => {
      if (!res.ok) throw new Error("Network response was not ok");
      return res.json();
    })
    .then(data => {
      setData(data);
      setLoading(false);
    })
    .catch(err => {
      if (err.name === "AbortError") {
        // fetch was cancelled, do nothing
        return;
      }
      setError(err);
      setLoading(false);
    });

  // cleanup function
  return () => {
    controller.abort();
  };
}, []);

```

A library which is useful for data fetching is Axios (I mainly use this one). It is popular for making HTTP requests to the backend API and it's easier to use compared to the native fetch method. It is promise-based and simplifies asynchronous requests and handling responses.

Here is a basic example using async function:

```JSX
useEffect(() => {
  const fetchData = async () => {
    try {
      const response = await axios.get('/api/data');
      console.log(response.data);
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  };

  fetchData();
}, []);
```
