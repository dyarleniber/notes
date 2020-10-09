[Back](../README.md)

## PHP 8

PHP 8 will be released on November 26, 2020.

### Just In Time compiler (JIT)

The JIT — just in time — compiler promises significant performance improvements, albeit not always within the context of web requests.

- [Just In Time compiler (JIT)](jit/README.md)

### Union types

Given the dynamically typed nature of PHP, there are lots of cases where union types can be useful. Union types are a collection of two or more types which indicate that either one of those can be used.

```php
public function foo(Foo|Bar $input): int|float;
```

Note that `void` can never be part of a union type, since it indicates "no return value at all". Furthermore, `nullable` unions can be written using `|null`, or by using the existing `?` notation:

```php
public function foo(Foo|null $foo): void;

public function bar(?Bar $bar): void;
```

### The nullsafe operator

If you're familiar with the null coalescing operator you're already familiar with its shortcomings: it doesn't work on method calls. Instead you need intermediate checks, or rely on optional helpers provided by some frameworks:

```php
$startDate = $booking->getStartDate();

$dateAsString = $startDate ? $startDate->asDateTimeString() : null;
```

With the addition of the nullsafe operator, we can now have null coalescing-like behaviour on methods!

```php
$dateAsString = $booking->getStartDate()?->asDateTimeString();
```

### Named arguments

Named arguments allow you to pass in values to a function, by specifying the value name, so that you don't have to take their order into consideration, and you can also skip optional parameters!

```php
function foo(string $a, string $b, ?string $c = null, ?string $d = null) 
{ /* … */ }

foo(
    b: 'value b', 
    a: 'value a', 
    d: 'value d',
);
```

### Attributes

Attributes, commonly known as annotations in other languages, offers a way to add meta data to classes, without having to parse docblocks.

As for a quick look, here's an example of what attributes look like, from the RFC:

```php
use App\Attributes\ExampleAttribute;

#[ExampleAttribute]
class Foo
{
    #[ExampleAttribute]
    public const FOO = 'foo';
 
    #[ExampleAttribute]
    public $x;
 
    #[ExampleAttribute]
    public function foo(#[ExampleAttribute] $bar) { }
}
```

```php
#[Attribute]
class ExampleAttribute
{
    public $value;
 
    public function __construct($value)
    {
        $this->value = $value;
    }
}
```

### Match expression

You could call it the big brother of the `switch` expresion: `match` can return values, doesn't require `break` statements, can combine conditions, uses strict type comparisons and doesn't do any type coercion.

It looks like this:

```php
$result = match($input) {
    0 => "hello",
    '1', '2', '3' => "world",
};
```

### Constructor property promotion

This RFC adds syntactic sugar to create value objects or data transfer objects. Instead of specifying class properties and a constructor for them, PHP can now combine them into one.

Instead of doing this:

```php
class Money 
{
    public Currency $currency;
 
    public int $amount;
 
    public function __construct(
        Currency $currency,
        int $amount,
    ) {
        $this->currency = $currency;
        $this->amount = $amount;
    }
}
```

You can now do this:

```php
class Money 
{
    public function __construct(
        public Currency $currency,
        public int $amount,
    ) {}
}
```

### New `static` return type

While it was already possible to return `self`, `static` wasn't a valid return type until PHP 8. Given PHP's dynamically typed nature, it's a feature that will be useful to many developers.

```php
class Foo
{
    public function test(): static
    {
        return new static();
    }
}
```

### New `mixed` type

Some might call it a necessary evil: the `mixed` type causes many to have mixed feelings. There's a very good argument to make for it though: a missing type can mean lots of things in PHP:

- A function returns nothing or null
- We're expecting one of several types
- We're expecting a type that can't be type hinted in PHP

Because of the reasons above, it's a good thing the `mixed` type is added. `mixed` itself means one of these types:

- `array`
- `bool`
- `callable`
- `int`
- `float`
- `null`
- `object`
- `resource`
- `string`

Note that `mixed` can also be used as a parameter or property type, not just as a return type.

> Also note that since `mixed` already includes null, it's not allowed to make it nullable.

### Throw expression

This RFC changes throw from being a statement to being an expression, which makes it possible to throw exception in many new places:

```php
$triggerError = fn () => throw new MyError();

$foo = $bar['offset'] ?? throw new OffsetDoesNotExist('offset');
```

### Inheritance with private methods

Previously, PHP used to apply the same inheritance checks on public, protected and private methods. In other words: private methods should follow the same method signature rules as protected and public methods. This doesn't make sense, since private methods won't be accessible by child classes.

This RFC changed that behaviour, so that these inheritance checks are not performed on private methods anymore. Furthermore, the use of `final private function` also didn't make sense, so doing so will now trigger a warning.

