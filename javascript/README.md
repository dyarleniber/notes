[Back](../README.md)

# JavaScript

## Values & Types

JavaScript provides eight different data types which are:

- `string`
- `number`
- `bigint`
- `boolean`
- `null`
- `undefined`
- `object`
- `symbol`

When JavaScript variables are declared, they have an initial value of undefined.

> If you do a mathematical operation on an undefined variable your result will be NaN which means "Not a Number".

> If you concatenate a string with an undefined variable, you will get a literal string of "undefined".

String values in JavaScript may be written with single or double quotes, as long as you start and end with the same type of quote. Unlike some other programming languages, single and double quotes work the same in JavaScript.

Quotes are not the only characters that can be escaped inside a string. There are two reasons to use escaping characters:
- To allow you to use characters you may not otherwise be able to type out, such as a carriage return.
- To allow you to represent multiple quotes in a string without JavaScript misinterpreting what you mean.

Escaping characters:

Code | Output
\' | single quote
\" | double quote
\\ | backslash
\n | newline
\r | carriage return
\t | tab
\b | word boundary
\f | form feed

JavaScript provides a typeof operator that can examine a value and tell you what type it is:

typeof null is an interesting case, because it errantly returns "object", when you'd expect it to return "null".

This is a long-standing bug in JS, but one that is likely never going to be fixed. Too much code on the Web relies on the bug and thus fixing it would cause a lot more bugs!

The value types array and function are not proper built-in types, these should be thought of more like subtypes -- specialized versions of the object type.

```javascript
var a;
typeof a; // "undefined"

a = null;
typeof a; // "object" -- weird, bug

var arr = [
	"hello world",
	42,
	true
];
typeof arr; // "object"

function foo() {
	return 42;
}

foo.bar = "hello world";

typeof foo; // "function"
typeof foo(); // "number"
typeof foo.bar; // "string"
```

## Built-In Type Methods

The built-in types and subtypes have behaviors exposed as properties and methods that are quite powerful and useful.

For example:

```javascript
var a = "hello world";
var b = 3.14159;

a.length; // 11
a.toUpperCase(); // "HELLO WORLD"
b.toFixed(4); // "3.1416"
```

The "how" behind being able to call a.toUpperCase() is more complicated than just that method existing on the value.

Briefly, there is a String (capital S) object wrapper form, typically called a "native," that pairs with the primitive string type; it's this object wrapper that defines the toUpperCase() method on its prototype.

When you use a primitive value like "hello world" as an object by referencing a property or method (e.g., a.toUpperCase() in the previous snippet), JS automatically "boxes" the value to its object wrapper counterpart (hidden under the covers).

A string value can be wrapped by a String object, a number can be wrapped by a Number object, and a boolean can be wrapped by a Boolean object. For the most part, you don't need to worry about or directly use these object wrapper forms of the values -- prefer the primitive value forms in practically all cases and JavaScript will take care of the rest for you.

## Comparing Values

There are four equality operators: ==, ===, !=, and !==. The ! forms are of course the symmetric "not equal" versions of their counterparts; non-equality should not be confused with inequality.

The difference between == and === is usually characterized that == checks for value equality and === checks for both value and type equality. However, this is inaccurate. The proper way to characterize them is that == checks for value equality with coercion allowed, and === checks for value equality without allowing coercion; === is often called "strict equality" for this reason.

For more information about the inequality comparison rules, see section 11.8.5 of the ES5 specification.

## Function Scopes

You use the var keyword to declare a variable that will belong to the current function scope, or the global scope if at the top level outside of any function.

### Hoisting

Wherever a var appears inside a scope, that declaration is taken to belong to the entire scope and accessible everywhere throughout.

Metaphorically, this behavior is called hoisting, when a var declaration is conceptually "moved" to the top of its enclosing scope. 

```javascript
var a = 2;

foo(); // works because `foo()` declaration is "hoisted"

function foo() {
	a = 3;

	console.log( a ); // 3

	var a; // declaration is "hoisted" to the top of `foo()`
}

console.log( a ); // 2
```

When you declare a variable, it is available anywhere in that scope, as well as any lower/inner scopes.

```javascript
function foo() {
	var a = 1;

	function bar() {
		var b = 2;

		function baz() {
			var c = 3;

			console.log( a, b, c );	// 1 2 3
		}

		baz();
		console.log( a, b ); // 1 2
	}

	bar();
	console.log( a ); // 1
}

foo();
```

If you try to set a variable that hasn't been declared, you'll either end up creating a variable in the top-level global scope (bad!) or getting an error, depending on "strict mode" (see "Strict Mode").

ES6 lets you declare variables to belong to individual blocks (pairs of { .. }), using the let keyword. Besides some nuanced details, the scoping rules will behave roughly the same as we just saw with functions:

```javascript
function foo() {
	var a = 1;

	if (a >= 1) {
		let b = 2;

		while (b < 5) {
			let c = b * 2;
			b++;

			console.log( a + c );
		}
	}
}

foo();
// 5 7 9
```

