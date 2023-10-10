---
title: js_newer_javascript_features
updated: 2023-10-09 12:56:27Z
---

# Newer Javascript Features

## Goals

- Default Params
- Spread Operator
- Rest Params
- Destructuring

## Default Params

- Default parameters let us assign default values to function parameters.
  - Before default params, we'd to check whether a param is undefined using one-liner or whatever way suits you.
    > We can use `=` operator to assign default value to any param in the function definition
    >> **Use of undefined**: The default value is used only when the parameter is not provided or is explicitly passed as undefined. It won't override values like `null`, `false`, `0`, or `an empty string`.
    >>> **Position of default parameters**: Default parameters must be listed after non-default parameters. For example, in the function example(a, b = 0), a is a non-default parameter, and b is a default parameter.
    >>>>
    >>>>```js
    >>>>function greet(name, greet = 'Hey there', punc = '!'){
    >>>>     console.log(`${greet}, ${name}${punc}`);
    >>>> }
    >>>> 
    >>>> greet('Recluze');   // Hey there, Recluze! - Working fine
    >>>> greet('Recluze', 'Hello');   // Hello, Recluze! - Working fine
    >>>> greet('Recluze', '!!!!!');  // !!!!!, Recluze!  - Inconsistency / Incorrect
    >>>>```

    - Usage Demonstration

    ```js
    // Before the advent of Default Params
    function multiply(a, b){
        // checks if value of b is passed or not
        // if not passed, it sets its value to 1
        // otherwise, to b
        b = (typeof b === 'undefined') ? 1 : b;
        return a * b;
    }

    // After Default Params introduced we can skip condition check
    function multiply(a, b = 1){
        return a * b;
    }
    ```

## Spread Operator

- Spread operator (`...`) allows us to expand elements from an iterable (like an array or a string) into individual elements.
  > It expands an iterable for function calls where zero or more elements are expected.
  >> It's not limited just to  arrays or strings. It works with any other data structure that can be iterated using `for...of`.
  >>> Useful when working with arrays, creating new arrays, or manipulating strings.

### Spread in Function Calls

- In the code below, spread operator is expanding the `array nums` and then passing each element to `Math.max()`.

  ```js
  // Math.max() can take as many params as we want
  Math.max(12, 32, 43, 2, 4, 64, 54);   // prints 64

  // What happens when we pass an integer array to max()
  // Sure, it prints NaN. Let's try out
  const nums = [12, 32, 43, 2, 4, 64, 54];
  Math.max(nums);    // NaN

  // Here comes Spread operator to rescue us!
  // It spreads / expands **cough cough** the array and pass individual element 
  // to the function
  Math.max(...nums)  // This works...

  // More things to try
  console.log(nums);
  console.log(...nums);

  console.log('Javascript');
  console.log(...'Javascript');
  ```

### Spread with Arrays / Array Literals / Strings

- Creates a new array using an existing array.
  - Spreading the element from one array into a new array.

    ```js
    const intro = ['My', 'name', 'is'];
    const name = ['Recluze', 'Geek'];
    
    // We can combine/copy both of these arrays into a new array using
    // slice() or some other methods. But we'll stick with spread.
    const introduction = [...intro, ...name];
    console.log(introduction);

    // We can also tweak the array in its creation statement
    const anotherIntro = [1, 2, 3, ...intro, ...name, '!!!!!']
    ```

### Spread with Objects

- Copies properties from one object into another object literal.
  >  Can be used to create a shallow copy of an object.
  >> A **shallow copy** means that the top-level properties of the object are copied, but if the object contains nested objects (or references), those nested objects are still shared between the original and the copied object.
  >>>
  >>>```js
  >>>// Original object
  >>>  const originalObject = { name: 'John', age: 25, hobbies: ['reading', 'coding'] };
  >>>
  >>>  // Creating a shallow copy using the spread operator
  >>>const copiedObject = { ...originalObject };
  >>>
  >>>  // Modifying the copied object
  >>> copiedObject.name = 'Jane';
  >>> copiedObject.hobbies.push('gardening');
  >>>
  >>>// Displaying both objects
  >>>console.log('Original Object:', originalObject);
  >>>console.log('Copied Object:', copiedObject);
  >>>
  >>> // Expected Output
  >>> Original Object: { name: 'John', age: 25, hobbies: ['reading', 'coding', 'gardening'] }
  >>> Copied Object: { name: 'Jane', age: 25, hobbies: ['reading', 'coding', 'gardening'] }
  >>>```

  - **Explanation**
    - The `spread` operator is used to create `copiedObject` by spreading the properties of `originalObject`.
    - When we modify the `name` property and push a new hobby into `copiedObject`, the `originalObject` remains unchanged.
    - However, note that the `hobbies` array is a reference to the same array in both objects. Modifying the array in one object affects the other.
  - **Real World Usage Example**

    ```js
    const dataFromForm = {
        email: 'john23@gmail.com',
        username: 'johnbuge',
        password: 'agIkw092l1$^k!2'
    };

    const newUser = {...dataFromForm, id: 201884763, isAdmin: false};
    console.log(newUser);
    ```

## Rest Params

- It's syntax is exactly alike to spread operator but doesn't behave like that. Spread expands out the params while Rest collects in the params.
- If we want our function to get as many params as we want, we can take advantage of Rest params. It's an actual array that holds the arguments passed into the function.
  - Collects all remaining arguments into an actual array

### The Arguments Object

- Available inside every function.
- It's an **array-like** object.
  - Has a `length / index` properties
  - Doesn't have array methods like `push / pop / reduce`
- Contains all the arguments passed to the function
- Not available inside of arrow functions!

