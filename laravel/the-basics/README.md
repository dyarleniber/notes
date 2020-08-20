[Back](../README.md)

# The Basics


## Routing

All Laravel routes are defined in your route files, which are located in the `routes` directory. These files are automatically loaded by the framework. The `routes/web.php` file defines routes that are for your web interface. These routes are assigned the `web` middleware group, which provides features like session state and CSRF protection. The routes in `routes/api.php` are stateless and are assigned the `api` middleware group.

For most applications, you will begin by defining routes in your `routes/web.php` file. The routes defined in `routes/web.php` may be accessed by entering the defined route's URL in your browser.

Routes defined in the `routes/api.php` file are nested within a route group by the `RouteServiceProvider`. Within this group, the `/api` URI prefix is automatically applied so you do not need to manually apply it to every route in the file. You may modify the prefix and other route group options by modifying your `RouteServiceProvider` class.

The router allows you to register routes that respond to any HTTP verb:

```php
Route::get($uri, $callback);
Route::post($uri, $callback);
Route::put($uri, $callback);
Route::patch($uri, $callback);
Route::delete($uri, $callback);
Route::options($uri, $callback);

Route::match(['get', 'post'], '/', function () {
    //
});

Route::any('/', function () {
    //
});
```

> Any HTML forms pointing to `POST`, `PUT`, `PATCH`, or `DELETE` routes that are defined in the web routes file should include a CSRF token field. Otherwise, the request will be rejected.

```php
<form method="POST" action="/profile">
    @csrf
    ...
</form>
```

> Form Method Spoofing
> HTML forms do not support `PUT`, `PATCH` or `DELETE` actions. So, when defining `PUT`, `PATCH` or `DELETE` routes that are called from an HTML form, you will need to add a hidden `_method` field to the form. The value sent with the `_method` field will be used as the HTTP request method:

```php
<form action="/foo/bar" method="POST">
    <input type="hidden" name="_method" value="PUT">
    <input type="hidden" name="_token" value="{{ csrf_token() }}">
</form>
```

> You may use the `@method` Blade directive to generate the `_method` input:

```php
<form action="/foo/bar" method="POST">
    @method('PUT')
    @csrf
</form>
```

> Cross-Origin Resource Sharing (CORS)
> Laravel can automatically respond to CORS OPTIONS requests with values that you configure. All CORS settings may be configured in your `cors` configuration file and OPTIONS requests will automatically be handled by the `HandleCors` middleware that is included by default in your global middleware stack.

Other examples:

