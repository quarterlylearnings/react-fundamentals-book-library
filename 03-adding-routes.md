## Part 3: Routes for Shelf View

In this lesson, you are going to add routing to your library. This will allow us to navigate to a specific URL for each shelf and display the books on that shelf. We'll use the `react-router-dom` library to add routing to our application.

First, you'll need to install `react-router-dom` if you haven't already. You can do this by running the following command in the terminal:

```bash
npm install react-router-dom
```

Next, let's import the necessary components from `react-router-dom` in our `index.js` file:

```jsx
...
import { BrowserRouter } from "react-router-dom";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
...
```

We'll wrap our entire application in a `BrowserRouter` component. This will allow us to use `Routes` throughout the application at any place we want to have a URL path be the determinant for which components are rendered.

Inside the `App` component, be sure to import `Routes` and `Route` (& `NavLink` if you need it) from `react-router-dom`, as well.

```jsx
import { NavLink, Route, Routes } from "react-router-dom";
```
We'll replace the `Library` component with a `Routes` component. The `Routes` child (`Route`) component will render the `Library` component when the URL matches the root path (`/`).


Here's how we can do this:

```jsx
// In App.js
return (
  <Routes>
    <Route path="/" element={<Library />} />
  </Routes>
);
```

Next, let's modify the `Library` component. Instead of displaying the books on a shelf when the shelf's name is clicked, we'll navigate to a new URL for that shelf. We'll do this by replacing the `h1` element with a `Link` component. The `Link` component will navigate to a new URL when clicked.

Here's how we can do this:

```jsx
// In Library.js
<Link to={`/shelf/${shelf.genre}`}>{shelf.genre}</Link>
```

In the code above, we're using a template string to create a URL that includes the genre of the shelf. The `to` prop of the `Link` component specifies the URL to navigate to when the link is clicked.

Finally, we need to add a new `Route` component to render a `Shelf` component when the URL matches the path for a shelf. We'll do this in the `Library` component.

How could we do this?

Use the [`fetching-cookbook`](https://github.com/quarterlylearnings/fetching-cookbook) and the latest [React Router documentation](https://reactrouter.com/en/main/start/overview#dynamic-segments) about dynamic segments to guide you towards displaying the contents of a shelf as a route as opposed to a toggle.

Here is a good [example](https://github.com/remix-run/react-router/blob/dev/examples/route-objects/src/App.tsx) of how this _could_ be achieved.

Hopefully, when you click on a shelf's name, the app's URL will change to `/shelf/:genre`, and a `Shelf` component with the books on that shelf will be displayed.

Do you think we could do the same thing for a Book?