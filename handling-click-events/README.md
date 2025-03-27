<h1>
  <span class="headline">Lifting State with Arrays in React Shopping Cart</span>
  <span class="subhead">Handling Click Events</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to update component state using click event handlers.

## Browser events in React

To change the `inventory` state in our app, we need to handle browser events. In React, event handling works a little differently from what you may have seen in plain JavaScript.

### Connecting event handlers in React

In regular JavaScript, we attach event listeners using `addEventListener()`. In React, we assign event handler functions directly to **event props** on JSX elements (like `<button>`, `<div>`, etc.).

Here are a few important points:

- Event prop names in React use **camelCase** (`onClick`), not lowercase (`onclick`) like in HTML. You can see [React’s list of supported events here](https://react.dev/reference/react-dom/components/common#react-event-object).

- The value inside the event prop must be a **function**, not the result of a function call. This means you use `{handleClick}`, not `{handleClick()}`.

- In regular JavaScript, returning `false` from an event handler can stop an event’s default behavior. In React, we use the `preventDefault()` method on the event object instead.

## Event Handling

By convention, we start event handler function names with the word `handle`. For example: `handleAddItem()`.

We’ll now create a function called `handleAddItem()` and use it with a `<button>` element. When the button is clicked, this function will run and update the `inventory` state.

Here’s how to set that up in `Shop.jsx`:

```jsx
// src/components/Shop/Shop.jsx

const Shop = () => {
  const [inventory, setInventory] = useState(inventoryData);

  // Define the event handler
  const handleAddItem = () => {
    setInventory([]);
  };

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

In this example, clicking the button calls `handleAddItem()`, which sets `inventory` to an empty array. You should see the list of items disappear from the page.

## Updating inventory with new items

Right now, the function is called `handleAddItem()`, but it doesn’t actually add anything—it clears the list. Let’s update it to add a new item.

Our initial `inventoryData` is an array of item objects. Each object has this structure:

```js
{ _id: 62345, name: 'Banana', price: 0.27 }
```

To add a new item to the list, we need to create a similar object and update the state.

Try this version of `handleAddItem()`:

```jsx
const handleAddItem = () => {
  setInventory({ _id: 62345, name: 'Banana', price: 0.27 });
};
```

> ⚠️ You’ll likely see an error in the console!

That’s because `InventoryList` uses `.map()` to loop through the `inventory`, which only works if `inventory` is an **array**. By setting `inventory` to a single object, we broke that expectation.

## Fixing the error

To fix the error, we need to make sure `inventory` is always an **array of objects**.

Update `handleAddItem()` like this:

```jsx
// src/components/Shop/Shop.jsx

const handleAddItem = () => {
  const newItemOne = { _id: 62345, name: 'Banana', price: 0.27 };
  setInventory([newItemOne]); // put the object inside an array
};
```

Now, clicking the button replaces the inventory list with a single item: the banana. Our state is still an array—it just has one item in it.

## Adding more than one item

What if we want to add multiple items at once?

Try this:

```jsx
// src/components/Shop/Shop.jsx

const handleAddItem = () => {
  const newItemOne = { _id: 62345, name: 'Banana', price: 0.27 };
  const newItemTwo = { _id: 22345, name: 'Avocado', price: 2.0 };
  setInventory([newItemOne, newItemTwo]); // two objects in the array
};
```

This time, clicking the button replaces the current inventory with both items. We’re successfully updating the inventory state using an array of objects.

In the next section, we’ll refactor the code further to match the final design and behavior shown in our mockup.
