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

Instead of defining all of your request handling logic as Closures in route files, you may wish to organize this behavior using Controller classes. Controllers can group related request handling logic into a single class. Controllers are stored in the `app/Http/Controllers` directory.

```php
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use App\User;

class UserController extends Controller
{
    /**
     * Instantiate a new controller instance.
     *
     * @return void
     */
    public function __construct()
    {
        $this->middleware('auth');

        $this->middleware('log')->only('index');

        $this->middleware('subscribed')->except('store');
    }

    /**
     * Show the profile for the given user.
     *
     * @param  int  $id
     * @return View
     */
    public function show($id)
    {
        return view('user.profile', ['user' => User::findOrFail($id)]);
    }
}
```

> Controllers are not required to extend a base class. However, you will not have access to convenience features such as the `middleware`, `validate`, and `dispatch` methods.

You can define a route to this controller action like so:

```php
Route::get('user/{id}', 'UserController@show');
```

> The `RouteServiceProvider` loads your route files within a route group that contains the namespace.
> If you choose to nest your controllers deeper into the `App\Http\Controllers` directory, use the specific class name relative to the `App\Http\Controllers` root namespace.

```php
Route::get('foo', 'Photos\AdminController@method');
```

### Single Action Controllers

If you would like to define a controller that only handles a single action, you may place a single `__invoke` method on the controller:

```php
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use App\User;

class ShowProfile extends Controller
{
    /**
     * Show the profile for the given user.
     *
     * @param  int  $id
     * @return View
     */
    public function __invoke($id)
    {
        return view('user.profile', ['user' => User::findOrFail($id)]);
    }
}
```

When registering routes for single action controllers, you do not need to specify a method:

```php
Route::get('user/{id}', 'ShowProfile');
```

You may generate an invokable controller by using the `--invokable` option of the `make:controller` Artisan command:

```bash
php artisan make:controller ShowProfile --invokable
```

### Resource Controllers

Laravel resource routing assigns the typical "CRUD" routes to a controller with a single line of code. For example, you may wish to create a controller that handles all HTTP requests for "photos" stored by your application. Using the `make:controller` Artisan command, we can quickly create such a controller:

```bash
php artisan make:controller PhotoController --resource
```

This command will generate a controller at app/Http/Controllers/PhotoController.php. The controller will contain a method for each of the available resource operations.

Next, you may register a resourceful route to the controller:

```php
Route::resource('photos', 'PhotoController');
```

This single route declaration creates multiple routes to handle a variety of actions on the resource. The generated controller will already have methods stubbed for each of these actions, including notes informing you of the HTTP verbs and URIs they handle.

If you are using route model binding and would like the resource controller's methods to type-hint a model instance, you may use the `--model` option when generating the controller:

```bash
php artisan make:controller PhotoController --resource --model=Photo
```

When declaring resource routes that will be consumed by APIs, you will commonly want to exclude routes that present HTML templates.

For convenience, you may use the apiResource method to automatically exclude these two routes:

```php
Route::apiResource('photos', 'PhotoController');
```

### Dependency Injection & Controllers

The Laravel service container is used to resolve all Laravel controllers. As a result, you are able to type-hint any dependencies your controller may need in its constructor. The declared dependencies will automatically be resolved and injected into the controller instance.

In addition to constructor injection, you may also type-hint dependencies on your controller's methods. A common use-case for method injection is injecting the `Illuminate\Http\Request` instance into your controller methods.

If your controller method is also expecting input from a route parameter, list your route arguments after your other dependencies. For example, if your route is defined like so:

```php
Route::put('user/{id}', 'UserController@update');
```

