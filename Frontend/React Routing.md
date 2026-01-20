Most modern web applications require multiple web pages, one for each section of the header, for example. However, React by itself only shows one page at a time, as it is made for SPA (single-page application). That is why we use React Router to tell the app how to change between pages.

First, wrap the BrowserRouter around the entire app and the different routes:

```JSX
import { BrowserRouter } from "react-router-dom";
import {Routes, Route} from "react-router-dom";

const App = () => {
  return (
    <BrowserRouter>
		<Routes>
			<Route path="/" element={<Home />} />
			<Route path="/about" element={<About />} />
		</Routes>
    </BrowserRouter>
  );
}
```

Let's say we are in the home page currently, and we want to put a link which allows us to navigate to another page. We would use the Link component:

```JSX
import { Link } from "react-router-dom";

<Link to="/about">About</Link>
```

We can also create dynamic routes with URL params, for example /users/42:

```JSX
<Route path="/users/:id" element={<User />} />
```

and then to access the params we would just do 

```JSX
import { useParams } from "react-router-dom";

const { id } = useParams();
```

Oftentimes, we would want to have certain routes protected behind authentication. We can do so like this:

```JSX
const ProtectedRoute = ({children}) => {
	return isAuth ? children : <Navigate to="/login" />;
}
```

Navigate is a function in React Router which routes the user to the argument path.