### Weak maps

Built upon the weakrefs RFC that was added in PHP 7.4, a `WeakMap` implementation is added in PHP 8. `WeakMap` holds references to objects, which don't prevent those objects from being garbage collected.

Take the example of ORMs, they often implement caches which hold references to entity classes to improve the performance of relations between entities. These entity objects can not be garbage collected, as long as this cache has a reference to them, even if the cache is the only thing referencing them.

If this caching layer uses weak references and maps instead, PHP will garbage collect these objects when nothing else references them anymore. Especially in the case of ORMs, which can manage several hundreds, if not thousands of entities within a request; weak maps can offer a better, more resource friendly way of dealing with these objects.

Here's what weak maps look like, an example from the RFC:

```php
class Foo 
{
    private WeakMap $cache;
 
    public function getSomethingWithCaching(object $obj): object
    {
        return $this->cache[$obj]
           ??= $this->computeSomethingExpensive($obj);
    }
}
```

### Allowing `::class` on objects

A small, yet useful, new feature: it's now possible to use `::class` on objects, instead of having to use `get_class()` on them. It works the same way as `get_class()`.

```php
$foo = new Foo();

var_dump($foo::class);
```

### Non-capturing catches

Whenever you wanted to catch an exception before PHP 8, you had to store it in a variable, regardless whether you used that variable or not. With non-capturing catches, you can omit the variable, so instead of this:

```php
try {
    // Something goes wrong
} catch (MySpecialException $exception) {
    Log::error("Something went wrong");
}
```

You can now do this:

```php
try {
    // Something goes wrong
} catch (MySpecialException) {
    Log::error("Something went wrong");
}
```

Note that it's required to always specify the type, you're not allowed to have an empty `catch`.

### Trailing comma in parameter lists

Already possible when calling a function, trailing comma support was still lacking in parameter lists. It's now allowed in PHP 8, meaning you can do the following:

```php
public function(
    string $parameterA,
    int $parameterB,
    Foo $objectfoo,
) {
    // …
}
```

### Create `DateTime` objects from interface

You can already create a `DateTime` object from a `DateTimeImmutable` object using `DateTime::createFromImmutable($immutableDateTime)`, but the other way around was tricky. By adding `DateTime::createFromInterface()` and `DatetimeImmutable::createFromInterface()` there's now a generalised way to convert `DateTime` and `DateTimeImmutable` objects to each other.

```php
DateTime::createFromInterface(DateTimeInterface $other);

DateTimeImmutable::createFromInterface(DateTimeInterface $other);
```

### New `Stringable` interface

The `Stringable` interface can be used to type hint anything that implements `__toString()`. Whenever a class implements `__toString()`, it automatically implements the interface behind the scenes and there's no need to manually implement it.

```php
class Foo
{
    public function __toString(): string
    {
        return 'foo';
    }
}

function bar(string|Stringable $stringable) { /* … */ }

bar(new Foo());
bar('abc');
```

### New `str_contains()` function

Some might say it's long overdue, but we finally don't have to rely on `strpos()` anymore to know whether a string contains another string.

Instead of doing this:

```php
if (strpos('string with lots of words', 'words') !== false) { /* … */ }
```

You can now do this:

```php
if (str_contains('string with lots of words', 'words')) { /* … */ }
```

### New `str_starts_with()` and `str_ends_with()` functions

Two other ones long overdue, these two functions are now added in the core.

```php
str_starts_with('haystack', 'hay'); // true
str_ends_with('haystack', 'stack'); // true
```

### New `fdiv()` function

The new `fdiv()` function does something similar as the `fmod()` and `intdiv()` functions, which allows for division by 0. Instead of errors you'll get `INF`, `-INF` or `NAN`, depending on the case.

### New `get_debug_type()` function

`get_debug_type()` returns the type of a variable. Sounds like something `gettype()` would do? `get_debug_type()` returns more useful output for arrays, strings, anonymous classes and objects.

For example, calling `gettype()` on a class `\Foo\Bar` would return `object`. Using `get_debug_type()` will return the class name.

A full list of differences between `get_debug_type()` and `gettype()` can be found in the RFC.

### New `get_resource_id()` function

Resources are special variables in PHP, referring to external resources. One example is a MySQL connection, another one a file handle.

Each one of those resources gets assigned an ID, though previously the only way to know that id was to cast the resource to `int`:

```php
$resourceId = (int) $resource;
```

PHP 8 adds the `get_resource_id()` functions, making this operation more obvious and type-safe:

```php
$resourceId = get_resource_id($resource);
```

### Abstract methods in traits improvements

Traits can specify abstract methods which must be implemented by the classes using them. There's a caveat though: before PHP 8 the signature of these method implementations weren't validated. The following was valid:

