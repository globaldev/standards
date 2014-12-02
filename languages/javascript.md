# JavaScript

## Style Guide

### Whitespace

- All source files use spaces, not tabs
- The indent level is always 4 spaces (this nicely lines up variable
  declarations)
- Feel free to use blank lines to make your code easier to read (they'll be
  removed by the minifier anyway)
- Don't leave trailing whitespace at the end of lines, as JSHint will moan
  about it

### Syntax

#### Quotes

In general we prefer double quotes. Single quotes may be used in separate
projects if you want, but whichever is chosen must be used consistently. This
choice should be enforced with use of the JSHint `quotmark` setting. Remember
that nested quotes can be escaped where necessary.

#### Semicolons

- Never, ever, ever rely on automatic semicolon insertion
- Remember that function expression assignments are followed by a semicolon

```javascript
var x = function () {
    // Function expressions assignments must be followed by a semicolon
};
```

#### Naming

- Use descriptive identifiers where possible (things like `i` for loop counters
  are still allowed)

```javascript
var camelCase; // Variable identifiers should always be camel case
function alsoCamelCase() {} // So should function identifiers (all types of function, except constructors)
function NotConstructors() {} // Constructors should always start with an upper case letter
var UPPERCASE_CONSTANTS; // Symbolic constants should be all-uppercase and delimited by underscores
Name.Spaces // Namespace elements should always start with an upper case letter
```

#### Parentheses, braces and line breaks

- Each variable declaration should appear on its own line
- Commas should appear at the end of a line, rather than at the start of the
  next line

```javascript
var myVariable = 10,
    anotherVariable = 20,
    oneMore = { // Notice that object literals span multiple lines
        p: 30,
        q: 40, // Don't quote property identifiers when it's not necessary
        default: 10 // Only quote reserved word identifiers when the environment requires you to do so (e.g. not Node)
    },
    anArray = [ // Notice that array literal span multiple lines too
        "item1"
    ];
```

- Statements that can be followed by a block (e.g. `if`, `for`, `while`) should
  always be followed by a block
- The opening curly brace of the block should always be on the same line as the
  preceding statement
- The closing curly brace should always be on its own line
- In the case of `if` statements, `else` blocks should begin on the same line
  as the closing brace of the `if` block

```javascript
if (i < 42) { // Notice the space between 'if' and the opening parenthesis
    x = true;
} else { // Notice that the 'else' is on the same line as the brace
    x = false;
}

for (j = 0; j < 10; j++) { // Notice the space between `for` and the opening parenthesis
    a.push(j); // Notice the lack of a space in the case of a method call
}
```

- All `case` clauses should be aligned with the `switch` statement
- The same applies to the `default` clause, if present

```javascript
switch (x) {
case 1:
    // The `case` keyword is aligned with the `switch` keyword
    break;
}
```

#### Assignments, declarations and functions

- A single variable statement should appear in any given execution context
  (except where no variables are declared)
- This rule may be broken when the variable is not initialised

```javascript
var a = 1,
    b = 2,
    c, d, e; // These are not initialised so may appear on one line
```

- The variable statement, if present, should always be the first statement of
  any given execution context
- Nested function declarations should appear after after the variable statement
- The formatting rules for function expressions is similar to block statements,
  but differs slightly for function declarations

```javascript
// Function declaration
function a(arg1, arg2) {
    // Notice the lack of space between the identifier and the opening parenthesis.
    // Notice the space between the closing parenthesis and the opening brace.
}

// Function expression
var expr = function (arg1, arg2) {
    // Notice the spaces between the `function` keyword and the opening paren, and the closing paren and opening brace.
    // Make sure you include the semicolon following an expression!
};

// Named function expression
var expr2 = function named(arg1, arg2) {
    // Notice that this time we follow the conventions of function declarations, but we still need the semicolon.
};
```

- Do not name function expressions unless absolutely necessary (prefer named
  function expressions to `arguments.callee` and additional global identifiers
  though - just be wary of the fact that the identifier leaks into the
  enclosing scope in IE8 and below)
- All functions should run in strict mode. This can be achieved with a wrapping
  IIFE instead of a directive in each function

```javascript
(function () {
    "use strict";
    // Any functions within this execution context inherit the strict mode directive
}());
```

- Function invocations should only contain spaces between arguments

```javascript
someFunction("arg1", "arg2");
```

- Immediately invoked function expressions should always be wrapped in
  parentheses. Don't use some other operator to force expression parsing
- The invoking parentheses should always be inside the wrapping set

```javascript
!function () {}(); // This is bad
(function () {})(); // This is better, but still not perfect
(function () {}()); // This is good
```

#### Types, coercion and type checking

- Always use strict comparisons to prevent type coercion

```javascript
10 == "10"; // true, not good
10 === "10"; // false, much better
```

- To check the type of something prefer the `typeof` operator where possible

```javascript
typeof x === "string"; // String
typeof x === "number"; // Number
typeof x === "boolean"; // Boolean
typeof x === "object"; // Object
```

- Due to their various inconsistencies, compare directly with the `null` and
  `undefined` values instead of using `typeof`

```javascript
x === null;
x === undefined;
```

