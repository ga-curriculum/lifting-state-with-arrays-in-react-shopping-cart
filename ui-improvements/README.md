# ![React Shopping Cart - UI Improvements](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

Let’s add a small UI touch. We can check if the `inventory` array passed down in our props object has any elements in it. If not, we want to render a single `<li>` element wrapped around a `<p>` that displays the text “Empty”. In any case that the inventory passed to the `InventoryList` component is empty, the appropriate list will show as empty.

Add the following to `InventoryList.jsx`.

```jsx
// src/components/InventoryList/InventoryList.jsx

// css
import './InventoryList.css'

const InventoryList = (props) => {
  return (
    <div>
      <h2>{props.title}</h2>
			<ul>
        {props.inventory.length 
          ?
            props.inventory.map((item)=> (
              <li key={item._id}>
                <p>{item.name}</p>
                <p>Cost: {item.cost}</p>
                {props.handleAddItem
                  ? <button onClick={()=> props.handleAddItem(item)}>
                      Add Item
                    </button>
                  : <button onClick={() => props.handleRemoveItem(item)}>
                      Remove Item
                    </button>
                }
              </li>
            ))
          :
            <li><p>Empty</p></li>
        }
      </ul>
    </div>
  )
}

export default InventoryList

```