Because of using let instead of var, b will belong only to the if statement and thus not to the whole foo() function's scope. Similarly, c belongs only to the while loop. Block scoping is very useful for managing your variable scopes in a more fine-grained fashion, which can make your code much easier to maintain over time.

## Strict Mode

ES5 added a "strict mode" to the language, which tightens the rules for certain behaviors. Generally, these restrictions are seen as keeping the code to a safer and more appropriate set of guidelines. Also, adhering to strict mode makes your code generally more optimizable by the engine. Strict mode is a big win for code, and you should use it for all your programs.

You can opt in to strict mode for an individual function, or an entire file, depending on where you put the strict mode pragma:

```javascript
function foo() {
	"use strict";

	// this code is strict mode

	function bar() {
		// this code is strict mode
	}
}

// this code is not strict mode
```

Compare that to:

```javascript
"use strict";

function foo() {
	// this code is strict mode

	function bar() {
		// this code is strict mode
	}
}

// this code is strict mode
```

One key difference (improvement!) with strict mode is disallowing the implicit auto-global variable declaration from omitting the var:

```javascript
function foo() {
	"use strict";	// turn on strict mode
	a = 1;			// `var` missing, ReferenceError
}

foo();
```

## Functions As Values

So far, we've discussed functions as the primary mechanism of scope in JavaScript.

Though it may not seem obvious from that syntax, foo is basically just a variable in the outer enclosing scope that's given a reference to the function being declared. That is, the function itself is a value, just like 42 or [1,2,3] would be.

This may sound like a strange concept at first, so take a moment to ponder it. Not only can you pass a value (argument) to a function, but a function itself can be a value that's assigned to variables, or passed to or returned from other functions.

```javascript
var foo = function() { // anonymous function
	// ..
};

var x = function bar() {
	// ..
};
```

### Immediately Invoked Function Expressions (IIFEs)

```javascript
(function IIFE(){
	console.log( "Hello!" );
})();
// "Hello!"
```

The outer ( .. ) that surrounds the (function IIFE(){ .. }) function expression is just a nuance of JS grammar needed to prevent it from being treated as a normal function declaration.

Because an IIFE is just a function, and functions create variable scope, using an IIFE in this fashion is often used to declare variables that won't affect the surrounding code outside the IIFE:

```javascript
var a = 42;

(function IIFE(){
	var a = 10;
	console.log( a ); // 10
})();

console.log( a ); // 42
```

IIFEs can also have return values:

```javascript
var x = (function IIFE(){
	return 42;
})();

x; // 42
```

### Closure

Closure is a way to "remember" and continue to access a function's scope (its variables) even once the function has finished running.

```javascript
function makeAdder(x) {
	// parameter `x` is an inner variable

	// inner function `add()` uses `x`, so
	// it has a "closure" over it
	function add(y) {
		return y + x;
	};

	return add;
}

// `plusOne` gets a reference to the inner `add(..)`
// function with closure over the `x` parameter of
// the outer `makeAdder(..)`
var plusOne = makeAdder( 1 );

plusOne( 3 ); // 4  <-- 1 + 3
```

#### Modules

The most common usage of closure in JavaScript is the module pattern. Modules let you define private implementation details (variables, functions) that are hidden from the outside world, as well as a public API that is accessible from the outside.

```javascript
function User(){
	var username, password;

	function doLogin(user,pw) {
		username = user;
		password = pw;

		// do the rest of the login work
	}

	var publicAPI = {
		login: doLogin
	};

	return publicAPI;
}

// create a `User` module instance
var fred = User();

fred.login( "fred", "12Battery34!" );
```

The inner doLogin() function has a closure over username and password, meaning it will retain its access to them even after the User() function finishes running.

At this point, the outer User() function has finished executing. Normally, you'd think the inner variables like username and password have gone away. But here they have not, because there's a closure in the login() function keeping them alive.

## this Identifier

While it may often seem that this is related to "object-oriented patterns," in JS this is a different mechanism.

If a function has a this reference inside it, that this reference usually points to an object. But which object it points to depends on how the function was called.

It's important to realize that this does not refer to the function itself, as is the most common misconception.

```javascript
function foo() {
	console.log( this.bar );
}

var bar = "global";

var obj1 = {
	bar: "obj1",
	foo: foo
};

var obj2 = {
	bar: "obj2"
};

// --------

foo(); // "global" - foo() ends up setting this to the global object in non-strict mode -- in strict mode, this would be undefined and you'd get an error in accessing the bar property -- so "global" is the value found for this.bar.
obj1.foo(); // "obj1" - obj1.foo() sets this to the obj1 object.
foo.call( obj2 ); // "obj2" - foo.call(obj2) sets this to the obj2 object.
new foo(); // undefined - new foo() sets this to a brand new empty object.
```

## Prototypes

The prototype mechanism in JavaScript is quite complicated. We will only glance at it here.

When you reference a property on an object, if that property doesn't exist, JavaScript will automatically use that object's internal prototype reference to find another object to look for the property on. You could think of this almost as a fallback if the property is missing.

The internal prototype reference linkage from one object to its fallback happens at the time the object is created. The simplest way to illustrate it is with a built-in utility called Object.create(..).