```php


// Basic Routing

Route::get('foo', function () {
    return 'Hello World';
});


// Redirect Routes

Route::redirect('/here', '/there'); // By default, Route::redirect returns a 302 status code.

Route::redirect('/here', '/there', 301);

Route::permanentRedirect('/here', '/there'); // Route::permanentRedirect method to return a 301 status code


// View Routes

Route::view('/welcome', 'welcome');

Route::view('/welcome', 'welcome', ['name' => 'Taylor']);


// Required Parameters

Route::get('user/{id}', function ($id) {
    return 'User '.$id;
});

Route::get('posts/{post}/comments/{comment}', function ($postId, $commentId) {
    //
});


// Optional Parameters

Route::get('user/{name?}', function ($name = null) {
    return $name;
});

Route::get('user/{name?}', function ($name = 'John') {
    return $name;
});


// Regular Expression Constraints

Route::get('user/{name}', function ($name) {
    //
})->where('name', '[A-Za-z]+');

Route::get('user/{id}', function ($id) {
    //
})->where('id', '[0-9]+');

Route::get('user/{id}/{name}', function ($id, $name) {
    //
})->where(['id' => '[0-9]+', 'name' => '[a-z]+']);


// Encoded Forward Slashes

// The Laravel routing component allows all characters except /. You must explicitly allow / to be part of your placeholder 
Route::get('search/{search}', function ($search) {
    return $search;
})->where('search', '.*');


// Named Routes

Route::get('user/profile', function () {
    //
})->name('profile');

Route::get('user/profile', 'UserProfileController@show')->name('profile');

Route::get('user/{id}/profile', function ($id) {
    //
})->name('profile');


// Generating URLs To Named Routes

$url = route('profile'); // Generating URLs...

$url = route('profile', ['id' => 1]); // Generating URLs...

$url = route('profile', ['id' => 1, 'photos' => 'yes']); // Generating URLs... (/user/1/profile?photos=yes)

return redirect()->route('profile'); // Generating Redirects...


// Inspecting The Current Route

if ($request->route()->named('profile')) {
    //
}


//Route Groups

// Middleware
Route::middleware(['first', 'second'])->group(function () {
    Route::get('/', function () {
        // Uses first & second Middleware
    });

    Route::get('user/profile', function () {
        // Uses first & second Middleware
    });
});

// Namespaces
Route::namespace('Admin')->group(function () {
    // Controllers Within The "App\Http\Controllers\Admin" Namespace
});

// Subdomain Routing
Route::domain('{account}.myapp.com')->group(function () {
    Route::get('user/{id}', function ($account, $id) {
        //
    });
});

// Route Prefixes
Route::prefix('admin')->group(function () {
    Route::get('users', function () {
        // Matches The "/admin/users" URL
    });
});

// Route Name Prefixes
Route::name('admin.')->group(function () {
    Route::get('users', function () {
        // Route assigned name "admin.users"...
    })->name('users');
});

// Shared attributes are specified in an array format as the first parameter to the Route::group method.
Route::group(['prefix' => 'tasks', 'as' => 'tasks.', 'middleware' => 'auth'], function () {
	//
});


// Route Model Binding

// Implicit Binding
// Since the $user variable is type-hinted as the App\User Eloquent model and the variable name matches the {user} URI segment, Laravel will automatically inject the model instance that has an ID matching the corresponding value from the request URI. If a matching model instance is not found in the database, a 404 HTTP response will automatically be generated.
Route::get('api/users/{user}', function (App\User $user) {
    return $user->email;
});

// Customizing The Key
// Sometimes you may wish to resolve Eloquent models using a column other than id. To do so, you may specify the column in the route parameter definition.
Route::get('api/posts/{post:slug}', function (App\Post $post) {
    return $post;
});


//Fallback Route

// It will be executed when no other route matches the incoming request.
Route::fallback(function () {
    //
});


// Rate Limiting

// In this example, an authenticated user may access the following group of routes 60 times per minute.
Route::middleware('auth:api', 'throttle:60,1')->group(function () {
    Route::get('/user', function () {
        //
    });
});


// Dynamic Rate Limiting

// You may specify a dynamic request maximum based on an attribute of the authenticated User model. 
// In this example, if your User model contains a rate_limit attribute, you may pass the name of the attribute to the throttle middleware so that it is used to calculate the maximum request count.
Route::middleware('auth:api', 'throttle:rate_limit,1')->group(function () {
    Route::get('/user', function () {
        //
    });
});


// Distinct Guest & Authenticated User Rate Limits

// You may specify different rate limits for guest and authenticated users. For example, you may specify a maximum of 10 requests per minute for guests 60 for authenticated users.
Route::middleware('throttle:10|60,1')->group(function () {
    //
});


// Accessing The Current Route

$route = Route::current();

$name = Route::currentRouteName();

$action = Route::currentRouteAction();
```


## Middleware

Middleware provide a convenient mechanism for filtering HTTP requests entering your application. For example, Laravel includes a middleware that verifies the user of your application is authenticated. If the user is not authenticated, the middleware will redirect the user to the login screen. However, if the user is authenticated, the middleware will allow the request to proceed further into the application.

All of these middleware are located in the `app/Http/Middleware` directory.

To create a new middleware, use the `make:middleware` Artisan command:

```bash
php artisan make:middleware CheckAge
```

This command will place a new `CheckAge` class within your `app/Http/Middleware` directory.

In this middleware, we will only allow access to the route if the supplied `age` is greater than 200. Otherwise, we will redirect the users back to the `home` URI:

