# PHP Loops

- **`for` Loop**: Executes a block of code a specified number of times.

    ```php
    for ($i = 0; $i < 5; $i++) {
        echo "The number is: $i <br>";
    }
    ```

- **`while Loop`**: Executes a block of code as long as the specified condition is true.

    ```php
    $i = 0;
    while ($i < 5) {
        echo "The number is: $i <br>";
        $i++;
    }
    ```

- **`do...while Loop`**: Similar to the while loop, but it will always execute the block of code at least once, even if the condition is false.

    ```php
    $i = 0;
    do {
        echo "The number is: $i <br>";
        $i++;
    } while ($i < 5);
    ```

- **`foreach Loop`**: Iterates over arrays and objects.

    ```php
    $fruits = array("Apple", "Banana", "Orange");
    foreach ($fruits as $fruit) {
        echo "I like $fruit <br>";
    }
    ```
  
  - The below version of `foreach` allows embedding HTML markup directly within the loop, making the code more readable and maintainable.

    ```php
    <body>
        <h1>List of Books</h1>
        <ul>
            <?php foreach ($books as $book) : ?>
                <li><?= $book ?></li>
            <?php endforeach; ?>
        </ul>
    </body>
    ```

- **`Nested Loops`**: You can also nest loops within each other to create complex iteration patterns.

    ```php
    for ($i = 0; $i < 3; $i++) {
        for ($j = 0; $j < 3; $j++) {
            echo "($i, $j) <br>";
        }
    }
    ```
