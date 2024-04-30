# ![Lifting State with Arrays in React - Shopping Cart - Handling Click Events](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to impact state with click event handlers.   

## Browser Events in React

To change inventory state, we’ll need to introduce browser events to our React App. Like many things in React, event handling is a little different than what we may be used to with vanilla DOM manipulation.

### Connecting Handler Code to Events in React

In React, we do not add event listeners using JavaScript’s `addEventListener` method.

Instead, we assign event handling functions to certain “event” props on React Elements (`<div>`, `<p>`, etc.).

Event observations thus far:

- The names for event props are lowerCamelCased (`onClick`). In HTML, the attribute would be `onclick`. Here’s the **[list of events](https://react.dev/reference/react-dom/components/common#react-event-object)** supported by React.
- The JS expression (always within curly braces) assigned to an event prop must evaluate to a **function**. A function type, **not** a function call (unless that function call returns a function).
- In native JS, if the event handler function returns `false`, it prevents that event’s default behavior. It stops event bubbling (same as calling both the `preventDefault()` & `stopPropagation()` methods). However, in React, we must call the `preventDefault()` method on the **Synthetic Event** object.

## Event Handling

It’s convention to start the name of event handler methods with the word `handle`, e.g., `handleSomeInteraction`

In the next step we’ll create a new function called `handleAddItem`. We’ll also want to add in a `<button>` tag with an `onClick` event handler. This will allow us to call upon the `handleAddItem` function when the user clicks the button. 

We’ll refactor this later on, but for now we’ll use it to demonstrate how state can be updated. 

Make the following changes to `Shop.jsx`:

```jsx
// src/components/Shop/Shop.jsx

const Shop = () => {
    const [inventory, setInventory] = useState(inventoryData)

    // Add the following function: 
    const handleAddItem = () => {
        setInventory([])
    }

    // Add the button: 
    return (
        <main>
            <h1>Shop</h1>
            <button onClick={handleAddItem}>Click Here</button>
            <InventoryList inventory={inventory} />
        </main>
    )
}

export default Shop
```

Notice how we are setting `inventory` state in our `handleAddItem` function. When this function runs, we `setInventory` state to an empty array (`[]`). This action replaces the initial state (`inventoryData`) with a brand new value (`[]`).  

Try it out in your browser. You should see the list of items clear out entirely.

You might have noticed an issue here. We have a function called `handleAddItem`, but at the moment all this function does is clear the list of items. Let’s address that now.

If we look at our initial state of `inventoryData`, we’ll see that it is an array of objects. Each object currently held in state should resemble the example below.

```jsx
{ _id: 62345, name: 'Banana', price: .27 }
```

Our goal is to add a new item object like this one to state. The object must contain properties for `_id`, `name`, and `price` in order to work with the rendering we built out earlier. 

Let’s try it out. Update the `handleAddItem` function as demonstrated below. Try adding an item.

```jsx
  const handleAddItem = () => {
    setInventory({ _id: 62345, name: 'Banana', price: .27 })
  }
```

> You should be seeing an error in the browser console right now!

In our `InventoryList` component, we are using `Array.prototype.map()` on the `inventory` data that we passed down as props. This method is only accessible on arrays. In the previous step, we changed the value of `inventory` state from an array of objects, to a single object - hence the error.  

Let’s fix this issue next. Update `handleAddItem` with the following and try it out!

```jsx
// src/components/Shop/Shop.jsx

  const handleAddItem = () => {
    const newItemOne = { _id: 62345, name: 'Banana', price: .27 }
    // Notice how we are placing newItemOne inside an array when setting state!
    setInventory([newItemOne])
  }
```

At this point, clicking on the button should replace the existing list of items with the `newItemOne` object. Our `inventory` state is still an array, but it only contains a **single object**! 

Can we add multiple items to state? Let’s try! Update `handleAddItem` with the following.

```jsx
// src/components/Shop/Shop.jsx

  const handleAddItem = () => {
    const newItemOne = { _id: 62345, name: 'Banana', price: .27 }
    const newItemTwo = { _id: 22345, name: 'Avocado', price: 2 }
    // Sets the inventory state to an array containing two objects
    setInventory([newItemOne, newItemTwo])
  }
```

You should now be able to add these two items to `inventory` state. So far, we've figured out how to add objects to inventory state, but we're still working with a single inventory. 

In the next section, we’ll do a little refactoring so that this UI and functionality mirrors what we have in our mockup. 