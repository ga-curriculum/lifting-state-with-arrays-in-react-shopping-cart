<h1>
  <span class="headline">Shopping Cart</span>
  <span class="subhead">UI Improvements</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to reflect an empty array in the UI using a ternary operator.

Let's add a slight UI touch. We can check if the `inventory` array passed down in our props object has any elements in it. If not, we want to render a single `<p>` element that displays the text "Empty". If the inventory passed to the `InventoryList` component is empty, the appropriate list will appear empty.

Add the following to `InventoryList.jsx`.

```jsx
// src/components/InventoryList/InventoryList.jsx

// css
import './InventoryList.css';

const InventoryList = (props) => {
  return (
    <div className="inventory-list">
      <h2>{props.title}</h2>
      {props.inventory.length ? (
        <ul>
          {props.inventory.map((item) => (
            <li key={item._id}>
              <p>{item.name}</p>
              <p>Price: {item.price}</p>
              {props.handleAddItem ? (
                <button onClick={() => props.handleAddItem(item)}>
                  Add Item
                </button>
              ) : (
                <button onClick={() => props.handleRemoveItem(item)}>
                  Remove Item
                </button>
              )}
            </li>
          ))}
        </ul>
      ) : (
        <p>Empty</p>
      )}
    </div>
  );
};

export default InventoryList;
```
