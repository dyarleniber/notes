[Back](../README.md)

# Frontend


## Blade Templates

Blade is the simple, yet powerful templating engine provided with Laravel. Unlike other popular PHP templating engines, Blade does not restrict you from using plain PHP code in your views. In fact, all Blade views are compiled into plain PHP code and cached until they are modified, meaning Blade adds essentially zero overhead to your application. Blade view files use the `.blade.php` file extension and are typically stored in the `resources/views` directory.

### Defining A Layout

```html
<!-- Stored in resources/views/layouts/app.blade.php -->

<html>
    <head>
        <title>App Name - @yield('title')</title>
    </head>
    <body>
        @section('sidebar')
            This is the master sidebar.
        @show

        <div class="container">
            @yield('content')
        </div>
    </body>
</html>
```

### Extending A Layout

```html
<!-- Stored in resources/views/child.blade.php -->

@extends('layouts.app')

@section('title', 'Page Title')

@section('sidebar')
    @parent

    <p>This is appended to the master sidebar.</p>
@endsection

@section('content')
    <p>This is my body content.</p>
@endsection
```

> By default, Blade `{{ }}` statements are automatically sent through PHP's `htmlspecialchars` function to prevent XSS attacks.

```html
Hello, {!! $name !!}.
```

### Other examples

```html

<!-- Displaying Data -->
Hello, {{ $name }}.

<!-- Rendering JSON -->
<script>
    var app = @json($array);

    var app = @json($array, JSON_PRETTY_PRINT);
</script>

<!-- Rendering JSON manually -->
<script>
    var app = <?php echo json_encode($array); ?>;
</script>

<!-- If Statements -->
@if (count($records) === 1)
    I have one record!
@elseif (count($records) > 1)
    I have multiple records!
@else
    I don't have any records!
@endif

@unless (Auth::check())
    You are not signed in.
@endunless

@isset($records)
    // $records is defined and is not null...
@endisset

@empty($records)
    // $records is "empty"...
@endempty

@auth
    // The user is authenticated...
@endauth

@guest
    // The user is not authenticated...
@endguest

@auth('admin')
    // The user is authenticated...
@endauth

@guest('admin')
    // The user is not authenticated...
@endguest

@hasSection('navigation')
    <div class="pull-right">
        @yield('navigation')
    </div>

    <div class="clearfix"></div>
@endif

@sectionMissing('navigation')
    <div class="pull-right">
        @include('default-navigation')
    </div>
@endif

@production
    // Production specific content...
@endproduction

@env('staging')
    // Staging specific content...
@endenv

@switch($i)
    @case(1)
        First case...
        @break

    @case(2)
        Second case...
        @break

    @default
        Default case...
@endswitch

@for ($i = 0; $i < 10; $i++)
    The current value is {{ $i }}
@endfor

@foreach ($users as $user)
    <p>This is user {{ $user->id }}</p>
@endforeach

@forelse ($users as $user)
    <li>{{ $user->name }}</li>
@empty
    <p>No users</p>
@endforelse

@while (true)
    <p>I'm looping forever.</p>
@endwhile

@foreach ($users as $user)
    @if ($user->type == 1)
        @continue
    @endif

    <li>{{ $user->name }}</li>

    @if ($user->number == 5)
        @break
    @endif
@endforeach

@foreach ($users as $user)
    @continue($user->type == 1)

    <li>{{ $user->name }}</li>

    @break($user->number == 5)
@endforeach

@foreach ($users as $user)
    @if ($loop->first)
        This is the first iteration.
    @endif

    @if ($loop->last)
        This is the last iteration.
    @endif

    <p>This is user {{ $user->id }}</p>
@endforeach

@foreach ($users as $user)
    @foreach ($user->posts as $post)
        @if ($loop->parent->first)
            This is first iteration of the parent loop.
        @endif
    @endforeach
@endforeach

{{-- This comment will not be present in the rendered HTML --}}

@php
    //
@endphp

<form action="/foo/bar" method="POST">
	@csrf
    @method('PUT')

    ...
</form>

<!-- Validation Errors -->
<!-- /resources/views/post/create.blade.php -->

<!-- You may pass the name of a specific error bag as the second parameter to the @error directive to retrieve validation error messages on pages containing multiple forms. -->

<label for="title">Post Title</label>

<input id="title" type="text" class="@error('title') is-invalid @enderror">

@error('title')
    <div class="alert alert-danger">{{ $message }}</div>
@enderror

<!-- Including Subviews -->
<div>
    @include('shared.errors')

    @include('view.name', ['some' => 'data'])

    <form>
        <!-- Form Contents -->
    </form>
</div>

@include('view.name', ['some' => 'data'])

@includeIf('view.name', ['some' => 'data'])

@includeWhen($boolean, 'view.name', ['some' => 'data'])

@includeUnless($boolean, 'view.name', ['some' => 'data'])

@includeFirst(['custom.admin', 'admin'], ['some' => 'data'])

<!-- Rendering Views For Collections -->
@each('view.name', $jobs, 'job')

```

