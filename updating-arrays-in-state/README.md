# ![React Shopping Cart - Updating Arrays in State](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## Updating Arrays in State

> Arrays are mutable in JavaScript, but you should treat them as immutable when you store them in state. Just like with objects, when you want to update an array stored in state, you need to create a new one (or make a copy of an existing one), and then set state to use the new array.

Check out the React docs on [Updating Arrays in State](https://react.dev/learn/updating-arrays-in-state) for more info. 

### Adding to an array

In our app, users will have the ability to select items from the shop’s inventory, and add those items to their own inventory. When an item is added to the user’s inventory, it should be removed from the shop’s inventory.

Add the following to `Shop.jsx`: 

```jsx
// src/components/Shop/Shop.jsx

const handleAddItem = (item) => {
	setUserInventory([...userInventory, item])
	setShopInventory(shopInventory.filter((el) => el._id !== item._id))
}
```

Let’s breakdown what is going on in the function above.

- The function accepts an `item` object as an argument. We’ll need to remember to pass in this argument when we call upon the `handleAddItem` function.
- Notice the use of the [spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax). When we `setUserInventory` state to a new array, we use the spread syntax to copy the contents of the existing `userInventory` array. The new array set to state includes in the existing contents, along with the new `item` added to the end of the array.
- Notice the use of the [filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) method here. Remember, the `filter` method returns a shallow copy of the filtered array. When we `setShopInventory`, the array being stored in state will include all of the `item` objects whose `_id` does not match the `_id` of the selected `item`.

Pass the function down to the `InventoryList` component that has a title of “Shop Inventory”.

```jsx
// src/components/Shop/Shop.jsx

  return (
    <main>
      <h1>Shop</h1>
      <section className="shop-section">
        <InventoryList
          title="Shop Inventory"
          inventory={shopInventory}
          handleAddItem={handleAddItem}
        />
        <InventoryList title="User Inventory" inventory={userInventory} />
      </section>
    </main>
  )
```

### Conditional Rendering

Next we’ll need to conditionally render some buttons! At the moment, we have two instances of `InventoryList`. One instance of this component needs to display an **Add Item** button, while the other needs to display a **Remove Item** button.

We’ll be able to identify which `button` the component instance should display based on the `handleAddItem` prop. 

Take a look at the `ternary` below. 

What is this accomplishing? If the `handleAddItem` function has been passed down to this component instance, it should display the **Add Item** button. Otherwise, it should display the **Remove Item** button.

```jsx
{props.handleAddItem
	? <button>Add Item</button>
  : <button>Remove Item</button>
}
```

Add the ternary to `InventoryList.jsx`. 

```jsx
// src/components/InventoryList/

const InventoryList = (props) => {
  return (
    <div>
      <h2>{props.title}</h2>
      <ul>
        {props.inventory.map((item) => (
          <li key={item._id}>
            <p>{item.name}</p>
            <p>Cost: {item.cost}</p>

            {props.handleAddItem
              ? <button>Add Item</button>
              : <button>Remove Item</button>
            }

          </li>
        ))}
      </ul>
    </div>
  )
}

export default InventoryList
```

### Event Handler 

Update the Add Item button with an `onClick` event handler that calls upon `handleAddItem`.

Be sure to pass in the `item` object as an `argument`.

```jsx
// src/components/InventoryList/InventoryList.jsx

  ? <button onClick={() => props.handleAddItem(item)}>
      Add Item
    </button>
```

Notice how we pass an `item` argument to the `handleAddItem` function. When we pass a value to a function, we need to invoke the function. Invoking a function within an event handler will cause that function to run when the page loads. Wrapping it with an anonymous callback function will prevent that behavior. In short, the anonymous function ensures that `handleAddItem` only runs on click.

Check your browser. You should now be able to add items from the shop inventory to your 'cart'.

### Removing items from an array

Add the following to `Shop.jsx`. Similar to the handleAddItem function, when an item is removed from the user’s inventory, it should be  added to the shop’s inventory.

```jsx
// src/components/Shop/Shop.jsx

  const handleRemoveItem = (item) => {
    setShopInventory([...shopInventory, item])
    setUserInventory(userInventory.filter((el) => el._id !== item._id))
  }
```

Pass the function down to the `InventoryList` component with a title of “User Inventory”.

```jsx
// src/components/Shop/Shop.jsx

return (
  <main>
    <h1>Shop</h1>
    <section style={style}>
      <InventoryList
        title="Shop Inventory"
        inventory={shopInventory}
        handleAddItem={handleAddItem}
      />
      <InventoryList
        title="User Inventory"
        inventory={userInventory}
        handleRemoveItem={handleRemoveItem}
      />
    </section>
  </main>
)
```

Update the Remove Item button with the following `onClick` event handler. Remember to pass in the `item` object.

```jsx
// src/components/InventoryList/InventoryList.jsx

  : <button onClick={() => props.handleRemoveItem(item)}>
      Remove Item
    </button>
```

Check your browser. You should now be able to remove items from your cart!

