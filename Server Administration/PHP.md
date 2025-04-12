# PHP

Along with php installation, we need to have some php extensions to interact with services databases, files archiving (zipping, reading xml), using `php-xml`, `php-mysql`, `php-pgsql`, and `php-fpm`.

`php-cgi` is a protocol that standardizes the way server execute scripts to generate dynamic content. Its an interface that bridges the gap between web servers and server-side applications. `php-cgi` is platform independent, and it spawns new process for each request, leading to lower performance and inefficiency in high loads environment. There comes **FastCGI**, that optimizes the CGI protocol by maintaining a pool of persistent processes that can serve multiple request over their lifetime, handling multiple requests at once (multiplexing), and also saving precious cpu time of processes creation and killing. `php-fpm` is an implementation of FastCGI, tailored for `php`. `php-fmp` runs as a separate daemon process on the server. Web server (Nginx) forwards PHP requests to `php-fpm`. `php-fpm` executes the PHP application, processes the request, and sends the response back to nginx. Nginx on its own doesn't know how to respond to `php` requests.

## PHP Post Installation Guidelines

- [PHP/Laravel Driver Not Found Issue](https://laracasts.com/discuss/channels/laravel/php-artisan-migrate-could-not-find-driver)

- After the php installation, make sure to adjust the `php ini` file to accommodate the necessary changes like to interact with mysql, install the `php-mysql` extension for your distro.
- You can find the `ini` file location using `php --ini`
- Install the
