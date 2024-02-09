# Laravel Routing

[Learn about Laravel Routes](https://laravel.com/docs/10.x/routing#redirect-routes)

## Rule Of Thumb

When defining routes in Laravel, it's a good practice to place more specific routes above more generalized ones to ensure that they are matched first.

```php
Route::get('/usr/{id}', function($id) {
    return "Passed ID is: $id";
});

Route::get('/usr/{id?}/{name}', function($id = null, $name = null) {
    if($id && $name) {
        return "id is: $id <br />and name is $name";
    } else {
        return "<h2>Incomplete Data...</h2> ";
    }
});
```

- The first route (`/usr/{id}`) handles cases where only the `id` segment is present in the URI.
- The second route (`/usr/{id?}/{name}`) handles cases where both the `id` and `name` segments are present.

Following the rule of thumb, we've placed the specific route (`/usr/{id}`) above the more generalized one (`/usr/{id?}/{name}`) to ensure that requests with only the `id` segment are handled before requests with both `id` and `name` segments.

## Basic Routing

In Laravel, routes are typically defined in the `routes/web.php` file for web routes and `routes/api.php` for API routes. You can define routes using the `Route` **facade**, specifying the HTTP method (`GET`, `POST`, `PUT`, `DELETE`, etc.) and the URI pattern.

```php
Route::get('/welcome', function () {
    return 'Welcome to Laravel!';
});

Route::get('/welcome', function (){
    return '<view_name>';
});

// Another way

Route::view('<route_name>', '<view_name>');
```

- If we want anchor tags `<a href=''>Login</a>`, we've to pass the route path and not the actual view that we want to render.

### Route Redirect

[Click here to learn about laravel route redirection](https://laravel.com/docs/10.x/routing#redirect-routes)

### Route Parameters

We can define route parameters to capture dynamic segments of the URI. Parameters are enclosed in `{}` braces.

```php
Route::get('/user/{id}', function ($id) {
    return 'User ID: ' . $id;
});
```

### Named Routes

We can assign names to routes to easily reference them in your application. This is useful for generating URLs or redirecting to named routes.

When we give our routes names, it makes managing them much simpler. If we've used a route in many parts of our code and need to change it, we have to update each reference separately. But if we use named routes, we only need to change it once, making things easier.

```php
Route::get('/profile', function () {
    // Logic here
})->name('profile');
```

- Access the named routes in blade templates like this:

```php
<a href="{{ route('profile') }}">User Profile</a>
```

### Route Prefixing

To add a common prefix to a group of routes to avoid repeating the same prefix in each route definition.

```php
Route::prefix('admin')->group(function () {
    Route::get('/dashboard', function () {
        // Logic here
    });

    Route::get('/users', function () {
        // Logic here
    });
});
```

## Laravel Route Constraints

Laravel provides various route constraints to filter route parameters. These constraints are useful for validating and restricting the format of route parameters.

### whereNumber

The `whereNumber` constraint ensures that the route parameter is a numeric value.

```php
Route::get('/user/{id}', function ($id) {
    // $id must be numeric
})->whereNumber('id');
```

### whereAlpha

The `whereAlpha` constraint ensures that the route parameter contains only alphabetic characters.

```php
Route::get('/user/{name}', function ($name) {
    // $name must contain only alphabetic characters
})->whereAlpha('name');
```

### whereAlphaNumeric

The `whereAlphaNumeric` constraint ensures that the route parameter contains only alphanumeric characters.

```php
Route::get('/product/{code}', function ($code) {
    // $code must contain only alphanumeric characters
})->whereAlphaNumeric('code');
```

### whereIn

The `whereIn` constraint restricts the route parameter to a predefined list of values.

```php
Route::get('/category/{category}', function ($category) {
    // $category must be one of the predefined values
})->whereIn('category', ['electronics', 'fashion', 'books']);
```

### where

The `where` method allows defining custom constraints using regular expressions.

```php
Route::get('/user/{username}', function ($username) {
    // Custom constraint for $username
})->where(['username' => '[A-Za-z]+']);
```

### Multiple Constraints

```php
Route::get('/user/{id}', function ($id) {
    // Route logic here
})->where([
    'id' => '[0-9]+', // Ensure $id is numeric
])->whereAlpha('name'); // Ensure $name contains only alphabetic characters
```
