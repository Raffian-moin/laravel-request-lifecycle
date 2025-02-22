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
- [Does Laravel uses application development configs or framework configs for configuration of the application?](#does-laravel-uses-application-development-configs-or-framework-configs-for-configuration-of-the-application)

## What is Laravel?
Laravel is a popular open-source PHP MVC framework created by Taylor Otwell.

## Does Laravel Restrict Developers to Follow the MVC (Model-View-Controller) Pattern?
No, Laravel does not strictly enforce the MVC pattern. Developers can build applications using different architectural approaches if needed.

## What is the Entry Point of a Laravel Application?
The entry point of a Laravel application is the `public/index.php` file. This file initializes the application, loads configurations, and handles incoming requests.

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

### Does Laravel uses application development configs or framework configs for configuration of the application?

Laravel uses both application-specific and framework-level configuration files. Your application’s configuration files reside in the `config` directory, while the framework’s default configurations are in the `vendor/laravel/framework/config` directory.

These configurations are merged during the bootstrap process by the `LoadConfiguration` class located at `vendor/laravel/framework/src/Illuminate/Foundation/Bootstrap/LoadConfiguration.php`, ensuring that your application settings override the framework defaults.

![Configuration Merge Method](/assets/configuration_merge_method.png)


