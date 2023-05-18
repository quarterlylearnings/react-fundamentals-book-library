## Part 1: Adding a New Shelf

In this lesson, you'll modify the `Library` component to display a list of shelf names instead of all the books on a shelf. We'll also add an arrow icon next to each shelf name to toggle the visibility of the books on that shelf. Finally, we'll add a `NewShelfForm` component to allow users to add new shelves to the library.

Let's start by modifying the `Library` component. 

First, we need to add a new piece of state to keep track of which shelves are currently expanded. We'll call this state `expandedShelves`, and it will be an array of booleans. Each boolean will correspond to a shelf in the `shelves` array. If a shelf is expanded, its corresponding boolean in `expandedShelves` will be `true` - otherwise, it will be `false`.

Here's how we can add this new state to the `Library` component:

```jsx
const [expandedShelves, setExpandedShelves] = useState(shelves.map(() => false));
```

If there were 4 shelves, the code above would initialize the `expandedShelves` state value to be:

`[false ,false ,false ,false]`

So none of those four shelves should be expanded to display the books they each contain.

<details>
<summary>Why isn't the visibility of the books on a shelf something that is determined in the Shelf component? (click for an explanation)</summary>
<br>

The visibility of the books on a shelf could indeed be managed within the Shelf component itself. However, in this particular case, we've chosen to manage it in the Library component for a couple of reasons:

1. Parent-Child Communication

     By managing the visibility in the Library component, we can easily control the visibility of each Shelf from the parent component. This is a common pattern in React where the parent component manages the state for its child components.

2. Single Source of Truth

    It's generally a good practice in React to have a single source of truth for any piece of data that's shared across multiple components. In this case, the Library component is the single source of truth for the visibility of the books on each shelf.

However, if the Shelf component had other child components that also needed to know about the visibility of the books, or if the visibility was a more integral part of the Shelf component's internal workings, it might make more sense to manage it within the Shelf component itself.

The way you manage state in React is a choice, and the best approach can depend on a variety of factors, including the specific requirements of your app and your personal preferences as a developer.

</details>

----

Next, we need to modify the JSX returned by the `Library` component. Instead of mapping over the `shelves` array from the `Library`'s state and returning a `Shelf` component for each shelf, we'll return a list item for each shelf. This list item will include the shelf's genre and an arrow icon. 

Here's how we can do this:

```jsx
return (
  <section>
    {shelves.map((shelf, index) => (
      <div key={index}>
        <h1 onClick={() => toggleShelf(index)}> 
          {shelf.genre} 
          <span>{expandedShelves[index] ? '▼' : '►'}</span>
        </h1>
        {expandedShelves[index] && <Shelf genre={shelf.genre} books={shelf.books} />}
      </div>
    ))}
  </section>
);
```

When the arrow icon is clicked, we'll toggle the visibility of the corresponding `Shelf` component by adding an `onClick` to the `h1` which invokes a function to modify whether that Shelf is expanded or not.

```jsx
...
        <h1 onClick={() => toggleShelf(index)}> 
...
```

This handler calls a function called `toggleShelf`: 

```jsx
const toggleShelf = (index) => {
    setExpandedShelves(expandedShelves.map((expanded, i) => (i === index ? !expanded : expanded)));
};
```

The `toggleShelf` function accepts an index and toggles the boolean at that index in the `expandedShelves` array. So, if someone clicks on the the second of four shelves (`expandedShelves[1]`), the `expandedShelves` state value would change from this:

`[false, false, false, false]`

to this:

`[false, true, false, false]`


In the above mapping of shelves, this line

```jsx
{expandedShelves[index] && <Shelf genre={shelf.genre} books={shelf.books} />}
```

is responsible for determining the visibility of a `Shelf` component for a book. If `expandedShelves[index]` is truthy, the `Shelf` will render, otherwise nothing will appear beneath the `h1`.


----

Finally, we need to add a `NewShelfForm` component. This component will be similar to the `NewBookForm` component. It will include a form with an input field for the genre of the new shelf and a submit button. When the form is submitted, it will call a function passed as a prop to add the new shelf to the `shelves` state in the `Library` component.


```jsx
const NewShelfForm = ({ addShelf }) => {
  const [genre, setGenre] = useState('');

  const handleSubmit = (event) => {
    event.preventDefault();
    addShelf({ genre, books: [] });
    setGenre('');
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={genre}
        onChange={(e) => setGenre(e.target.value)}
        placeholder="Genre"
      />
      <button type="submit">Add Shelf</button>
    </form>
  );
};
```

In the `NewShelfForm` component, we have a `useState` hook that keeps track of the genre of the new shelf. The `handleSubmit` function prevents the default form submission behavior, calls the `addShelf` function (passed as a prop) with the new genre, and then resets the genre to an empty string.

The JSX returned by the `NewShelfForm` component includes a form with an input field and a submit button. The `value` prop of the input field is set to the current genre, and the `onChange` prop is set to a function that updates the genre with the new input value.

Finally, we need to add the `NewShelfForm` component to the `Library` component and pass the `addShelf` function as a prop. The `addShelf` function should add the new shelf to the `shelves` state and add a new `false` value to the `expandedShelves` state.

We can modify the `Library` component with the below function and component addition.

```jsx
const addShelf = (newShelf) => {
  setShelves([...shelves, newShelf]);
  setExpandedShelves([...expandedShelves, false]);
};

return (
  <section>
    <NewShelfForm addShelf={addShelf} />
    {shelves.map((shelf, index) => (
      // ...
    ))}
  </section>
);
```

In the code above, the `addShelf` function uses the spread operator (`...`) to create a new array that includes all current shelves and the new shelf. This new array is then set as the new `shelves` state. Similarly, a new `false` value is added to the `expandedShelves` state.

You've now added a new shelf to your library 😮‍💨

In the next lesson, we'll learn how to update the `Library` state after a Book gets added to a Shelf. 

> If you are unclear on what you just did, do not proceed. Re-read this lesson and look at the code line by line. If you need to ask questions, then do so but do not just click through in order to "finish".

[I understand what I just wrote.](02-lifting-book-state-up.md)