### Components

Components and slots provide similar benefits to sections and layouts; however, some may find the mental model of components and slots easier to understand. There are two approaches to writing components: class based components and anonymous components.

To create a class based component, you may use the `make:component` Artisan command.

```bash
php artisan make:component Alert
```

The `make:component` command will place the component in the `App\View\Components` directory.

The `make:component` command will also create a view template for the component. The view will be placed in the `resources/views/components` directory.

To display a component, you may use a Blade component tag within one of your Blade templates. Blade component tags start with the string `x-` followed by the kebab case name of the component class.

```html
<x-alert/>

<x-user-profile/>

<!-- App\View\Components\Inputs\Button.php -->
<x-inputs.button/>
```

### Stacks

Blade allows you to push to named stacks which can be rendered somewhere else in another view or layout. This can be particularly useful for specifying any JavaScript libraries required by your child views.

```html
<head>
    <!-- Head Contents -->

    @stack('scripts')
</head>
```

```html
@push('scripts')
    <script src="/example.js"></script>
@endpush
```

> If you would like to prepend content onto the beginning of a stack, you should use the `@prepend` directive.

### Service Injection

The `@inject` directive may be used to retrieve a service from the Laravel service container.

```html
@inject('metrics', 'App\Services\MetricsService')

<div>
    Monthly Revenue: {{ $metrics->monthlyRevenue() }}.
</div>
```

### Extending Blade

Blade allows you to define your own custom directives using the `directive` method.

```php
<?php

namespace App\Providers;

use Illuminate\Support\Facades\Blade;
use Illuminate\Support\ServiceProvider;

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
        Blade::directive('datetime', function ($expression) {
            return "<?php echo ($expression)->format('m/d/Y H:i'); ?>";
        });
    }
}
```

> Programming a custom directive is sometimes more complex than necessary when defining simple, custom conditional statements. For that reason, Blade provides a `Blade::if` method which allows you to quickly define custom conditional directives using Closures.

```php
use Illuminate\Support\Facades\Blade;

/**
 * Bootstrap any application services.
 *
 * @return void
 */
public function boot()
{
    Blade::if('cloud', function ($provider) {
        return config('filesystems.default') === $provider;
    });
}
```

```php
@cloud('digitalocean')
    // The application is using the digitalocean cloud provider...
@elsecloud('aws')
    // The application is using the aws provider...
@else
    // The application is not using the digitalocean or aws environment...
@endcloud

@unlesscloud('aws')
    // The application is not using the aws environment...
@endcloud
```


## Localization

Laravel's localization features provide a convenient way to retrieve strings in various languages, allowing you to easily support multiple languages within your application. Language strings are stored in files within the `resources/lang` directory. Within this directory there should be a subdirectory for each language supported by the application.

