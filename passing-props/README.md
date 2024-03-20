# ![React Shopping Cart - Passing Props](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## Props Refresher

### What are Props?

A parent component uses props to pass data/state and references to methods down to child components.

A prop looks much like an `attribute=value` pair in an HTML element.

As discussed in the JSX lesson, we use camelCasing to name our props instead of the kebab-casing preferred in HTML.

Note that it’s common to pass the same data as a prop, over and over, down the component hierarchy.

### Passing Props

You must use curly braces to pass any value other than a simple string (even template literals need to be surrounded by curly braces).

Just like how Chrome’s DevTools are invaluable when it comes to troubleshooting the DOM, so are React’s Developer Tools when it comes to troubleshooting a React app!

With the Chrome (or Firefox, or Edge) extension installed, you will now see a **Components** tab in Chrome’s DevTools!

### Accessing Passed Props

When React renders a Functional Component, it will always pass in props as the first argument to the function. As a result, we can always access props as a parameter like so: 

```jsx
const ComponentName = (props) => (
  ...
```

### Props Cannot be Changed

Props are immutable; their values can never to be changed.

Remember, the prop came from a component somewhere up the hierarchy, and if the prop's value originated from state, it would be **that** component's responsibility to update its state.

However, a component can pass down methods via props that can update its state - but that's for another lesson.

## Working with Props

Let’s move some of the code we wrote in `src/components/Shop/Shop.jsx` to a new component. This will help use keep our application organized and make our code more reusable. 

Run the following commands in your terminal:

```bash
mkdir src/components/InventoryList
touch src/components/InventoryList/InventoryList.jsx
```

Add the following to `InventoryList.jsx`.

```jsx
// src/components/InventoryList/InventoryList.jsx

const InventoryList = (props) => {
  return (
    <div>
      <h2>Inventory List</h2>
      <ul>
        ...map here
      </ul>
    </div>
  )
}

export default InventoryList
```

Back in `Shop.jsx`, import the newly created `InventoryList` component.

```jsx
// src/components/Shop/Shop.jsx

// components
import InventoryList from "../../components/InventoryList/InventoryList"
```

Add the `InventoryList` component within the `main` tag, and pass it an `inventory` prop with a value set to `inventoryData` (this approach will make more sense in the next few steps).

```jsx
// src/components/Shop/Shop.jsx

// components
import InventoryList from "../../components/InventoryList/InventoryList"

// data
import { inventoryData } from "../../data/data"

const Shop = () => {
  return (
    <main>
      <h1>Shop</h1>
      <InventoryList inventory={inventoryData} />
    </main>
  )
}

export default Shop
```

After verifying that `props` are being passed down correctly, `map` through `props.inventory` to produce an array of `li` tags in `InventoryList.jsx`.

```jsx
// src/components/InventoryList/InventoryList.jsx

const InventoryList = (props) => {
  return (
    <div>
      <h2>Inventory List</h2>
      <ul>
        {props.inventory.map((item) => (
          <li key={item._id}>
            <p>{item.name}</p>
						<p>Cost: {item.cost}</p>
          </li>
        ))}
      </ul>
    </div>
  )
}

export default InventoryList 
```

At the moment, there is no way for a user to make changes to the inventory list, as we are rendering a hardcoded array of objects.

If we want our component to remember the changes a user makes to the inventory list between renders, we’ll need to use **state**.