You may still type-hint the `Illuminate\Http\Request` and access your `id` parameter by defining your controller method.

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller
{
    /**
     * Update the given user.
     *
     * @param  Request  $request
     * @param  string  $id
     * @return Response
     */
    public function update(Request $request, $id)
    {
        //
    }
}
```

### Route Caching

> Closure based routes cannot be cached. To use route caching, you must convert any Closure routes to controller classes.

If your application is exclusively using controller based routes, you should take advantage of Laravel's route cache. Using the route cache will drastically decrease the amount of time it takes to register all of your application's routes. In some cases, your route registration may even be up to 100x faster. To generate a route cache, just execute the `route:cache` Artisan command:

```bash
php artisan route:cache
```

After running this command, your cached routes file will be loaded on every request. Remember, if you add any new routes you will need to generate a fresh route cache. Because of this, you should only run the `route:cache` command during your project's deployment.

You may use the `route:clear` command to clear the route cache:

```php
php artisan route:clear
```


## HTTP Requests

To obtain an instance of the current HTTP request via dependency injection, you should type-hint the `Illuminate\Http\Request` class on your controller method. The incoming request instance will automatically be injected by the service container.

You may also type-hint the `Illuminate\Http\Request` class on a route Closure. The service container will automatically inject the incoming request into the Closure when it is executed.

```php
use Illuminate\Http\Request;

Route::get('/', function (Request $request) {
    //
});
```

The `Illuminate\Http\Request` instance provides a variety of methods for examining the HTTP request for your application and extends the `Symfony\Component\HttpFoundation\Request` class.

> By default, Laravel includes the `TrimStrings` and `ConvertEmptyStringsToNull` middleware in your application's global middleware stack. These middleware are listed in the stack by the `App\Http\Kernel` class. These middleware will automatically trim all incoming string fields on the request, as well as convert any empty string fields to `null`. This allows you to not have to worry about these normalization concerns in your routes and controllers.

Examples:

```php

// Retrieving The Request Path
// If the incoming request is targeted at http://domain.com/foo/bar, the path method will return foo/bar
$uri = $request->path();

// The is method allows you to verify that the incoming request path matches a given pattern. You may use the * character as a wildcard when utilizing this method.
if ($request->is('admin/*')) {
    //
}

// Retrieving The Request URL Without Query String...
$url = $request->url();

// Retrieving The Request URL With Query String...
$url = $request->fullUrl();

// Retrieving The Request Method
$method = $request->method();

// Verify that the HTTP verb matches a given string.
if ($request->isMethod('post')) {
    //
}

// Retrieve all of the input data as an array.
$input = $request->all();

// Retrieving An Input Value regardless of the HTTP verb.
$name = $request->input('name');

// You may pass a default value as the second argument to the input method.
$name = $request->input('name', 'Sally');

// When working with forms that contain array inputs, use "dot" notation to access the arrays.
$name = $request->input('products.0.name');
$names = $request->input('products.*.name');

// You may call the input method without any arguments in order to retrieve all of the input values as an associative array.
$input = $request->input();

// Retrieving Input only From The Query String
$name = $request->query('name');
$name = $request->query('name', 'Helen');
$query = $request->query();

// Retrieving Input Via Dynamic Properties
// When using dynamic properties, Laravel will first look for the parameter's value in the request payload. If it is not present, Laravel will search for the field in the route parameters.
$name = $request->name;

// Retrieving JSON Input Values
// When sending JSON requests to your application, you may access the JSON data via the input method as long as the Content-Type header of the request is properly set to application/json.
// You may even use "dot" syntax to dig into JSON arrays.
$name = $request->input('user.name');

// Retrieving Boolean Input Values
// When dealing with HTML elements like checkboxes, your application may receive "truthy" values that are actually strings. For example, "true" or "on".
// For convenience, you may use the boolean method to retrieve these values as booleans.
// The boolean method returns true for 1, "1", true, "true", "on", and "yes". All other values will return false.
$archived = $request->boolean('archived');

// Retrieving A Portion Of The Input Data
$input = $request->only(['username', 'password']);
$input = $request->only('username', 'password');
$input = $request->except(['credit_card']);
$input = $request->except('credit_card');

// Determining If An Input Value Is Present
if ($request->has('name')) {
    //
}
if ($request->has(['name', 'email'])) {
    //
}
if ($request->hasAny(['name', 'email'])) {
    //
}
if ($request->filled('name')) {
    //
}
if ($request->missing('name')) {
    //
}

