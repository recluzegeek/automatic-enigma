# PHP Functions

## Anonymous / Lambda Functions (Closures)

- Anonymous functions, also known as closures, are functions without a specified name. They're analogous to callback functions in JS.
  - They are defined using the `function` keyword followed by optional parameters and function body.
  - Anonymous functions can be assigned to variables, passed as arguments to other functions, or returned from other functions.

    ```php
    $addition = function($a, $b) {
        return $a + $b;
    };

    echo $addition(2, 3); // Output: 5
    ```

## Variable-length Argument Lists

- PHP functions can accept a variable number of arguments using the `func_num_args()`, `func_get_arg()`, and `func_get_args()` functions.

    ```php
    function sum() {
        $total = 0;
        $args = func_get_args();
        foreach ($args as $arg) {
            $total += $arg;
        }
        return $total;
    }

    echo sum(1, 2, 3, 4); // Output: 10
    ```

## Default Parameters

- PHP functions can have default parameter values. If no value is provided for a parameter when the function is called, the default value is used.

    ```php
    function greet($name = "Guest") {
        echo "Hello, $name!";
    }

    greet(); // Output: Hello, Guest
    greet("John"); // Output: Hello, John
    ```

## Return Type Declarations (PHP 7+)

- PHP 7 introduced support for return type declarations, allowing you to specify the expected data type of the return value of a function.

    ```php
    function add(int $a, int $b): int {
        return $a + $b;
    }

    echo add(2, 3); // Output: 5
    ```
