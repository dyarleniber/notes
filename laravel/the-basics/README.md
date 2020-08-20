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
