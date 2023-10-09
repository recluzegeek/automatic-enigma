---
title: js_array_callback_methods
updated: 2023-10-08 23:04:29Z
---

# Array Callback Methods

## GOALS

- Use the new arrow function syntax
- Understand and use these methods
  - `forEach`
  - `map`
  - `filter`
  - `find`
  - `reduce`
  - `some`
  - `every`

- **Callback Methods** - Requires you to pass them functions as arguments.

## forEach

- Once per element of the array. Have been replaced by `for...of` loop mostly.
  - Accepts a callback function and runs it for each element present in the array.
  - We could pass the Anonymous Functions or Predefined Functions to `forEach`

  ```js
  // Try it in console
  const intArray = [3, 7, 12, 5, 18, 1, 9, 14, 8, 20, 11, 6, 17, 4, 10, 15, 2, 19, 13, 16];

  intArray.forEach(function(el){
    if(el % 2 === 0) console.log(`${el} is an Even Number`);
  });

  intArray.forEach(function(el){
    if(el % 3 === 0) console.log(`${el} is divisible by 3.`)
  });

  const movies = [
  { title: "Inception", rating: 9.3 },
  { title: "The Shawshank Redemption", rating: 9.2 },
  { title: "The Dark Knight", rating: 9.0 },
  { title: "Pulp Fiction", rating: 8.9 },
  { title: "Forrest Gump", rating: 8.8 },
  ];

  movies.forEach(function(movie){
    console.log(`${movie.title} - ${movie.rating}/10`)
  });

  // Same thing can be achieved using for...of loop
  // No need to pass in a callback function.
  // Short and nice syntax to iterate over arrays
  for(const element of intArray){
    if(element % 3  === 0) console.log(`${element} is divisible by 3`);
  }
  ```

## map

- Creates a new array with the results of calling a callback on every element in the array

  > It calls the function for each element of the array and returns the array of results.
  >> It is similar to `forEach` in a sense that it runs the callback on every element of the array and is different in sense that it generates a new array and stores the result of each iteration in the new array.
  >>> It is a way to map an array from one state to another. It doesn't mutates the original array rather transforms a data structure (array in this case) by applying a function to its elements.

  ```js
  // Double the values in the array
  const arr = [2, 4, 5, 1, 8, 7];

  const doubledArray = arr.map(function (num){
    return num * 2;
  });

  // 1st Iteration: doubledArray =  [4]
  // 2nd Iteration: doubledArray = [4, 8]
  // 3rd Iteration: doubledArray = [4, 8, 10]
  // ...
  // and Last Iteration: doubledArray = [4, 8, 10, 2, 16, 14]
  ```

## Arrow Functions

- There’s one more very simple and concise syntax for creating functions, that’s often better than Function Expressions. It’s called “arrow functions”, because it looks like this:
  - `let func = (arg1, arg2, ...argN) => expression` is equivalent to something like this

      > ```js
      > let func = function(arg1, arg2, ...argN) {
      >   return expression;
      >  };
      >```

  - Some more examples
  
    ```js
    // Using Function Expression Syntax
    const squareResult = function square(x){
      return x * x;
    }

    console.log(squareResult(10));

    // Using Arrow Function Syntax
    const square = (x) =>{
      return x * x;
    }

    console.log(square(10));

    // Another Example of Arrow Functions
    const sum = (x, y) => {
      return x + y;
    }
    ```

  - If there are no params to pass, we've to use `()`.

      ```js
      const rollDie = () => {
        return Math.floor(Math.random() * 6) + 1;
      }
      ```

  - Useful for writing callbacks.

***Note:*** `this` keyword behaves differently inside arrow functions compared to regular functions.

### Implicit Returns - Only works in Arrow Functions

- Consider the following functions
  - All these functions do the same thing: checking for even number

  ```js
  const isEven = function sum(num){ // Regular Function Expression
    return num % 2 === 0;
  }

  const isEven = (num) => { // Arrow Function with parens around param
    return num % 2 === 0;
  }

  const isEven = num => { // Arrow Function with no parens around params
    return num % 2 === 0;
  };

  const isEven = num => ( // Implicit Return
    num % 2 === 0
  )

  const isEven = num => num % 2 === 0; // one-liner implicit return
  ```

- If there's only one thing that's being returned from the arrow functions, we can omit `return` keyword
  
  ```js
  const sum = (x, y) => x + y;
  ```

  - And if that single statement is long, we can use `()` to surround it and don't use `;` at the end of that statement.

    ```js
    const random = () => (
      Math.floor(Math.random() * 6) + 1
    );

    // Instead of
    const random = () => Math.floor(Math.random() * 6) +1
    ```

***Note:*** Implicit Return **works only and only when there's only 1 statement** inside the function

### setTimeout & setInterval & clearInterval

## filter

- Creates new array with all the elements that pass the test implemented by the the callback function
  > Used when we want to make a subset from an array satisfying condition implemented in the callback method.
  >> **Callback should return either true or false. It needs to be a boolean function.**
  >>> If callback returns true for any element, then that element gets copied to the newly filtered array. Otherwise, it gets ignored.
  >>>> `filter()` doesn't change the content of the original array. Useful for **filtering highest rating,** **old items,** **new items**

  ```js
  const nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

  // Filter only those nums less than 5
  const res = nums.filter((num) => {
    return n < 5;
  });

  // short and nice syntax using implicit return one liner
  const smallNums = nums.filter( (num) => num < 5 );
  console.log(smallNums);

  // Filter even numbers
  console.log(nums.filter( (num) => num % 2 === 0 ));
  // Filter odd numbers
  console.log(nums.filter( (num) => num % 2!== 0 )); 
  
  // We can chain methods on the output of the prev. method
  // Let's assume we want to filter out num < 5 and then double them
  //  We can use filter() first and then map() to double the result
  const doubled = nums
                  .filter( (num) => num < 5 )
                  .map( (num) => num * 2 );
  console.log(doubled);
  ```

