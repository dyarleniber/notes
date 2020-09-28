[Back](../README.md)

## PHP 7.4

PHP 7.4 is the latest stable version of PHP. It was released on November 28, 2019

### Arrow functions for cleaner one-liner functions

Arrow functions, also called "short closures", allow for less verbose one-liner functions.

Before:
```php
array_map(function (User $user) { 
    return $user->id; 
}, $users)
```

After:
```php
array_map(fn (User $user) => $user->id, $users)
```

> They can always access the parent scope, there's no need for the `use` keyword.
> `$this` is available just like normal closures.
> Arrow functions may only contain one expression, which is also the return statement.

### Typed properties in classes

Typed class properties have been added in PHP 7.4 and provide a major improvement to PHP's type system.

Class variables can be type hinted:

```php
class Foo
{
    public int $a;

    public ?string $b = 'foo';

    private Foo $prop;

    protected static string $static = 'default';
}
```

> They are only available in classes and require an access modifier: `public`, `protected` or `private`; or `var`

> All types are allowed, except `void` and `callable`

> Note that it's impossible to have default values with `object` or class types. You should use the constructor to set their defaults

The following code is valid:

```php
class Foo
{
    public int $bar;
}

$foo = new Foo;
```

Even though the value of `$bar` isn't an integer after making an object of `Foo`, PHP will only throw an error when `$bar` is accessed.

If `$bar` didn't have a type, its value would simply be `null`. Types can be nullable though, so it's not possible to determine whether a typed nullable property was set, or simply forgotten.

> Using `unset` on a typed property will make it uninitialized, while unsetting an untyped property will make it `null`.

PHP will try to coerce or convert types whenever possible. Say you pass a string where you expect an integer, PHP will try and convert that string automatically.

If you don't like this behaviour you can disabled it by declaring strict types:

```php
declare(strict_types=1);
```

Even though PHP 7.4 introduced improved type variance, typed properties are still invariant. This means that the following is not valid:

```php
class A {}
class B extends A {}

class Foo
{
    public A $prop;
}

class Bar extends Foo
{
    public B $prop;
}
```

### Improved type variance

You'll be able use covariant return types:

```php
class ParentType {}
class ChildType extends ParentType {}

class A
{
    public function covariantReturnTypes(): ParentType
    { /* … */ }
}

class B extends A
{
    public function covariantReturnTypes(): ChildType
    { /* … */ }
}
```

And contravariant arguments:

```php
class A
{
    public function contraVariantArguments(ChildType $type)
    { /* … */ }
}

class B extends A
{
    public function contraVariantArguments(ParentType $type)
    { /* … */ }
}
```

### The null coalescing assignment operator as a shorthand

A shorthand for null coalescing operations.

Before:
```php
$data['date'] = $data['date'] ?? new DateTime();
```

After:
```php
$data['date'] ??= new DateTime();
```

### Spread operator in arrays

It's now possible to use the spread operator in arrays:

```php
$arrayA = [1, 2, 3];

$arrayB = [4, 5];

$result = [0, ...$arrayA, ...$arrayB, 6 ,7];

// [0, 1, 2, 3, 4, 5, 6, 7]
```

> Note that this only works with arrays with numerical keys.

```php
// Type Hint + Spread Operator (...)

function sum(int ...$numbers) {
   echo array_sum($numbers);
}

sum(1, 2, 3, 4);
sum(1, 2, 3, 4, 5, 6, 7);
```

### Numeric Literal Separator

PHP 7.4 allows for underscores to be used to visually separate numeric values. It looks like this:

```php
$unformattedNumber = 107925284.88;

$formattedNumber = 107_925_284.88;
```

> The underscores are simply ignored by the engine.

### FFI (Foreign function interface) for better extension development in PHP

Moving on to some more core-level features: foreign function interface or "FFI" in short, allows us to call C code from userland. This means that PHP extensions could be written in pure PHP and loaded via composer.

It should be noted though that this is a complex topic. You still need C knowledge to be able to properly use this feature.

### Preloading to improve performance

Another lower-level feature is preloading. It's is an amazing addition to PHP's core, which can result in some significant performance improvements.

In short: if you're using a framework, its files have to be loaded and linked on every request. Preloading allows the server to load PHP files in memory on startup, and have them permanently available to all subsequent requests.

The performance gain comes of course with a cost: if the source of preloaded files are changed, the server has to be restarted.

### Custom object serialization

Two new magic methods have been added: `__serialize` and `__unserialize`.

### Reflection for references

Libraries like Symfony's var dumper rely heavily on the reflection API to reliably dump a variable. Previously it wasn't possible to properly reflect references, resulting in these libraries relying on hacks to detect them.

