Want to revise Map? 
Let's break down the `map` method syntax and then look at examples using different ways to create JavaScript functions.

### Understanding `map` Method Syntax
The `map` method creates a new array by applying a function to each element of the original array. The syntax is:

```javascript
array.map(function(currentValue, index, array) {
  // return element for new array
});
```

- `currentValue`: The current element being processed in the array.
- `index` (optional): The index of the current element being processed.
- `array` (optional): The array `map` was called upon.

### Examples with Different Ways of Creating Functions

#### 1. Using Arrow Functions
Arrow functions provide a concise way to write functions in JavaScript.

```javascript
const array = [1, 2, 3, 4, 5];
const doubled = array.map(number => number * 2);
console.log(doubled); // Output: [2, 4, 6, 8, 10]
```

#### 2. Using Function Keyword
You can also use the traditional `function` keyword to define the function.

```javascript
const array = [1, 2, 3, 4, 5];
const doubled = array.map(function(number) {
  return number * 2;
});
console.log(doubled); // Output: [2, 4, 6, 8, 10]
```

#### 3. Creating a Separate Function
You can define a function separately and then pass it to the `map` method.

```javascript
const array = [1, 2, 3, 4, 5];

function doubleNumber(number) {
  return number * 2;
}

const doubled = array.map(doubleNumber);
console.log(doubled); // Output: [2, 4, 6, 8, 10]
```

### Summary
- **Arrow Functions**: `array.map(number => number * 2);`
- **Function Keyword**: `array.map(function(number) { return number * 2; });`
- **Separate Function**: `array.map(doubleNumber);`

Each of these methods will give you the same result, but they offer different ways to define the function you pass to `map`.

Feel free to ask if you need more examples or further clarification!
