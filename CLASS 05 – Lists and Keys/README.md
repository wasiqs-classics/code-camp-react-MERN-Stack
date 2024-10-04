## Class 5: Lists and Keys

### 1. Lists in React

#### What are Lists?
- **Lists**: In React, lists are used to display a collection of data. They are typically rendered using the JavaScript `map()` function, which transforms an array of data into an array of React elements.

#### Rendering Lists
- **Using `map()`**: The `map()` function iterates over an array and returns a new array of elements. [Learn more about 'Map Method' in Javascript](https://github.com/wasiqs-classics/code-camp-react-MERN-Stack/blob/master/CLASS%2005%20%E2%80%93%20Lists%20and%20Keys/Map-Revision.md)
- **Example**: Rendering a list of numbers:

```jsx
import React from 'react';

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
function App() {
  return <NumberList numbers={numbers} />;
}

export default App;
```

### 2. Keys in React

#### What are Keys?
- **Keys**: Keys are unique identifiers used to identify which items in a list have changed, been added, or removed. They help React optimize rendering by keeping track of elements.

#### Importance of Keys
- **Performance**: Keys help React efficiently update and re-render lists by identifying which items have changed.
- **Uniqueness**: Keys must be unique among siblings. They do not need to be globally unique.

#### Best Practices for Keys
- **Stable and Unique**: Use stable and unique values like IDs.
- **Avoid Using Index as Key**: Using the array index as a key can lead to performance issues and unexpected behavior.

### 3. Examples
### Example 1: Rendering a List of Users

Imagine you have a list of users that you want to display in a table. Each user has a unique ID, which can be used as a key.

```jsx
import React from 'react';

function UserTable(props) {
  const users = props.users;
  return (
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Email</th>
        </tr>
      </thead>
      <tbody>
        {users.map((user) => (
          <tr key={user.id}>
            <td>{user.id}</td>
            <td>{user.name}</td>
            <td>{user.email}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}

const users = [
  { id: 1, name: 'John Doe', email: 'john@example.com' },
  { id: 2, name: 'Jane Smith', email: 'jane@example.com' },
  { id: 3, name: 'Alice Johnson', email: 'alice@example.com' }
];

function App() {
  return <UserTable users={users} />;
}

export default App;
```

**Explanation**:
- In this example, we have a `UserTable` component that takes a list of users as a prop.
- Each user has a unique `id`, which is used as the key when rendering the list of users in a table.
- Using the `id` as the key helps React efficiently update and re-render the list when the data changes.

### Example 2: Rendering a List of Products

Consider a scenario where you have a list of products, each with a unique SKU (Stock Keeping Unit). You want to display these products in a list.

```jsx
import React from 'react';

function ProductList(props) {
  const products = props.products;
  return (
    <ul>
      {products.map((product) => (
        <li key={product.sku}>
          {product.name} - ${product.price}
        </li>
      ))}
    </ul>
  );
}

const products = [
  { sku: 'A123', name: 'Laptop', price: 999.99 },
  { sku: 'B456', name: 'Smartphone', price: 699.99 },
  { sku: 'C789', name: 'Tablet', price: 499.99 }
];

function App() {
  return <ProductList products={products} />;
}

export default App;
```

**Explanation**:
- In this example, we have a `ProductList` component that takes a list of products as a prop.
- Each product has a unique `sku`, which is used as the key when rendering the list of products.
- Using the `sku` as the key ensures that each product is uniquely identified, allowing React to efficiently manage the list.


---