```php
<?php

namespace App\Http\Middleware;

use Closure;

class CheckAge
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        if ($request->age <= 200) {
            return redirect('home');
        }

        return $next($request);
    }
}
```

Additional middleware parameters will be passed to the middleware after the `$next` argument:

```php
<?php

namespace App\Http\Middleware;

use Closure;

class CheckRole
{
    /**
     * Handle the incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @param  string  $role
     * @return mixed
     */
    public function handle($request, Closure $next, $role)
    {
        if (! $request->user()->hasRole($role)) {
            // Redirect...
        }

        return $next($request);
    }

}
```

Middleware parameters may be specified when defining the route by separating the middleware name and parameters with a `:`. Multiple parameters should be delimited by commas:

```php
Route::put('post/{id}', function ($id) {
    //
})->middleware('role:editor');
```

> All middleware are resolved via the service container, so you may type-hint any dependencies you need within a middleware's constructor.

> If you want a middleware to run during every HTTP request to your application, list the middleware class in the `$middleware` property of your `app/Http/Kernel.php` class.

> If you would like to assign middleware to specific routes, you should first assign the middleware a key in your `app/Http/Kernel.php` file. By default, the `$routeMiddleware` property of this class contains entries for the middleware included with Laravel. To add your own, append it to this list and assign it a key of your choosing.

```php
protected $routeMiddleware = [
    'auth' => \App\Http\Middleware\Authenticate::class,
    'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
    'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
    'cache.headers' => \Illuminate\Http\Middleware\SetCacheHeaders::class,
    'can' => \Illuminate\Auth\Middleware\Authorize::class,
    'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
    'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
    'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
    'verified' => \Illuminate\Auth\Middleware\EnsureEmailIsVerified::class,
];
```

> Once the middleware has been defined in the HTTP kernel, you may use the `middleware` method to assign middleware to a route:

Examples:

```php

Route::get('admin/profile', function () {
    //
})->middleware('auth');


Route::get('/', function () {
    //
})->middleware('first', 'second');


use App\Http\Middleware\CheckAge;

Route::get('admin/profile', function () {
    //
})->middleware(CheckAge::class);


use App\Http\Middleware\CheckAge;

Route::middleware([CheckAge::class])->group(function () {
    Route::get('/', function () {
        //
    });

    Route::get('admin/profile', function () {
        //
    })->withoutMiddleware([CheckAge::class]);
});


Route::get('/', function () {
    //
})->middleware('web');


Route::group(['middleware' => ['web']], function () {
    //
});


Route::middleware(['web', 'subscribed'])->group(function () {
    //
});

```

> Sometimes you may want to group several middleware under a single key to make them easier to assign to routes. You may do this using the `$middlewareGroups` property of your HTTP kernel. Out of the box, Laravel comes with `web` and `api` middleware groups that contain common middleware you may want to apply to your web UI and API routes.
> Out of the box, the `web` middleware group is automatically applied to your `routes/web.php` file by the `RouteServiceProvider`.

```php
/**
 * The application's route middleware groups.
 *
 * @var array
 */
protected $middlewareGroups = [
    'web' => [
        \App\Http\Middleware\EncryptCookies::class,
        \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
        \Illuminate\Session\Middleware\StartSession::class,
        \Illuminate\View\Middleware\ShareErrorsFromSession::class,
        \App\Http\Middleware\VerifyCsrfToken::class,
        \Illuminate\Routing\Middleware\SubstituteBindings::class,
    ],

    'api' => [
        'throttle:60,1',
        'auth:api',
    ],
];
```

> Rarely, you may need your middleware to execute in a specific order but not have control over their order when they are assigned to the route. In this case, you may specify your middleware priority using the `$middlewarePriority` property of your `app/Http/Kernel.php` file.

