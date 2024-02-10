# Laravel Blade Template

[Click here to learn more about Laravel Blade template](https://laravel.com/docs/10.x/blade#displaying-data)

- Blade templates provide easy and convenient way to create views.
- It's  a template engine based on PHP.
  - We can create dynamic and reusable templates.

## Blade Template Syntax

- **Echoing Content**: Use `{{ }}` to output content onto the webpage directly. This is useful for static content or simple variable echoes.
  
  ```php
  {{ 'Hello, World!' }}
  ```

- **Echoing Variables**: Use `{{ $var_name }}` to output the value of a variable onto the webpage. This is commonly used for dynamic content.

  ```php
  {{ $user->name }}
  ```

- **Displaying Unescaped Data &mdash; Execute HTML, JS**: Use `{!! !!}` to execute HTML or JavaScript code directly onto the webpage. Be cautious when using this to prevent Cross-Site Scripting (XSS) vulnerabilities.

  ```php
  {!! '<h1>Hello, World!</h1>' !!}
  {!! '<script>alert("Hello, World!");</script>' !!}
  ```

- **Laravel Construct Syntaxes**: Use various constructs provided by Laravel such as `@for`, `@while`, `@foreach`, `@forelse`, `@if`, `@elseif`, `@endif`, etc., for controlling the flow of your Blade templates.

  ```php
  @foreach($users as $user)
      {{ $user->name }}
  @endforeach
  ```

  Example with `@if` and `@endif`:

  ```php
  @if($user->isAdmin)
      <p>This user is an administrator.</p>
  @endif
  ```

  Example with `@forelse` and `@endforelse`:

  ```php
  @forelse($users as $user)
      {{ $user->name }}
  @empty
      <p>No users found.</p>
  @endforelse
  ```

### Blade Template Loop Variables

Blade templates provide loop variables that can be used within loop constructs such as `@foreach`, `@forelse`, etc. These loop variables provide additional information about the current state of the loop.

| Loop Variable  | Description                                                        |
|----------------|--------------------------------------------------------------------|
| `$loop->index` | The zero-based index of the current iteration (starts from 0).      |
| `$loop->iteration` | The current iteration number (starts from 1).                   |
| `$loop->first` | `true` if it's the first iteration, otherwise `false`.             |
| `$loop->last`  | `true` if it's the last iteration, otherwise `false`.              |
| `$loop->count` | The total number of items in the loop.                             |
| `$loop->depth` | The current loop depth, useful for nested loops (starts from 1).    |
| `$loop->parent` | The parent loop's variables when inside nested loops.             |

## Blade Template Main Directives

- These main directives are used to create reusable templates
  - `@include()`
  - `@section()`
  - `@extend()`
  - `@yield()`

### Including Subviews using `@include()`

- We can include one blade template file into other as in EJS or any templating engine would do. We've `Header.blade.php` and `Footer.blade.php` file, that we want to include in our `Home.blade.php`. We can easily do with the help of `@include()` blade directive.

  - **`Header.blade.php`**

    ```php
    <h1>Header Section</h1>
    ```

  - **`Footer.blade.php`**

    ```php
    <h1>Footer Section</h1>
    ```

    - **`Home.blade.php`**

      ```php
      @include('Header')
      <h1>Welcome to this World!</h1>
      @include('Footer')
      ```

    - We can pass data into subviews using **associative arrays**. Let's modify our **`Header.blade.php`** and **`Home.blade.php`** to echo our name onto the webpage

      - **`Header.blade.php`**

      ```php
      <h1>Header Section</h1>
      {{ 'Hello ' . $name . ' !' }}
      ```

      - **`Home.blade.php`**

      ```php
      @include('Header', ['name' => 'recluzegeek'])
      <h1>Welcome to this World!</h1>
      @include('Footer')
      ```

#### `includeIf()`

- Include a Blade view file if it exists. This means you can conditionally include a view based on whether it's present in your application's file structure.

  ```php
  @includeIf('view.name', ['data' => 'value'])
  ```

- `'view.name'`: The name of the Blade view file you want to include.
- `['data' => 'value']` (optional): Data to pass to the included view.

#### Including Subviews with `@includeWhen()` and `@includeUnless()`

When you need to include views conditionally based on certain criteria, you can use `@includeWhen()` and `@includeUnless()` directives in Blade. These directives simplify the process of including views based on whether a condition is met or not.

#### `@includeWhen()`

The `@includeWhen()` directive includes a Blade view file if a specified condition is true. Here's the syntax:

```php
@includeWhen(condition_value, 'view_file', ['name' => 'recluzegeek'])
```

- `condition_value`: The condition that must be evaluated to true or false.
- `'view_file'`: The name of the Blade view file to include if the condition is true.
- `['name' => 'recluzegeek']` (optional): Data to pass to the included view.

If the `condition_value` is true, the view specified by `'view_file'` will be included. Otherwise, it won't be included.

#### `@includeUnless()`

Conversely, the `@includeUnless()` directive includes a Blade view file if a specified condition is false:

```php
@includeUnless(condition_value, 'view_file', ['name' => 'recluzegeek'])
```

- `condition_value`: The condition that must be evaluated to true or false.
- `'view_file'`: The name of the Blade view file to include if the condition is false.
- `['name' => 'recluzegeek']` (optional): Data to pass to the included view.

If the `condition_value` is false, the view specified by `'view_file'` will be included. Otherwise, it won't be included.

### Template Inheritance  using `@extend()`, `@yield()` & `@section`

- Template inheritance in Blade simplifies the process of creating consistent layouts for web pages in Laravel applications.
  - Start by creating a `masterlayout.blade.php` file. This file defines the overall structure of your web pages. You can include placeholders for specific content sections using the `@yield(name)` directive.
  - You can provide default or fallback values for `@yield()` directives in Blade templates. This ensures that if no value is provided for a particular section, a default value will be displayed instead. This is achieved using the `@yield('name', 'default_value')` directive.

```php
<!-- masterlayout.blade.php -->

<html>
<head>
    <title>@yield('title')</title>
</head>
<body>

<header>
    @yield('header')
</header>

<main>
    @yield('content', 'Content not found.')
</main>

<footer>
    @yield('footer')
</footer>

</body>
</html>
```

#### Extend Master Layout

- Extend the master layout to create specific pages with unique content. Use the `@extends()` directive to extend the `masterlayout.blade.php` file. Then, fill in the placeholders using the `@section(name)` and `@endsection()` directives.

```php
<!-- home.blade.php -->

@extends('masterlayout')

@section('title', 'Home Page')

@section('header')
    <h1>Welcome to My Website</h1>
@endsection

@section('content')
    <p>This is the content of the home page.</p>
@endsection

@section('footer')
    <p>Contact us at example@example.com</p>
@endsection
```

#### `hasSection()`

The `@hasSection()` directive in Blade is used to check if a given section has been defined in the current template. It returns `true` if the section has been defined, and `false` otherwise.

```php
@hasSection('sidebar')
    <div class="sidebar">
        @yield('sidebar')
    </div>
@else
    <p>No sidebar content available.</p>
@endif
```

- If the `sidebar` section is defined in the extending view, the content within the `@hasSection('sidebar') ... @else ... @endif` block will be executed.
- If the `sidebar` section is not defined, the content within the `@else ... @endif` block will be executed.

## Javascript in Blade using `@json()`

The `@json()` directive allows you to easily output JSON-encoded data directly into your JavaScript code. This is useful when you need to pass server-side data to your client-side JavaScript.

  ```php
  <script>
      var jsonData = @json($data);
  </script>
  ```

- `$data`: The server-side data (e.g., an array or object) that you want to output as JSON.

  ```php
  @php
      $user = ['name' => 'John', 'age' => 30];
  @endphp

  <script>
      var userData = @json($user);
      console.log(userData);
  </script>
  ```

- The `$user` array is passed to the `@json()` directive.
- The `@json($user)` directive outputs the JSON representation of the `$user` array directly into the JavaScript code.

### `@stack()` and `@push()` Directives

- In Laravel's Blade templating engine, the `@stack` and `@push` directives provide a convenient way to manage and render content from multiple views or components.
  > You can ask that `@section()` directive does the same thing as `@push()` does so whats the point of using it over something that we already know. And the answer is, if you've multiple `@section()` for the same yield name, Blade will only parse the `@section()` and will simply ignore the others. But in case of `@push()`,  every `@push()` so to say :) will be pushed onto stack and will be parsed by the Blade.

| Directive   | Purpose                                                | Behavior                                                     |
|-------------|--------------------------------------------------------|--------------------------------------------------------------|
| `@section()`| Defines content for a specific section                 | Only the first `@section()` for a section name is parsed      |
| `@push()`   | Adds content to a stack, allowing multiple additions  | Every `@push()` is appended to the stack and parsed          |

#### `@push` Directive

The `@push` directive allows you to add content to a stack. If the stack does not exist, it will be created.

```php
@push('scripts')
    <script>
        // JavaScript code
    </script>
@endpush
```

#### `@stack` Directive

The `@stack` directive allows you to render the content of a stack at a specific place in your layout.

```php
<head>
    <!-- Other head content -->
    @stack('scripts')
</head>
```