```

### Old Input

Laravel allows you to keep input from one request during the next request. This feature is particularly useful for re-populating forms after detecting validation errors. However, if you are using Laravel's included validation features, it is unlikely you will need to manually use these methods, as some of Laravel's built-in validation facilities will call them automatically.

```php

// Flashing Input To The Session
// The flash method on the Illuminate\Http\Request class will flash the current input to the session so that it is available during the user's next request to the application.
$request->flash();

// You may also use the flashOnly and flashExcept methods to flash a subset of the request data to the session.
// These methods are useful for keeping sensitive information such as passwords out of the session.
$request->flashOnly(['username', 'email']);
$request->flashExcept('password');

// Flashing Input Then Redirecting
return redirect('form')->withInput();
return redirect('form')->withInput(
    $request->except('password')
);

// Retrieving Old Input
$username = $request->old('username');

```

Laravel also provides a global `old` helper. If you are displaying old input within a Blade template.

```html
<input type="text" name="username" value="{{ old('username') }}">
```

### Cookies

All cookies created by the Laravel framework are encrypted and signed with an authentication code, meaning they will be considered invalid if they have been changed by the client. To retrieve a cookie value from the request, use the `cookie` method on a `Illuminate\Http\Request` instance.

```php
$value = $request->cookie('name');

// Alternatively, you may use the Cookie facade to access cookie values.
use Illuminate\Support\Facades\Cookie;

$value = Cookie::get('name');
```

You may attach a cookie to an outgoing `Illuminate\Http\Response` instance using the `cookie` method.

```php
return response('Hello World')->cookie(
    'name', 'value', $minutes
);
```

### Retrieving Uploaded Files

You may access uploaded files from a `Illuminate\Http\Request` instance using the `file` method or using dynamic properties. The `file` method returns an instance of the `Illuminate\Http\UploadedFile` class, which extends the PHP `SplFileInfo` class and provides a variety of methods for interacting with the file.

```php
$file = $request->file('photo');

$file = $request->photo;

if ($request->hasFile('photo')) {
    //
}

// Validating Successful Uploads
if ($request->file('photo')->isValid()) {
    //
}

// File Paths & Extensions
$path = $request->photo->path();
$extension = $request->photo->extension();
```

To store an uploaded file, you will typically use one of your configured filesystems. The `UploadedFile` class has a `store` method which will move an uploaded file to one of your disks, which may be a location on your local filesystem or even a cloud storage location like Amazon S3.

The `store` method accepts the path where the file should be stored relative to the filesystem's configured root directory. This path should not contain a file name, since a unique ID will automatically be generated to serve as the file name.

The `store` method also accepts an optional second argument for the name of the disk that should be used to store the file. The method will return the path of the file relative to the disk's root.

```php
$path = $request->photo->store('images');

$path = $request->photo->store('images', 's3');
```

If you do not want a file name to be automatically generated, you may use the `storeAs` method, which accepts the path, file name, and disk name as its arguments.

```php
$path = $request->photo->storeAs('images', 'filename.jpg');

$path = $request->photo->storeAs('images', 'filename.jpg', 's3');
```

### PSR-7 Requests

The PSR-7 standard specifies interfaces for HTTP messages, including requests and responses. If you would like to obtain an instance of a PSR-7 request instead of a Laravel request, you will first need to install a few libraries. Laravel uses the Symfony HTTP Message Bridge component to convert typical Laravel requests and responses into PSR-7 compatible implementations.

```bash
composer require symfony/psr-http-message-bridge
composer require nyholm/psr7
```

Once you have installed these libraries, you may obtain a PSR-7 request by type-hinting the request interface on your route Closure or controller method.

```php
use Psr\Http\Message\ServerRequestInterface;