```php
/**
 * The priority-sorted list of middleware.
 *
 * This forces non-global middleware to always be in the given order.
 *
 * @var array
 */
protected $middlewarePriority = [
    \Illuminate\Session\Middleware\StartSession::class,
    \Illuminate\View\Middleware\ShareErrorsFromSession::class,
    \Illuminate\Contracts\Auth\Middleware\AuthenticatesRequests::class,
    \Illuminate\Routing\Middleware\ThrottleRequests::class,
    \Illuminate\Session\Middleware\AuthenticateSession::class,
    \Illuminate\Routing\Middleware\SubstituteBindings::class,
    \Illuminate\Auth\Middleware\Authorize::class,
];
```

>Terminable Middleware
>Sometimes a middleware may need to do some work after the HTTP response has been sent to the browser. If you define a `terminate` method on your middleware and your web server is using FastCGI, the `terminate` method will automatically be called after the response is sent to the browser.


## CSRF Protection

Laravel makes it easy to protect your application from cross-site request forgery (CSRF) attacks. Cross-site request forgeries are a type of malicious exploit whereby unauthorized commands are performed on behalf of an authenticated user.

Laravel automatically generates a CSRF "token" for each active user session managed by the application. This token is used to verify that the authenticated user is the one actually making the requests to the application.

Anytime you define an HTML form in your application, you should include a hidden CSRF token field in the form so that the CSRF protection middleware can validate the request. You may use the `@csrf` Blade directive to generate the token field:

```php
<form method="POST" action="/profile">
    @csrf
    ...
</form>
```

The `VerifyCsrfToken` middleware, which is included in the `web` middleware group, will automatically verify that the token in the request input matches the token stored in the session.

> When building JavaScript driven applications, it is convenient to have your JavaScript HTTP library automatically attach the CSRF token to every outgoing request. By default, the Axios HTTP library provided in the `resources/js/bootstrap.js` file automatically sends an `X-XSRF-TOKEN` header using the value of the encrypted `XSRF-TOKEN` cookie. If you are not using this library, you will need to manually configure this behavior for your application.

### Excluding URIs From CSRF Protection

Sometimes you may wish to exclude a set of URIs from CSRF protection. For example, if you are using Stripe to process payments and are utilizing their webhook system, you will need to exclude your Stripe webhook handler route from CSRF protection since Stripe will not know what CSRF token to send to your routes.

Typically, you should place these kinds of routes outside of the `web` middleware group that the `RouteServiceProvider` applies to all routes in the `routes/web.php` file. However, you may also exclude the routes by adding their URIs to the `$except` property of the `VerifyCsrfToken` middleware:

```php
<?php

namespace App\Http\Middleware;

use Illuminate\Foundation\Http\Middleware\VerifyCsrfToken as Middleware;

class VerifyCsrfToken extends Middleware
{
    /**
     * The URIs that should be excluded from CSRF verification.
     *
     * @var array
     */
    protected $except = [
        'stripe/*',
        'http://example.com/foo/bar',
        'http://example.com/foo/*',
    ];
}
```

> The CSRF middleware is automatically disabled when running tests.

### X-CSRF-Token

In addition to checking for the CSRF token as a POST parameter, the `VerifyCsrfToken` middleware will also check for the `X-CSRF-TOKEN` request header. You could, for example, store the token in an HTML `meta` tag:

```html
<meta name="csrf-token" content="{{ csrf_token() }}">
```

Then, once you have created the `meta` tag, you can instruct a library like jQuery to automatically add the token to all request headers. This provides simple, convenient CSRF protection for your AJAX based applications:

```javascript
$.ajaxSetup({
    headers: {
        'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
    }
});
```

### X-XSRF-Token

Laravel stores the current CSRF token in an encrypted `XSRF-TOKEN` cookie that is included with each response generated by the framework. You can use the cookie value to set the `X-XSRF-TOKEN` request header.

This cookie is primarily sent as a convenience since some JavaScript frameworks and libraries, like Angular and Axios, automatically place its value in the `X-XSRF-TOKEN` header on same-origin requests.

> By default, the `resources/js/bootstrap.js` file includes the Axios HTTP library which will automatically send this for you.


## Controllers
## Requests
## Responses
## Views
## URL Generation
## Session
## Validation
## Error Handling
## Logging

