# PHP Basics

PHP (Hypertext Preprocessor) is a server-side scripting language used primarily for web development. Below are some fundamental concepts of PHP:

## PHP Learning Resources

- [PHPApprentice](https://phpapprentice.com)
- [PHPTheRightWay](https://phptherightway.com/)

## Syntax and Variables

```php
<?php
    // PHP code starts with <?php and ends with ?>
    $name = "John"; // Variable declaration
    $age = 25;
    echo "Hello, $name!"; // Output: Hello, John!
    echo "Sum of 2 + 2 is " . (2+2);
?>
```

## Single vs Double Quotes

In PHP, both single quotes (`''`) and double quotes (`""`) can be used to define strings, and each has its own use case.

### Single Quotes (`''`)

- String literals are treated literally, meaning escape sequences (like `\n` for newline) and variable interpolation (e.g., `$name`) are not parsed.
  - Single quotes are generally faster because PHP doesn't have to parse the string for variables or escape sequences.

### Double Quotes (`""`)

- String literals are parsed, meaning escape sequences and variable interpolation are processed.
  - Double quotes are typically used when you want to include variables or escape sequences within the string.

   ```php
   // Taken from Stackoverflow
    $s = "dollars";
    echo 'This costs a lot of $s.'; // This costs a lot of $s.
    echo "This costs a lot of $s."; // This costs a lot of dollars.
   ```

### `echo` and `print`

### `echo`

- `echo` is a language construct, not a function, so the parentheses are optional.
  - It can take multiple parameters and does not return a value.
  - It's slightly faster compared to `print`.

   ```php
   <?php
        echo "Hello, world!";
        echo "This ", "is ", "a ", "concatenated ", "string.";
   ?>
   ```

### `print`

- `print` is a function, so the parentheses are required.
  - It can only take one argument and returns 1, always.
  - It's slightly slower compared to `echo`.

   ```php
   <?php
    print "Hello, world!";
   ?>
   ```

- In PHP, when you use double quotes ("), escape sequences like `\n` are interpreted as newlines. However, if you want to output a newline on HTML pages or in text, you need to use the HTML newline character `<br>` instead of `\n`.

### `php -S localhost:8888`

- This command is used to start a simple PHP server locally.
- `-S` flag specifies the address and port to run the server.
- `localhost:8888` means the server will be accessible at `http://localhost:8888` in the browser.

  - The built-in PHP development server does not automatically restart when you make changes to your PHP files.

### Difference between `index.html` and `index.php`

- PHP files are typically used for dynamic content generation, while HTML files are used for static content presentation. Using `index.php` allows you to include dynamic content and interact with server-side resources.

1. **index.html**:
   - HTML file.
   - Contains static content.
   - Cannot execute PHP code.

   ```html
   <!DOCTYPE html>
   <html>
    <head>
        <title>HTML Page</title>
    </head>
    <body>
        <h1>Hello, world!</h1>
    </body>
   </html>
   ```

2. **index.php**:
   - PHP file.
   - Can contain both HTML and PHP code.
   - PHP code can be executed within `<?php ?>` tags.

   ```php
   <!DOCTYPE html>
   <html>
    <head>
        <title>PHP Page</title>
    </head>
    <body>
        <h1><?php echo "Hello, world!"; ?></h1>
    </body>
   </html>
   ```

## Data Types

- String: `"Hello"`
- Integer: `25`
- Float: `3.14`
- Boolean: `true` or `false`
- Array: `["apple", "banana", "cherry"]`
- Object: `$person = new Person()`

## Operators

- Arithmetic: `+`, `-`, `*`, `**`, `/`, `%`
- Assignment: `=`, `+=`, `-=`, `*=`, `/=`, `%=`
- Comparison: `==`, `!=`, `>`, `<`, `>=`, `<=`
- Logical: `&&`, `||`, `!`

## Control Structures

### If Statement

```php
<?php
    if ($age >= 18) {
        echo "You are an adult.";
    } else {
        echo "You are a minor.";
    }
?>
```

### Loops

- **While Loop**

```php
<?php
    $i = 0;
    while ($i < 5) {
        echo $i;
        $i++;
    }
?>
```

- **For Loop**

```php
<?php
    for ($i = 0; $i < 5; $i++) {
        echo $i;
    }
?>
```

## Functions

```php
<?php
    function greet($name) {
        echo "Hello, $name!";
    }
    greet("Alice"); // Output: Hello, Alice!
?>
```

## Arrays

```php
<?php
    $fruits = ["apple", "banana", "cherry"];
    echo $fruits[0]; // Output: apple
?>
```

## Classes and Objects

```php
<?php
    class Person {
        public $name;
        public $age;
        
        function __construct($name, $age) {
            $this->name = $name;
            $this->age = $age;
        }
        
        function greet() {
            echo "Hello, my name is $this->name.";
        }
    }

    $person = new Person("John", 25);
    $person->greet(); // Output: Hello, my name is John.
?>
```

## File Handling

```php
<?php
    $file = fopen("example.txt", "r") or die("Unable to open file!");
    echo fread($file, filesize("example.txt"));
    fclose($file);
?>
```

## Error Handling

```php
<?php
    try {
        // Code that may throw an exception
        throw new Exception("An error occurred!");
    } catch (Exception $e) {
        echo "Error: " . $e->getMessage();
    }
?>
```

## Database Interaction

```php
<?php
    $conn = mysqli_connect("localhost", "username", "password", "database");
    $sql = "SELECT * FROM users";
    $result = mysqli_query($conn, $sql);
    while ($row = mysqli_fetch_assoc($result)) {
        echo $row['username'];
    }
    mysqli_close($conn);
?>
```
