# PHP Artisan

[Click here to read more about PHP Artisan](https://laravel.com/docs/10.x/artisan#introduction)

> Artisan is the command line interface included with Laravel. Artisan exists at the root of your application as the `artisan` script and provides a number of helpful commands that can assist you while you build your application.

| Command                                 | Description                                                                                                       |
|-----------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| `php artisan serve`                     | Starts a development server to serve your Laravel application locally.                                            |
| `php artisan route`                     | Displays a list of all registered routes for your application.                                                     |
| `php artisan route:list`                | Displays a formatted list of all registered routes, including route name, HTTP method, URI, action, middleware, and more. |
| `php artisan route:list --except-vendor` | Displays a list of all registered routes excluding routes defined in vendor packages.                              |
| `php artisan route:list --path=posts`   | Displays a list of all registered routes that match the given path prefix (`posts` in this example).              |

## Information about the `-h` flag

| Command                                 | Description                                                                                                       |
|-----------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| `php artisan route:list -h`             | Displays help information and usage instructions for the `route:list` command.                                   |
| `php artisan serve -h`                  | Displays help information and usage instructions for the `serve` command.                                          |
| `php artisan route -h`                  | Displays help information and usage instructions for the `route` command.                                          |
| `php artisan route:list -h`             | Displays help information and usage instructions for the `route:list` command.                                     |
| `php artisan route:list --except-vendor -h` | Displays help information and usage instructions for the `route:list` command excluding routes from vendor packages. |
| `php artisan route:list --path=posts -h`   | Displays help information and usage instructions for the `route:list` command for routes matching the `posts` path prefix. |

Using the `-h` flag with `php artisan` commands is a convenient way to quickly access help and usage instructions directly from the command line.
