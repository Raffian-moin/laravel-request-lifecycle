# Laravel Request Lifecycle

## Table of Contents
- [What is Laravel?](#what-is-laravel)
- [Does Laravel Restrict Developers to Follow the MVC Pattern?](#does-laravel-restrict-developers-to-follow-the-mvc-pattern)
- [What is the Entry Point of a Laravel Application?](#what-is-the-entry-point-of-a-laravel-application)
- [How Does Laravel Handle Routes Without .php Extensions?](#how-does-laravel-handle-routes-without-php-extensions)
- [What is the First Thing That Laravel Does Internally?](#what-is-the-first-thing-that-laravel-does-internally)
- [Laravel File Structure](#laravel-file-structure)
  - [Application Development Framework Structure](#application-development-framework-structure)
  - [Core Framework Structure](#core-framework-structure)
- [What is service container?](#what-is-service-container)
- [Does Laravel uses application development configs or framework configs for configuration of the application?](#does-laravel-uses-application-development-configs-or-framework-configs-for-configuration-of-the-application)

## What is Laravel?
Laravel is a popular open-source PHP MVC framework created by Taylor Otwell.

## Does Laravel Restrict Developers to Follow the MVC (Model-View-Controller) Pattern?
No, Laravel does not strictly enforce the MVC pattern. Developers can build applications using different architectural approaches if needed.
For example, you can write code that bypasses the use of controllers or models entirely. However, for larger, maintainable, and scalable applications, following the MVC pattern generally helps in organizing your code and separating concerns.

```php
Route::get('/users-list', function () {
    $users = DB::table("users")->get();
    return view('users-list', compact("users"));
});
```

In this example, the code directly handles data without interacting with a controller or using a model to retrieve data from the database. Although this approach can work for small projects or quick prototypes, it might lead to challenges as the application grows in complexity.

## What is the Entry Point of a Laravel Application?

The entry point for a Laravel application depends on the type of request:

- **Web Requests:**
  The `public/index.php` file is the entry point. It loads the Composer autoloader, bootstraps the framework, loads the necessary configuration, and handles incoming HTTP requests.

- **Console Requests:**
  The `artisan` file, located in the root directory of the application, serves as the entry point for console commands. It initializes the application for command-line operations.

This design allows Laravel to properly initialize and manage the application context whether it’s handling a web request or a console command.

## How Does Laravel Handle Routes Without .php Extensions?
In traditional PHP applications, URLs map directly to `.php` files (e.g., `example.com/home.php`). Laravel, however, uses a routing system that maps URLs to controller methods or closure functions.

Laravel achieves this clean URL structure through:
1. **Apache/Nginx Configuration:** Laravel applications use `.htaccess` (for Apache) or server block configuration (for Nginx) to route all requests to `public/index.php`.
2. **Routing System:** Laravel's `routes/web.php` and `routes/api.php` define application routes, mapping URLs to appropriate controllers or logic.
3. **Front Controller Pattern:** The `index.php` file serves as a single entry point that directs all requests to Laravel’s core.

## What is the First Thing That Laravel Does Internally?
When a request reaches Laravel, the framework performs the following steps:
1. **Autoloading:** Composer’s autoloader loads necessary classes.
2. **Bootstrapping:** The `bootstrap/app.php` file creates a new Laravel application instance.
3. **HTTP Kernel Processing:** Laravel executes middleware, processes the request, and routes it to the correct controller or closure.
4. **Response Generation:** The application generates an HTTP response and sends it back to the browser.

## Laravel File Structure

### Application Development Framework Structure
The Laravel framework provides a structured directory layout for application development:

- **`app/`** – Contains the core application logic (Models, Controllers, Middleware, etc.).
- **`bootstrap/`** – Contains application bootstrapping files.
- **`config/`** – Stores configuration files.
- **`database/`** – Manages migrations, seeders, and factories.
- **`public/`** – Contains `index.php`, the entry point, along with assets like images, JavaScript, and CSS files.
- **`resources/`** – Holds views, language files, and frontend assets.
- **`routes/`** – Contains route definition files (`web.php`, `api.php`, etc.).
- **`storage/`** – Stores logs, caches, and uploaded files.
- **`tests/`** – Contains unit and feature tests.
- **`vendor/`** – Stores dependencies installed via Composer.

### Core Framework Structure
Laravel's core framework resides in the `vendor/laravel/framework` directory and follows this structure:

- **`src/Illuminate/`** – Contains Laravel’s core components (Routing, HTTP, Console, etc.).
- **`src/Illuminate/Foundation/`** – Houses the main application bootstrapping logic.
- **`src/Illuminate/Routing/`** – Manages request routing.
- **`src/Illuminate/Database/`** – Handles ORM (Eloquent), migrations, and query builder.
- **`src/Illuminate/Http/`** – Processes HTTP requests and responses.
- **`src/Illuminate/Support/`** – Provides helper functions and utilities.

## What is service container?

Imagine you need a variety of groceries—vegetables, meat, and other essentials—for your home. Instead of buying each item from different specialized shops, you prefer the convenience of a super shop that has everything under one roof.

In Laravel, core services such as session, cookie, and database are like those groceries. The service container in Laravel acts as your super shop. When you request a service, the container doesn’t provide you with a raw, unprocessed component (like a live cow when you need meat); instead, it automatically resolves all of the service’s dependencies and returns a fully constructed, ready-to-use instance. This streamlined process simplifies dependency management and ensures that every service you work with is properly configured and ready for immediate use.

## Does Laravel use application development configs or framework configs for configuration of the application?

Laravel uses both application-specific and framework-level configuration files. Your application’s configuration files reside in the `config` directory, while the framework’s default configurations are in the `vendor/laravel/framework/config` directory.

These configurations are merged during the bootstrap process by the `LoadConfiguration` class located at `vendor/laravel/framework/src/Illuminate/Foundation/Bootstrap/LoadConfiguration.php`, ensuring that your application settings override the framework defaults.

```php
/**
     * Load the configuration items from all of the files.
     *
     * @param  \Illuminate\Contracts\Foundation\Application  $app
     * @param  \Illuminate\Contracts\Config\Repository  $repository
     * @return void
     *
     * @throws \Exception
     */
    protected function loadConfigurationFiles(Application $app, RepositoryContract $repository)
    {
        $files = $this->getConfigurationFiles($app);

        $shouldMerge = method_exists($app, 'shouldMergeFrameworkConfiguration')
            ? $app->shouldMergeFrameworkConfiguration()
            : true;

        $base = $shouldMerge
            ? $this->getBaseConfiguration()
            : [];


        foreach (array_diff(array_keys($base), array_keys($files)) as $name => $config) {
            $repository->set($name, $config);
        }

        foreach ($files as $name => $path) {
            $base = $this->loadConfigurationFile($repository, $name, $path, $base);
        }

        foreach ($base as $name => $config) {
            $repository->set($name, $config);
        }
    }
```