All language files return an array of keyed strings.

```php
<?php

return [
    'welcome' => 'Welcome to our application',

    // If you wish, you may define placeholders in your translation strings. All placeholders are prefixed with a :
    'customwelcome' => 'Welcome, :name',

    // If your placeholder contains all capital letters, or only has its first letter capitalized, the translated value will be capitalized accordingly.
    'capitalizedwelcome' => 'Welcome, :NAME', // Welcome, DAYLE
];
```

> Pluralization is a complex problem, as different languages have a variety of complex rules for pluralization. By using a "pipe" character, you may distinguish singular and plural forms of a string.

> For languages that differ by territory, you should name the language directories according to the ISO 15897.

The default language for your application is stored in the `config/app.php` configuration file. You may modify this value to suit the needs of your application. You may also change the active language at runtime using the `setLocale` method on the `App` facade.

```php
Route::get('welcome/{locale}', function ($locale) {
    if (! in_array($locale, ['en', 'es', 'fr'])) {
        abort(400);
    }

    App::setLocale($locale);

    //
});
```

> A fallback language is also configured in the `config/app.php` configuration file.

Determining The Current Locale:

```php
$locale = App::getLocale();

if (App::isLocale('en')) {
    //
}
```

Retrieving Translation Strings:

```php
echo __('messages.welcome');

echo __('I love programming.');

{{ __('messages.welcome') }}

@lang('messages.welcome')
```


## Frontend Scaffolding

While Laravel does not dictate which JavaScript or CSS pre-processors you use, it does provide a basic starting point using Bootstrap, React, and / or Vue that will be helpful for many applications. By default, Laravel uses NPM to install both of these frontend packages.


## Compiling Assets (Mix)

Laravel Mix provides a fluent API for defining Webpack build steps for your Laravel application using several common CSS and JavaScript pre-processors. Through simple method chaining, you can fluently define your asset pipeline.

The `webpack.mix.js` file is your entry point for all asset compilation. Think of it as a light configuration wrapper around Webpack. Mix tasks can be chained together to define exactly how your assets should be compiled.

Example:

```javascript

mix.js('resources/js/app.js', 'public/js')
    .sass('resources/sass/app.scss', 'public/css');

mix.styles([
    'public/css/vendor/normalize.css',
    'public/css/vendor/videojs.css'
], 'public/css/all.css');

mix.scripts([
    'public/js/admin.js',
    'public/js/dashboard.js'
], 'public/js/all.js');

mix.copy('node_modules/foo/bar.css', 'public/css/bar.css');

mix.copyDirectory('resources/img', 'public/img');

```

Many developers suffix their compiled assets with a timestamp or unique token to force browsers to load the fresh assets instead of serving stale copies of the code. Mix can handle this for you using the `version` method.

```javascript
mix.js('resources/js/app.js', 'public/js')
    .version();
```

After generating the versioned file, you won't know the exact file name. So, you should use Laravel's global `mix` function within your views to load the appropriately hashed asset.

```html
<script src="{{ mix('/js/app.js') }}"></script>
```

> For CSS compilation, Webpack will rewrite and optimize any `url()` calls within your stylesheets.
> By default, Laravel Mix and Webpack will find `example.png`, copy it to your `public/images` folder, and then rewrite the `url()`.
> As useful as this feature may be, it's possible that your existing folder structure is already configured in a way you like. If this is the case, you may disable `url()` rewriting like so:

```javascript
mix.sass('resources/sass/app.scss', 'public/css')
    .options({
        processCssUrls: false
    });
```

Within a fresh installation of Laravel, you'll find a `package.json` file in the root of your directory structure. The default `package.json` file includes everything you need to get started.

```bash

# Install packages
npm install

# Run all Mix tasks...
npm run dev

# Run all Mix tasks and minify output...
npm run production

# Watching Assets For Changes
npm run watch
npm run watch-poll

```
