<h1>
  <span class="headline">Lifting State with Arrays in React Shopping Cart</span>
  <span class="subhead">Handling Click Events</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to impact state with click event handlers.

## Browser events in React

We'll must introduce browser events to our React App to change the inventory state. Like many things in React, event handling is slightly different than what we may be used to with vanilla DOM manipulation.

### Connecting handler code to events in React

In React, we do not add event listeners using JavaScript's `addEventListener()` method.

Instead, we assign event-handling functions to specific event props on React Elements (`<div>`, `<p>`, etc.).

Event observations thus far:

- The names for event props are lowerCamelCased (`onClick`). In HTML, the attribute would be `onclick`. Here's the [list of events](https://react.dev/reference/react-dom/components/common#react-event-object) supported by React.

- The JavaScript expression (always within curly braces) assigned to an event prop must evaluate to a function. It may not be a function call (unless that function call returns a function).

- In native JavaScript, if the event handler function returns `false`, that event's default behavior is suppressed. It stops event bubbling (just like calling both the `preventDefault()` and `stopPropagation()` methods). However, in React, we must call the `preventDefault()` method on the React event object object.

## Event Handling

By convention, we'll start the name of event handler methods with the word `handle`, for example, `handleSomeInteraction()`

In the next step, we'll create a new function called `handleAddItem()`. We also want to add a `<button>` tag with an `onClick` event handler. This will allow us to call upon the `handleAddItem()` function when the user clicks the button.

We'll refactor this later on, but for now, we'll use it to demonstrate how state can be updated.

Make the following changes to `Shop.jsx`:

```jsx
// src/components/Shop/Shop.jsx

const Shop = () => {
  const [inventory, setInventory] = useState(inventoryData);

  // Add the following function:
  const handleAddItem = () => {
    setInventory([]);
  };

  // Add the button:
  return (
    <main>
      <h1>Shop</h1>
      <button onClick={handleAddItem}>Click Here</button>
      <InventoryList inventory={inventory} />
    </main>
  );
};

export default Shop;
```

Notice how we set `inventory` state in our `handleAddItem()` function. When this function runs, we call `setInventory()` to set the `inventory` state to an empty array (`[]`). This action replaces the initial state (`inventoryData`) with a brand-new value (`[]`).

Try it out in your browser. You should see the list of items clear out entirely.

You might have noticed an issue here. We have a function called `handleAddItem()`, but at the moment, all this function does is clear the list of items. Let's address that now.

If we look at our initial state of `inventoryData`, we'll see that it is an array of objects. Each object currently held in state should resemble the example below.

```jsx
{ _id: 62345, name: 'Banana', price: .27 }
```

Our goal is to add a new item object like this one to state. The object must contain properties for `_id`, `name`, and `price` in order to work with the rendering we built out earlier.

Let's try it out. Update the `handleAddItem()` function as demonstrated below. Try adding an item.

```jsx
const handleAddItem = () => {
  setInventory({ _id: 62345, name: 'Banana', price: 0.27 });
};
```

> 😎 You should see an error in the browser console right now!

Our `InventoryList` component, uses the array's `map()` method on the `inventory` data we passed down as props. This method is only accessible on arrays. In the previous step, we changed the value of the `inventory` state from an array of objects to a single object - hence the error.

Let's fix this issue next. Update `handleAddItem()` with the following and try it out!

```jsx
// src/components/Shop/Shop.jsx

const handleAddItem = () => {
  const newItemOne = { _id: 62345, name: 'Banana', price: 0.27 };
  // Notice how we are placing newItemOne inside an array when setting state!
  setInventory([newItemOne]);
};
```

At this point, clicking on the button should replace the existing list of items with the `newItemOne` object. Our `inventory` state is still an array but only contains a **single object**!

Can we add multiple items to state? Let's try! Update `handleAddItem()` with the following.

```jsx
// src/components/Shop/Shop.jsx

const handleAddItem = () => {
  const newItemOne = { _id: 62345, name: 'Banana', price: 0.27 };
  const newItemTwo = { _id: 22345, name: 'Avocado', price: 2 };
  // Sets the inventory state to an array containing two objects
  setInventory([newItemOne, newItemTwo]);
};
```

You should now be able to add these two items to the `inventory` state. So far, we've figured out how to add objects to state, but we're still working with a single inventory.

In the next section, we'll do a little refactoring so that this UI and functionality mirrors what we have in our mockup.