Route::get('/', function (ServerRequestInterface $request) {
    //
});
```

> If you return a PSR-7 response instance from a route or controller, it will automatically be converted back to a Laravel response instance and be displayed by the framework.

### Configuring Trusted Proxies

When running your applications behind a load balancer that terminates TLS / SSL certificates, you may notice your application sometimes does not generate HTTPS links. Typically this is because your application is being forwarded traffic from your load balancer on port 80 and does not know it should generate secure links.

To solve this, you may use the `App\Http\Middleware\TrustProxies` middleware that is included in your Laravel application, which allows you to quickly customize the load balancers or proxies that should be trusted by your application. Your trusted proxies should be listed as an array on the `$proxies` property of this middleware. In addition to configuring the trusted proxies, you may configure the proxy `$headers` that should be trusted.

```php
<?php

namespace App\Http\Middleware;

use Fideloper\Proxy\TrustProxies as Middleware;
use Illuminate\Http\Request;

class TrustProxies extends Middleware
{
    /**
     * The trusted proxies for this application.
     *
     * @var string|array
     */
    protected $proxies = [
        '192.168.1.1',
        '192.168.1.2',
    ];

    /**
     * The headers that should be used to detect proxies.
     *
     * @var int
     */
    protected $headers = Request::HEADER_X_FORWARDED_ALL;
}
```

> If you are using AWS Elastic Load Balancing, your `$headers` value should be `Request::HEADER_X_FORWARDED_AWS_ELB`. For more information on the constants that may be used in the `$headers` property, check out Symfony's documentation on trusting proxies.

If you are using Amazon AWS or another "cloud" load balancer provider, you may not know the IP addresses of your actual balancers. In this case, you may use `*` to trust all proxies.

```php
/**
 * The trusted proxies for this application.
 *
 * @var string|array
 */
protected $proxies = '*';
```


## HTTP Responses

All routes and controllers should return a response to be sent back to the user's browser. Laravel provides several different ways to return responses.

```php

// The framework will automatically convert the string into a full HTTP response.
Route::get('/', function () {
    return 'Hello World';
});

// The framework will automatically convert the array into a JSON response.
Route::get('/', function () {
    return [1, 2, 3];
});

// Returning a full Illuminate\Http\Response instance allows you to customize the response's HTTP status code and headers.
Route::get('home', function () {
    return response('Hello World', 200)
                  ->header('Content-Type', 'text/plain');
});

// Attaching Headers To Responses
return response($content)
            ->header('Content-Type', $type)
            ->header('X-Header-One', 'Header Value')
            ->header('X-Header-Two', 'Header Value');

// Attaching Headers To Responses
return response($content)
            ->withHeaders([
                'Content-Type' => $type,
                'X-Header-One' => 'Header Value',
                'X-Header-Two' => 'Header Value',
            ]);

// Cache Control Middleware
// Laravel includes a cache.headers middleware, which may be used to quickly set the Cache-Control header for a group of routes. If etag is specified in the list of directives, an MD5 hash of the response content will automatically be set as the ETag identifier.
Route::middleware('cache.headers:public;max_age=2628000;etag')->group(function () {
    Route::get('privacy', function () {
        // ...
    });

    Route::get('terms', function () {
        // ...
    });
});

// Attaching Cookies To Responses
// By default, all cookies generated by Laravel are encrypted and signed so that they can't be modified or read by the client. If you would like to disable encryption for a subset of cookies generated by your application, you may use the $except property of the App\Http\Middleware\EncryptCookies middleware, which is located in the app/Http/Middleware directory.
return response($content)
                ->header('Content-Type', $type)
                ->cookie('name', 'value', $minutes);

// Redirects
// Redirect responses are instances of the Illuminate\Http\RedirectResponse class.
Route::get('dashboard', function () {
    return redirect('home/dashboard');
});

// Redirects
//Sometimes you may wish to redirect the user to their previous location, such as when a submitted form is invalid. You may do so by using the global back helper function.
Route::post('user/profile', function () {
    // Validate the request...

    return back()->withInput();
});