- Code Demonstration

    ```js
    // using function expression
    function sum(...nums){
        // using purely function expression
        // and it's a bit verbose as compared to arrow function
            // return nums.reduce( function (total, el) {
            //    return total + el;
            // });

        // using with arrow syntax
        return nums.reduce( (total, el) =>{
            return total + el;
        });
    }

    // using arrow function and Rest params
    // finding sum of squares
    let squareSum = (...nums) => {
        console.log(`Passed Arguments are : ${nums}`);
        return nums.reduce( (sqSum, el) => {
            return sqSum += el * el;
        });
    };

    // we can have variable number of arguments
    squareSum(9, 10) // 2 args
    squareSum(9, 18, 27) // 3 args
    squareSum(9, 81, 162, 334) // 4 args
    ```

- We can also have named parameters alongside `rest param` since its name is `rest parameters` makes sense. Refer below

    ```js
    const resultProfile = (name, rollNum, ...subjMarks) => {
        console.log(`Name: ${name} \t\t Roll Num: ${rollNum}`);
        return subjMarks.reduce( (total, subjMark) => {
            return total + subjMark;
        });
    }
    const marksObtd = resultProfile('John', 345232, 89, 71, 82, 95, 79);
    console.log(`Obtained Marks: ${marksObtd}/500`);
    ```

## Destructuring

- Destructuring let us unpack / extracting / single-out values from arrays or properties from objects into distinct variables.
  - We can also assign default values to variables in case the said property doesn't exist. (doesn't make much an impact on arrays, but sure on objects and functions).
    >
    > - In the case of arrays, variables receive their **values based on their positions** or indices within the array. The order of variables on the left side of the destructuring assignment corresponds to the indices in the array.
    > - **var1** is at pos 0; var1 = arr[0];
    > - **var2** is at pos 1; var2 = arr[1];
    > - and so on...
    >
    > ```js
    > const [ var1, var2, var3, ...remainingVar ] = array name from which destructuring will happen ;
    > ```
    >
    >> - Object properties get their **values based on their names.**
    >> - We can also **rename** the variables: `const { born: birthYear } = user;`
    >>   - We've renamed born to birthYear using a semicolon`:`.
    >> - We can also set **default values** to  the variables: `const { died: deathYear = 'N/A' } = user;`
    >>   - We've renamed as well as set the default value. In case deathYear is not specified, its' default to `N/A.`
    >>
    >>> Default values in object destructuring are handy when we're extracting properties from objects that might not have certain properties. They help us set fallback values, ensuring variables get a default if the property is missing, avoiding errors. Useful when dealing with multiple objects where the structure may vary.
    >>
    >>```js
    >> const { prop1_name, prop2_name, ...remainingProps } = object name from which destructuring will happen.
    >> const { prop1_name : property1 = 'default value', prop2_name = 'default value', ...remainingProps } = object name from which destructuring will happen.
    >> ```

- Array Destructuring

    ```js
    // Example with an array
    const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];

    // Destructuring assignment
    // We can skip an index using a comma, and it will be ignored
    const [first, second, , fourth, ...everythingElse] = numbers;

    // Explanation:
    // first gets the value at index 0: numbers[0] = 1
    // second gets the value at index 1: numbers[1] = 2
    // fourth gets the value at index 3: numbers[3] = 4
    // everythingElse copies the remaining elements, excluding index 2 (skipped with a comma)

    console.log(first);          // Output: 1
    console.log(second);         // Output: 2
    console.log(fourth);         // Output: 4
    console.log(everythingElse); // Output: [5, 6, 7, 8, 9]
    ```

- Object Destructuring

    ```js
    // Configuration object
    const config = {
        serverUrl: 'https://api.example.com',
        timeout: 5000,
        logging: true,
    };

    // Destructuring assignment with default values and variable renaming
    const { serverUrl, timeout, logging: enableLogging = false, retryAttempts: maxRetries = 3 } = config;

    // Logging the extracted values
    console.log(`Server URL: ${serverUrl}`);
    console.log(`Timeout: ${timeout} milliseconds`);
    console.log(`Enable Logging: ${enableLogging}`);
    console.log(`Max Retry Attempts: ${maxRetries}`);

    ```

- Parameter Destructuring
  - Destructure objects or arrays directly in the function parameter list.
  - We still need to call the function with the object and we'll destructure the object in function definition.

    ```js
    const user = {
        firstName: 'John',
        lastName: 'Doe',
        username: 'jsCoder',
        email: 'jsCoder@example.com',
        password: 'securePassword123',
        age: 30
        rememberMe: true
    };

    // Calling the function with an object !important
    authenticateUser(userCredentials);

    // Example function with parameter destructuring
    const authenticateUser = ({ username, password, rememberMe = false }) => {
        console.log(`Authenticating user: ${username}, Remember Me: ${rememberMe}`);
    };
    ```

  - Examples

    ```js
    const movies = [
        { title: "Inception", year: 2010, rating: 9.3 },
        { title: "The Shawshank Redemption", year: 1994, rating: 9.2 },
        { title: "The Dark Knight", year: 2008, rating: 9.0 },
        { title: "Pulp Fiction", year: 1994, rating: 8.9 },
        { title: "Forrest Gump", year: 1994, rating: 8.8 },
    ];

    // Without using param destructuring
        // const highRatedMovies = movies.filter( (movie) => movie.rating >= 9);

    // with param destructuring
    const highRatedMovies = movies.filter(({ rating: score }) => score >= 9 );

    console.log(highRatedMovies)

    // without param destructuring
        // const movieInfo = movies.map((movie) => {
        //     console.log(`${movie.title} (${movie.year}) is rated as ${movie.rating}`);
        // });

    // with param destructuring
    const movieInfo = movies.map(({ title, year, rating }) => {
        console.log(`${title} (${year}) is rated as ${rating}`);
    });
    ```