- Alternatively, use of the internal Class property is acceptable

```javascript
Object.prototype.toString.call(x) === "[object String]";
Object.prototype.toString.call(x) === "[object Number]";
Object.prototype.toString.call(x) === "[object Boolean]";
Object.prototype.toString.call(x) === "[object Object]";
Object.prototype.toString.call(x) === "[object Null]";
Object.prototype.toString.call(x) === "[object Undefined]";
Object.prototype.toString.call(x) === "[object Array]";
```

- In general, use the most self-explanatory method to cast something to a
  particular type
- In the case of casting strings and dates to numbers, the use of the unary `+`
  operator is allowed

```javascript
typeof +"100"; // Number
typeof +new Date(); // Number
```

#### Constructors, classes and methods

- Don't create instance methods unless absolutely necessary. Always prefer the
  prototype. It's much more memory-efficient

```javascript
function Person(name) {
    this.name = name;
}
Person.prototype.sayHello = function () {
    console.log(this.name + " says hello");
}
```

- It's ok to create static properties on function objects if absolutely
  necessary, but prefer a closure

```javascript
// Prefer this...
var Person = (function () {
    var personCounter = 0;
    return function (name) {
        this.name = name;
        personCounter++;
    };
}());

// Over this...
function Person(name) {
    this.name = name;
    Person.counter++;
}
Person.counter = 0;
```

- If you're adding properties to an existing prototype (especially a native
  one) and the environment supports it, always use `Object.defineProperty` and
  consider making your new property non-enumerable

```javascript
// This is bad (the property will be enumerable)
Object.prototype.isEmpty = function () {
    return Object.keys(this).length === 0;  
};

// This is good (the property will not be enumerable)
Object.defineProperty(Object.prototype, "isEmpty", {
    value: function () {
        return Object.keys(this).length === 0;   
    },
    enumerable: false
});
```

- Don't use native ES5 getters and setters (unless you're working in Node and
  really want to)

#### Looping and iteration

##### Arrays

- In ES5 environments we prefer `Array.prototype.forEach`

```
myArray.forEach(function (item, index) {
    console.log(myArray[item]);
});
```

- If you need to bail out of a loop early, use a `for` loop

##### Objects

- In ES5 environments we prefer `Object.keys`

```
Object.keys(collection).forEach(function (item) {
    collection[item].doSomething();
});
```

- In non-ES5 environments (or if you want to bail out of the loop early) use a
  `for...in` loop but **always** check the property with `hasOwnProperty`
  before using it

```
for (item in collection) {
    if (collection.hasOwnProperty(item)) {
         // do something with item
    }
}
```

### Performance

- Cache long "namespaces" to prevent repeated property lookups (never use the
  `with` statement to do this)

```javascript
var namespace = My.Really.Long.Namespace;
namespace.method();
namespace.somethingElse();
```

- Cache array lengths in `for` loop initialisers (if you know the length won't
  change during iteration)

```javascript
for (i = 0, length = arr.length; i < length; i++) {
    // Saves a lookup of the length property on every iteration
}
```

- Cache regular expressions where possible

```javascript
var regex = /something/g;
function doStuff() {
    // If you use the regex in here but declare it outside it only has to be compiled once, rather than every invocation
}
```

### Working with the DOM

- When selecting elements, use simple CSS selectors where possible. If you need
  a complex selector it's usually better to select each set of elements
  individually.

- Cache selected elements!

- Use a single delegated event handler rather than binding an event to multiple
  elements
  - You can bind handlers before child elements are available
  - You can add new child elements to a container and they'll automatically
    work

```javascript
// this is better...
var gallery = document.getElementById("gallery"),
gallery.addEventListener("click", function (e) {
    if (e.target.tagName === "IMG") {
        alert("you clicked: " + e.target.src);
    }
});

// ...than this...
var gallery = document.getElementById("gallery"),
    imgs = gallery.querySelectorAll("img"),
    c;

for (c = 0; c < imgs.length; c++) {
    imgs.addEventListener("click", function (e) {
        alert("you clicked: " + e.target.src);
    });
}
```


### General good practices

- If you discover some weird inconsistency or quirk, put a comment in the code
  giving details of the fix, preferably with a link (e.g. to a Stack Overflow
  question and answer)

- Always run JSHint against your code. We prefer the following set of options,
  which you can set in a Grunt file, or on a per-file basis. Scope-level
  overrides are acceptable, but only when absolutely necessary

```javascript
{
    browser: true, // OR `node: true`, depending on environment
    curly: true,
    eqeqeq: true,
    forin: true,
    immed: true,
    latedef: true,
    newcap: true,
    noempty: true,
    nonew: true,
    onevar: true,
    undef: true,
    unused: true,
    strict: true,
    trailing: true,
    white: true,
    indent: 4,
    quotmark: "double"
}
```

- If you don't understand the reasoning behind a JSHint message, try searching
  on [JSHint Error Explanations](http://jshinterrors.com). If an article
  doesn't exist for the message in question, investigate it and write one!

- Finally, just remember that **all code should look as if it was written by
  one person**

## Principles



## Testing



## Preferred Frameworks and Libraries