PHP 7.4 adds the `ReflectionReference` class which solves this issue.

### Weak references

Weak references are references to objects, which don't prevent them from being destroyed.

### `mb_str_split` added

This function provides the same functionality as `str_split`, but on multi-byte strings.

### Password Hashing Registry

Internal changes have been made to how hashing libraries are used, so that it's easier for userland to use them.

More specifically, a new function `password_algos` has been added which returns a list of all registered password algorithms.

> It means that ext/sodium (or anyone really) can register password hashing algorithms dynamically. The upshot of which is that argon2i and argon2id will be more commonly available moving forward.

### Changes and deprecations

#### Left-associative ternary operator deprecation

This RFC adds a deprecation warning for nested ternary statements. In PHP 8, this deprecation will be converted to a compile time error.

```php
1 ? 2 : 3 ? 4 : 5;   // deprecated
(1 ? 2 : 3) ? 4 : 5; // ok
```

#### Exceptions allowed in `__toString`

Previously, exceptions could not be thrown in `__toString`. They were prohibited because of a workaround for some old core error handling mechanisms, but Nikita pointed out that this "solution" didn't actually solve the problem it tried to address.

This behaviour is now changed, and exceptions can be thrown from `__toString`.

#### Concatenation precedence

If you'd write something like this:

```php
echo "sum: " . $a + $b;
```

PHP would previously interpret it like this:

```php
echo ("sum: " . $a) + $b;
```

PHP 8 will make it so that it's interpreted like this:

```php
echo "sum: " . ($a + $b);
```

PHP 7.4 adds a deprecation warning when encountering an unparenthesized expression containing a . before a + or - sign.

#### `array_merge` without arguments

Since the addition of the spread operator, there might be cases where you'd want to use `array_merge` like so:

```php
$merged = array_merge(...$arrayOfArrays);
```

To support the edge case where `$arrayOfArrays` would be empty, both `array_merge` and `array_merge_recursive` now allow an empty parameter list. An empty array will be returned if no input was passed.

#### Curly brackets for array and string access

It was possible to access arrays and string offsets using curly brackets:

```php
$array{1};
$string{3};
```

This has been deprecated.

#### Invalid array access notices

If you were to use the array access syntax on, say, an integer; PHP would previously return `null`. As of PHP 7.4, a notice will be emitted.

```php
$i = 1;

$i[0]; // Notice
```

#### `proc_open` improvements

Changes were made to `proc_open` so that it can execute programs without going through a shell. This is done by passing an array instead of a string for the command.

#### `strip_tags` also accepts arrays

You used to only be able to strip multiple tags like so:

```php
strip_tags($string, '<a><p>')
```

PHP 7.4 also allows the use of an array:

```php
strip_tags($string, ['a', 'p'])
```

#### `ext-hash` always enabled

This extension is now permanently available in all PHP installations.

#### PEAR not enabled by default

Because PEAR isn't actively maintained anymore, the core team decided to remove its default installation with PHP 7.4.

#### Several small deprecations

This RFC bundles lots of small deprecations, each with their own vote. Be sure to read a more detailed explanation on the RFC page, though here's a list of deprecated things:

- The `real` type
- Magic quotes legacy
- `array_key_exists()` with objects
- `FILTER_SANITIZE_MAGIC_QUOTES` filter
- Reflection `export()` methods
- `mb_strrpos()` with encoding as 3rd argument
- `implode()` parameter order mix
- Unbinding `$this` from non-static closures
- `hebrevc()` function
- `convert_cyr_string()` function
- `money_format()` function
- `ezmlm_hash()` function
- `restore_include_path()` function
- `allow_url_include` ini directive

#### Other changes

You should always take a look at the full [UPGRADING document](https://github.com/php/php-src/blob/PHP-7.4/UPGRADING) when upgrading PHP versions.

Here are some changes highlighted:

- Calling `parent::` in a class without a parent is deprecated.
- Calling `var_dump` on a `DateTime` or `DateTimeImmutable` instance will no longer leave behind accessible properties on the object.
- `openssl_random_pseudo_bytes` will throw an exception in error situations.
- Attempting to serialise a `PDO` or `PDOStatement` instance will generate an `Exception` instead of a `PDOException`.
- Calling `get_object_vars()` on an `ArrayObject` instance will return the properties of the `ArrayObject` itself, and not the values of the wrapped array or object. Note that `(array)` casts are not affected.
- `ext/wwdx` has been deprecated.

### References

- https://stitcher.io/blog/new-in-php-74