// Redirecting To Named Routes
return redirect()->route('login');
return redirect()->route('profile', ['id' => 1]);

// Populating Parameters Via Eloquent Models
// If you are redirecting to a route with an "ID" parameter that is being populated from an Eloquent model, you may pass the model itself. The ID will be extracted automatically.
// For a route with the following URI: profile/{id}
return redirect()->route('profile', [$user]);

// Redirecting To Controller Actions
return redirect()->action('HomeController@index');
return redirect()->action(
    'UserController@profile', ['id' => 1]
);

// Redirecting To External Domains
return redirect()->away('https://www.google.com');

// Redirecting With Flashed Session Data
// Redirecting to a new URL and flashing data to the session are usually done at the same time. Typically, this is done after successfully performing an action when you flash a success message to the session. For convenience, you may create a RedirectResponse instance and flash data to the session in a single, fluent method chain.
Route::post('user/profile', function () {
    // Update the user's profile...

    return redirect('dashboard')->with('status', 'Profile updated!');
});

// View Responses
return response()
            ->view('hello', $data, 200)
            ->header('Content-Type', $type);

// JSON Responses
return response()->json([
    'name' => 'Abigail',
    'state' => 'CA',
]);

// If you would like to create a JSONP response, you may use the json method in combination with the withCallback method.
return response()
            ->json(['name' => 'Abigail', 'state' => 'CA'])
            ->withCallback($request->input('callback'));

// File Downloads
// The download method may be used to generate a response that forces the user's browser to download the file at the given path.
return response()->download($pathToFile);
return response()->download($pathToFile, $name, $headers);
return response()->download($pathToFile)->deleteFileAfterSend();            

// Streamed Downloads
// Sometimes you may wish to turn the string response of a given operation into a downloadable response without having to write the contents of the operation to disk. You may use the streamDownload method in this scenario.
return response()->streamDownload(function () {
    echo GitHub::api('repo')
                ->contents()
                ->readme('laravel', 'laravel')['contents'];
}, 'laravel-readme.md');

// File Responses
// The file method may be used to display a file, such as an image or PDF, directly in the user's browser instead of initiating a download.
return response()->file($pathToFile);
return response()->file($pathToFile, $headers);

```


## Views

Views contain the HTML served by your application and separate your controller / application logic from your presentation logic. Views are stored in the `resources/views` directory.

```php

Route::get('/', function () {
    return view('greeting');
});

// Passing Data To Views
Route::get('/', function () {
    return view('greeting', ['name' => 'James']);
});

return view('greeting')->with('name', 'Victoria');

// "Dot" notation may be used to reference nested views.
Route::get('/', function () {
    return view('admin.profile', $data);
});

// Determining If A View Exists
use Illuminate\Support\Facades\View;

if (View::exists('emails.customer')) {
    //
}

```

### Sharing Data With All Views

Occasionally, you may need to share a piece of data with all views that are rendered by your application. You may do so using the view facade's `share` method. Typically, you should place calls to `share` within a service provider's `boot` method. You are free to add them to the `AppServiceProvider` or generate a separate service provider to house them.

```php
<?php

namespace App\Providers;

use Illuminate\Support\Facades\View;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Register any application services.
     *
     * @return void
     */
    public function register()
    {
        //
    }

    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        View::share('key', 'value');
    }
}
```

### Optimizing Views

By default, views are compiled on demand. When a request is executed that renders a view, Laravel will determine if a compiled version of the view exists. If the file exists, Laravel will then determine if the uncompiled view has been modified more recently than the compiled view. If the compiled view either does not exist, or the uncompiled view has been modified, Laravel will recompile the view.

Compiling views during the request negatively impacts performance, so Laravel provides the `view:cache` Artisan command to precompile all of the views utilized by your application. For increased performance, you may wish to run this command as part of your deployment process.

```bash
php artisan view:cache
```

You may use the `view:clear` command to clear the view cache.

```php
php artisan view:clear
```


## URL Generation
## Session
## Validation
## Error Handling
## Logging