```php
trait Test {
    abstract public function test(int $input): int;
}

class UsesTrait
{
    use Test;

    public function test($input)
    {
        return $input;
    }
}
```

PHP 8 will perform proper method signature validation when using a trait and implementing its abstract methods. This means you'll need to write this instead:

```php
class UsesTrait
{
    use Test;

    public function test(int $input): int
    {
        return $input;
    }
}
```

### Object implementation of `token_get_all()`

The `token_get_all()` function returns an array of values. This RFC adds a `PhpToken` class with a `PhpToken::getAll()` method. This implementation works with objects instead of plain values. It consumes less memory and is easier to read.

### Variable syntax tweaks

From the RFC: "the Uniform Variable Syntax RFC resolved a number of inconsistencies in PHP's variable syntax. This RFC intends to address a small handful of cases that were overlooked."

### Type annotations for internal functions

Lots of people pitched in to add proper type annotations to all internal functions. This was a long standing issue, and finally solvable with all the changes made to PHP in previous versions. This means that internal functions and methods will have complete type information in reflection.

### `ext-json` always available

Previously it was possible to compile PHP without the JSON extension enabled, this is not possible anymore. Since JSON is so widely used, it's best developers can always rely on it being there, instead of having to ensure the extension exist first.

### Breaking changes

This is a major update and thus there will be breaking changes. The best thing to do is take a look at the full list of breaking changes over at the [UPGRADING document](https://github.com/php/php-src/blob/master/UPGRADING#L20).

Many of these breaking changes have been deprecated in previous 7.* versions though, so if you've been staying up-to-date over the years, it shouldn't be all that hard to upgrade to PHP 8.

#### Consistent type errors

User-defined functions in PHP will already throw `TypeError`, but internal functions did not, they rather emitted warnings and returned `null`. As of PHP 8 the behaviour of internal functions have been made consistent.

#### Reclassified engine warnings

Lots of errors that previously only triggered warnings or notices, have been converted to proper errors.

#### The `@` operator no longer silences fatal errors

It's possible that this change might reveal errors that again were hidden before PHP 8. Make sure to set `display_errors=Off` on your production servers!

#### Default error reporting level

It's now `E_ALL` instead of everything but `E_NOTICE` and `E_DEPRECATED`. This means that many errors might pop up which were previously silently ignored, though probably already existent before PHP 8.

#### Default PDO error mode

From the RFC: The current default error mode for PDO is silent. This means that when an SQL error occurs, no errors or warnings may be emitted and no exceptions thrown unless the developer implements their own explicit error handling.

This RFC changes the default error will change to `PDO::ERRMODE_EXCEPTION` in PHP 8.

#### Concatenation precedence

While already deprecated in PHP 7.4, this change is now taken into effect. If you'd write something like this:

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

#### Stricter type checks for arithmetic and bitwise operators

Before PHP 8, it was possible to apply arithmetic or bitwise operators on arrays, resources or objects. This isn't possible anymore, and will throw a `TypeError`.

#### Namespaced names being a single token

PHP used to interpret each part of a namespace (separated by a backslash `\`) as a sequence of tokens. This RFC changed that behaviour, meaning reserved names can now be used in namespaces.

#### Saner numeric strings

PHP's type system tries to do a lot of smart things when it encounters numbers in strings. This RFC makes that behaviour more consistent and clear.

#### Saner string to number comparisons

This RFC fixes the very strange case in PHP where `0 == "foo"` results in `true`. There are some other edge cases like that one, and this RFC fixes them.

#### Reflection method signature changes

Three method signatures of reflection classes have been changed:

```php
ReflectionClass::newInstance($args);
ReflectionFunction::invoke($args);
ReflectionMethod::invoke($object, $args);
```

Have now become:

```php
ReflectionClass::newInstance(...$args);
ReflectionFunction::invoke(...$args);
ReflectionMethod::invoke($object, ...$args);
```

The upgrading guide specifies that if you extend these classes, and still want to support both PHP 7 and PHP 8, the following signatures are allowed:

```php
ReflectionClass::newInstance($arg = null, ...$args);
ReflectionFunction::invoke($arg = null, ...$args);
ReflectionMethod::invoke($object, $arg = null, ...$args);
```

#### Stable sorting

Before PHP 8, sorting algorithms were unstable. This means that the order of equal elements wasn't guaranteed. PHP 8 changes the behaviour of all sorting functions to stable sorting.

#### Fatal error for incompatible method signatures

From the RFC: Inheritance errors due to incompatible method signatures currently either throw a fatal error or a warning depending on the cause of the error and the inheritance hierarchy.

#### Other deprecations and changes

During the PHP 7.* development, several deprecations were added that are now finalised in PHP 8.

### References

- https://stitcher.io/blog/new-in-php-8
