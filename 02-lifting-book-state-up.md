## Part 2: Updating the Library State When A New Book Gets Added

In the previous lesson, you delivered the ability to add a new shelf to the library. In this lesson, we're going to modify the application so that when a new book is added to a shelf, the state of the `Library` component is updated, not just the state of the `Shelf` component.

Currently, the `Shelf` component has its own state for the books it contains. When a new book is added, the `addBook` function in the `Shelf` component updates this state. However, this means that the `Library` component isn't aware of the new book. To fix this, we need to lift the state up from the `Shelf` component to the `Library` component.

First, let's remove the `useState` hook from the `Shelf` component. Instead of managing its own state, the `Shelf` component will now receive its books as a prop from the `Library` component.

```jsx
const Shelf = ({ genre, books, addBook }) => {
  // ...
};
```

Next, let's modify the `Library` component to manage the state of the books in each shelf. We'll do this by adding a new `addBook` function in the `Library` component. This function will take a book and the index of the shelf to which the book should be added. It will then update the `shelves` state with the new book.

```jsx
const addBook = (book, shelfIndex) => {
  setShelves(shelves.map((shelf, index) => {
    if (index === shelfIndex) {
      return { ...shelf, books: [...shelf.books, book] };
    } else {
      return shelf;
    }
  }));
};
```

In the `addBook` function above, we're using the `map` function to create a new array of shelves. For each shelf, if the index matches the `shelfIndex` passed to the `addBook` function, we return a new shelf object that includes all the properties of the current shelf and a new `books` array that includes all the current books and the new book. If the index doesn't match the `shelfIndex`, we simply return the current shelf.

Finally, we need to pass the `addBook` function as a prop to each `Shelf` component in the `Library` component. 

We also need to pass the `addBook` function as a prop to the `NewBookForm` component in the `Shelf` component.

Here's how we can do this:

```jsx
// In the Library component
{shelves.map((shelf, index) => (
  <Shelf key={index} genre={shelf.genre} books={shelf.books} addBook={(book) => addBook(book, index)} />
))}

// In the Shelf component
<NewBookForm addBook={addBook} />
```

Now, when a new book is added to a shelf, the state of the `Library` component is updated, keeping our state consistent across the application.

In the next lesson, we'll experiment with routes for each Shelf. 

[I'm Ready To Move On](03-adding-routes.md)