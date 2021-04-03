[Back](../README.md)

# JavaScript

JavaScript provides eight different data types which are undefined, null, boolean, string, symbol, bigint, number, and object.

When JavaScript variables are declared, they have an initial value of undefined.

If you do a mathematical operation on an undefined variable your result will be NaN which means "Not a Number".

If you concatenate a string with an undefined variable, you will get a literal string of "undefined".


String values in JavaScript may be written with single or double quotes, as long as you start and end with the same type of quote. Unlike some other programming languages, single and double quotes work the same in JavaScript.


Quotes are not the only characters that can be escaped inside a string. There are two reasons to use escaping characters:

To allow you to use characters you may not otherwise be able to type out, such as a carriage return.
To allow you to represent multiple quotes in a string without JavaScript misinterpreting what you mean.
We learned this in the previous challenge.

Code	Output
\'	single quote
\"	double quote
\\	backslash
\n	newline
\r	carriage return
\t	tab
\b	word boundary
\f	form feed




Global Scope and Functions
In JavaScript, scope refers to the visibility of variables. Variables which are defined outside of a function block have Global scope. This means, they can be seen everywhere in your JavaScript code.

Variables which are used without the var keyword are automatically created in the global scope. This can create unintended consequences elsewhere in your code or when running a function again. You should always declare your variables with var.


Variables which are declared within a function, as well as the function parameters have local scope. That means, they are only visible within that function.

It is possible to have both local and global variables with the same name. When you do this, the local variable takes precedence over the global variable.



Objects are similar to arrays, except that instead of using indexes to access and modify their data, you access the data in objects through what are called properties.

Objects are useful for storing data in a structured way, and can represent real world objects

if your object has any non-string properties, JavaScript will automatically typecast them as strings.


There are two ways to access the properties of an object: dot notation (.) and bracket notation ([]), similar to an array.

Dot notation is what you use when you know the name of the property you're trying to access ahead of time.


The second way to access the properties of an object is bracket notation ([]). If the property of the object you are trying to access has a space in its name, you will need to use bracket notation.

However, you can still use bracket notation on object properties without spaces.