## find

## reduce

- Executes the reducer function(callback) on each element of the array, resulting in a single value.
- Goal of `reduce()` is to just boil things down to a single value. It's up to us how we get there.
  - It applies the function against an accumulator and each element in the array (from left to right) to reduce it to a single value.
  
    ```js
    const nums = [1, 5, 6, 7, 9];
    
    // summing array elements...

    // nums.reduce(function (total, curr) {
    //   return total + curr;
    // });

    // nums.reduce( (total, curr) => {
    //  return total + curr;
    // });

    nums.reduce( (total, curr) => total + curr ); // ✔
    ```

    - **accumulatingValue**: The accumulated result of the previous callback calls (or the initialValue if provided).
    - **currentValue**: The current element being processed in the array.
    - **initialValue** (optional): A value to use as the first argument to the first call of the callback. If no initial value is provided, the first element of the array will be used as the initial accumulator.
      > At each iteration, first variable gets updated by the prev. return value.
       We can modify the way that variable gets updated to perform useful operations
       such as finding min, max or counting occurrences from an array using reduce().

    - Dry Run
  
      | Callback   | Accumulator | Current Value | Return Value |
      |------------|:-----------:|:-------------:|:------------:|
      | firstCall  |      1      |       5       |       6      |
      | secondCall |      6      |       6       |      12      |
      | thirdCall  |      12     |       7       |      19      |
      | fourthCall |      19     |       9       |      28      |
    ---
  - **Finding Minimum / Maximum Value**

      ```js
      nums.reduce( (max, curr) => {
        if(curr > max){
          return curr;
        }
        return max;
      });
      ```

    - Dry Run
  
      | Callback   | Maximum Value | Current Value | Return Value |
      |------------|:-----------:|:-------------:|--------------|
      | firstCall  |      1      |       5       |       5 > 1 = True, 5 is returned       |
      | secondCall |      5      |       6       |      6 > 5 = True, 6 is returned      |
      | thirdCall  |      6     |       7       |      7 > 7 = True, 7 is returned      |
      | fourthCall |      7     |       9       |      9 > 7 = True, 9 is returned      |
    ---

  - **Finding number of occurrences**
    - `0` is used as initial value.
    - Don't use increment / decrement operator inside `reduce()` as it leads to inconsistent results.

    ```js
    const numbers = [4, 2, 3, 4, 5, 23, 2, 4];
    numbers.reduce( (counter, curr) => {
      if(curr === 4){
        return counter + 1;   // don't use counter++ — !important
      }
      return counter;
    }, 0);
    ```

    - Dry Run

      | Callback    | Counter Value | Current Value |                  Return Value                  |
      |-------------|:-------------:|:-------------:|------------------------------------------------|
      | firstCall   |       0       |       4       | 4 === 4, counter incremented [counter + 1 = 1] |
      | secondCall  |       1       |       2       | 4 !== 4, counter remain same [counter = 1]     |
      | thirdCall   |       1       |       3       | 4 !== 3, counter remain same [counter = 1]     |
      | fourthCall  |       1       |       4       | 4 === 4, counter incremented [counter + 1 = 2] |
      | fifthCall   |       2       |       5       | 4 !== 5, counter remain same [counter = 2]     |
      | sixthCall   |       2       |       23      | 4 !== 23, counter remain same [counter = 2]    |
      | seventhCall |       2       |       2       | 4 !== 2, counter remain same [counter = 2]     |
      | eighthCall   |       2       |       4       | 4 === 4, counter incremented [counter + 1 = 3] |
      ---

## some & every

- `every()` is a boolean function that checks whether every element in the array meets the condition specified in the callback.
  - `every()` returns `true` iff all the elements meet the condition and false even if a single element fails to meet the condition.
- `some()` works in similar way to `every` but returns true even if single element holds the specified condition and `false` when all the elements fail to meet the condition.
  > **Early Exit:** `some()` stops iterating over once it finds an element satisfying the condition

  ```js
  const words = ['dog', 'jello', 'log', 'cupcake', 'bag', 'wage'];

  // Are there any words longer than 4 characters?
  words.some( (word) => word.length > 4 ); // true

  // Do any words starts with z?
  words.some( (word) => word[0] === 'z' ) // false

  // Do any word contains 'cake'?
  words.some((word) => word.includes('cake')) // true

  // Check whether every last letter of each word ends in "g"
  words.every( (word) => word[word.length - 1] === 'g' );
  ```

## Summary

- When we need to iterate over an array – we can use `forEach` , `for` or `for..of`.
- When we need to iterate and return the data for each element – we can use `map`.
  >To move from one state to another, we can also use `map`
    >> Doubling the original array is an example for state transitioning.
- When we need to make a subset from an array, we uses `filter`.
- When we need to calculate a single value based on the array we use `reduce`.