```javascript
var foo = {
	a: 42
};

// create `bar` and link it to `foo`
var bar = Object.create( foo );

bar.b = "hello world";

bar.b;		// "hello world"
bar.a;		// 42 <-- delegated to `foo`
```

The a property doesn't actually exist on the bar object, but because bar is prototype-linked to foo, JavaScript automatically falls back to looking for a on the foo object, where it's found.

This linkage may seem like a strange feature of the language. The most common way this feature is used is to try to emulate/fake a "class" mechanism with "inheritance.".

## Old & New

Some of the new JS features will not necessarily be available in older browsers. In fact, some of the newest features in the specification aren't even implemented in any stable browsers yet.

There are two main techniques you can use to "bring" the newer JavaScript stuff to the older browsers: polyfilling and transpiling.

### Polyfilling

The word "polyfill" is an invented term (by Remy Sharp) (https://remysharp.com/2010/10/08/what-is-a-polyfill) used to refer to taking the definition of a newer feature and producing a piece of code that's equivalent to the behavior, but is able to run in older JS environments.

```javascript
// NaN value is the only value in the whole language that is not equal to itself. So the NaN value is the only one that would make x !== x be true.

if (!Number.isNaN) {
	Number.isNaN = function isNaN(x) {
		return x !== x;
	};
}
```

The if statement guards against applying the polyfill definition in ES6 browsers where it will already exist. If it's not already present, we define Number.isNaN(..).

Not all new features are fully polyfillable. Sometimes most of the behavior can be polyfilled, but there are still small deviations. You should be really, really careful in implementing a polyfill yourself, to make sure you are adhering to the specification as strictly as possible.

Or better yet, use an already vetted set of polyfills that you can trust, such as those provided by ES5-Shim (https://github.com/es-shims/es5-shim) and ES6-Shim (https://github.com/es-shims/es6-shim).

### Transpiling

There's no way to polyfill new syntax that has been added to the language. The new syntax would throw an error in the old JS engine as unrecognized/invalid.

So the better option is to use a tool that converts your newer code into older code equivalents. This process is commonly called "transpiling," a term for transforming + compiling.

Essentially, your source code is authored in the new syntax form, but what you deploy to the browser is the transpiled code in old syntax form. You typically insert the transpiler into your build process, similar to your code linter or your minifier.

You might wonder why you'd go to the trouble to write new syntax only to have it transpiled away to older code -- why not just write the older code directly?

There are several important reasons you should care about transpiling:

- The new syntax added to the language is designed to make your code more readable and maintainable. The older equivalents are often much more convoluted. You should prefer writing newer and cleaner syntax, not only for yourself but for all other members of the development team.
- If you transpile only for older browsers, but serve the new syntax to the newest browsers, you get to take advantage of browser performance optimizations with the new syntax. This also lets browser makers have more real-world code to test their implementations and optimizations on.
- Using the new syntax earlier allows it to be tested more robustly in the real world, which provides earlier feedback to the JavaScript committee (TC39). If issues are found early enough, they can be changed/fixed before those language design mistakes become permanent.

Here's a quick example of transpiling. ES6 adds a feature called "default parameter values." It looks like this:

```javascript
function foo(a = 2) {
	console.log( a );
}

foo();		// 2
foo( 42 );	// 42
```

So, what will a transpiler do with that code to make it run in older environments?

```javascript
function foo() {
	var a = arguments[0] !== (void 0) ? arguments[0] : 2;
	console.log( a );
}
```

As you can see, it checks to see if the arguments[0] value is void 0 (aka undefined), and if so provides the 2 default value; otherwise, it assigns whatever was passed.

The last important detail to emphasize about transpilers is that they should now be thought of as a standard part of the JS development ecosystem and process. JS is going to continue to evolve, much more quickly than before, so every few months new syntax and new features will be added.

If you use a transpiler by default, you'll always be able to make that switch to newer syntax whenever you find it useful, rather than always waiting for years for today's browsers to phase out.

There are quite a few great transpilers for you to choose from. Here are some good options at the time of this writing:

- Babel (https://babeljs.io) (formerly 6to5): Transpiles ES6+ into ES5
- Traceur (https://github.com/google/traceur-compiler): Transpiles ES6, ES7, and beyond into ES5

## Non-JavaScript

Most JS is written to run in and interact with environments like browsers. A good chunk of the stuff that you write in your code is, strictly speaking, not directly controlled by JavaScript.

The most common non-JavaScript JavaScript you'll encounter is the DOM API. For example:

```javascript
var el = document.getElementById( "foo" );
```

The document variable exists as a global variable when your code is running in a browser. It's not provided by the JS engine, nor is it particularly controlled by the JavaScript specification. It takes the form of something that looks an awful lot like a normal JS object, but it's not really exactly that. It's a special object, often called a "host object.".

Moreover, the getElementById(..) method on document looks like a normal JS function, but it's just a thinly exposed interface to a built-in method provided by the DOM from your browser. In some (newer-generation) browsers, this layer may also be in JS, but traditionally the DOM and its behavior is implemented in something more like C/C++.
