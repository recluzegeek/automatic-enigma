# Laravel Controllers

## Role of Controllers in MVC Framework

## Working with Controllers

- Create Controller File
- Create Controller Class
- Create Route

### Creating Controllers using Artisan

- Create new Controller File and Controller Class using `php artisan` with the following command:

    ```php
    php artisan make:controller UserController
    ```

- Create new Route in the `web.php` file for our newly made `UserController` with the help of little bit messy php code.

```php
// UserController.php
<?php
    namespace App\Http\Controllers\UserController;
    use Illuminate\Http\Request;

    public class UserController extends Controller{
        public function show(){
            return view('user.profile'); // same as user/profile
        }
    }
```

```php
user App\Http\Controllers\UserController

Route::get('/user', [UserController::class, 'show']);
```

- In the above example, when the user hits our `localhost:8888/user` route, we want to show them `show()` method of `UserController::class`, which in turn returns the `user.profile` view.

#### Passing Route Variables to Views using Controllers

- Route Variables are passed directly to the `UserController` and then we can access the variable in the passed function

```php
Route::get('/usr/{id}', [UserController::class, 'showID']);
```

- **Grouping Related Controller Routes** as we would do with [Routes](Laravel%20Routing%20Basics.md)

```php
Route::controller(PageController::class)->group(function(){
    Route::get('/', 'showHome')->name('home');
    Route::get('/post', 'showPost')->name('posts');
    Route::get('/usr', 'showUsr')->name('usr');
});
```

### Single Action Controller

- Make invokable
