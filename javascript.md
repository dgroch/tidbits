# Javascript

## General
* Cross platform OO scripting language that executes within a host environment
* The core provides a standard library fo objects such as Array, Date, Math and a set of operators, control structures and statements
* The core can be extended for different purposes
  - Client side JS: adds objects to control a browser and its DOM, such as responding to user events, page navigation or adding elements to the DOM
  - Server side JS: provides objects for allowing JS to communicate with a DB, perform file maniupulations etc
* Dynamic, loosely typed
* Uses a prototype-based model to provide dynamic inheritance.
* Properties and methods can be added to any object dynamically
* JS is standardized by ECMA international,  TC39 committee. ECMAScript behaves the same way in all applications that support the standard
  - ECMA provides standards for, language syntax, error handling mechanisms, Types, the global object, a prototype inheritance mechanism, built in objects and functions (JSON, Math, Array...), strict mode
  - the DOM is standardized by W3C not ecma
  - Other apis include: XMLHttpRequest, CSS object model, WebWorkers, WebSockets, Canvas 2D

## Scopes, chains, closures
* lexical scoping - this is the scope that is created within a function
    - **var** is used to define a variable lexically scoped
* block scoping - the scope created within curly braced blocks
    - **let** and **const** are used to define block scope variables
* JS also has scopes global, with, catch and eval scopes
* Scopes can be nested, both lexical and block scopes can contain a nested scope
    - ie if nested in a function, function nested ina function etc
* Each nested scope has access to the outer scopes variables, but not vice versa
    - sibling nested scopes cannot access each others variables
* **Scope chain** is a tree that maps the nesting of scopes within a function
   ` (global)
        ↑
        |
       foo()
      var bar
        ↑
        |
       zip()
     var quux`
* All JS runtimes must have a global scope object
    - **window** for the browser
    - **global** for node
* When assigning a variable without using var, let or const, the variable is assumed to exist in an outer scope
    - the runtime will search upwards through the scope tree to try and find the variable, if it can't then it will create it on the global scope object
* **Shadowing** - redefining a variable in a inner scope, the inner scope can only access its own version of the variable
* **closures** - an inner function accessing a variable in its outer scope is said to **close over** the variable
* Most JS runtimes use the **mark and sweep** algorithim for garbage collection
    - references to memory (variables, functions etc) are marked if they are still reachable from active code
    - any unmarked reference is swept into the garbage and the memory is released
    - in the memory timeline of chrome dev tools, GC appears as a sudden drop
    - GC events block the main thread

### Syntax
#### Grammar and types
* case sensitive and uses the unicode character set
* instructions are called *statements* and are seperated by a semicolon(;)
* Spaces tabs and newline characters are all whitespace
* Scripts are scanned from left to right and converted into a sequence of input elements which are tokens, control characters, line terminators, comments or whitespace
* var, let and const are used to declare variables
  - let declares a block scope variable
  - const declares a read-only named constant
  - variables declared by var or let that have no initial value are set to *undefined*
  - undefined value behaves as false in a boolean context and NaN in a numeric context
  - null behaves as 0 in a numeric context and false in boolean

#### Control flow and error handling
* Just about any object be thrown in JS, its best to throw one of the specifically created types for this
  - ECMAScript exceptions
  - DOMException and DOMError
* You can specify an object when you throw an exception
`// Create an object type UserException
function UserException(message) {
  this.message = message;
  this.name = "UserException";
}

// Make the exception convert to a pretty string when used as a string
// (e.g. by the error console)
UserException.prototype.toString = function() {
  return this.name + ': "' + this.message + '"';
}

// Create an instance of the object type and throw it
throw new UserException("Value too high");`
* Use the try / catch statement to execute a block and catch exceptions
  - the finally block can be used to execute code that runs regardless of an exception being caught or not
  - ErrorType(message) object can be used to throw an error and supply a message to be displayed

#### Data types
* there 7 data types defined in ECMAScript
* the 6 primitive data types are
  - Boolean: true|false
  - null: a keyword to denote a null value. since JS is case-sensitive Null and NULL etc are not the same
  - undefined
  - Number: 42|3.14159
  - Stirng: "Hello world"
  - Symbol: provides unique and immutable instances
* the 7th data type is Object
* Expressions containing the *+* operator with numeric and string value automatically convert the numerics to strings
* literals are used to represent values, they are fixed values you provide in your script
  - Array Literal: a list of 0 or more expressions which represent an element in the array enclosed by []. An arary literal used in a function is created each time the function is called
  - Boolean Literal: true/false
  - Floating point Literal: decimal integer that can be signed, fraction or an exponent (.1e-23)
  - Integers: number expressed in decimal, hex(ox), octal(0o) or binary (0b)
  - Object Literal: list of 0 or more pairs of key/values enclosed in {}. Property names can be any string including the empty string
  - RegExp Literal
  - String Literal
* `String.charCodeAt(index)` - returns the utf16 representation of the character at the index selected

#### [Typed arrays](http://developer.mozilla.org/en-US/docs/Web/Javascript/Typed_arrays)
Provide access to binary data, have been standardized across most browsers.
There are various types such as **Uint8Array** (similar to nodejs buffers) and Int8Array (can store +ve and -ve integers)

Typed arrays use [**ArrayBuffers**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) underneath to the raw bytes.
To access their data, you need to create a typed array (a view) thorugh which to access the data.
- the `.buffer` property returns the underlying arraybuffer used for the view

#### Notes
* *debugger* can be used to invoke any available debugger if one exists ie like setting a breakpoint
* Octal numbers use a leading 0 followed by a lower/uppercase "O" ie 0o644 => 420
* Hexadecimal numbers use a leading 0 followed by a lower/upper case "X" 0xA => 10
* Hexadecimal escape sequences '\xA9' => "©"
* Unicode escape sequences '\u00A9' => "©"
* Regex literals /ab+c/g

## Prototypes
* The simplest way to create an object is by using the object literal syntax:
  `var obj = { prop: value };`
* the `__proto__` property can be used to create a new object from the prototype of an existing object, its available in ES6 onwards
  `var robot = { kind: 'robot'}; var x = {}; x.__proto__ = robot; x.kind --> 'robot' `
* To check if an object is the prototype of another use `obj.isPrototypeOf(obj)`
* Properties can be added to the prototype of an object at any time. The chain lookup will find the new property.
  - Objects delegate property lookups to their prototype
  - a prototype can be shared between objects
  - updates to a property only apply to the object not to the prototype

## ES6
* `Object.assign()`- copy the values of all enumerable own properties form one or more source objects to a target object. The function returns the target object.
  - this assigns properties as opposed to just copying them or defining new properties

### Promises
From ECMAScript 2015, promises have been standardised as part of the language. A promise is an object that defines a method
called **then**. The promise object represents a value that may be available some time in the future.

Promises have an internal state. A promise can be **fulfilled**, **rejected** or **pending**. A promise that is either fulfilled or
rejected is said to be **resolved**.

Promises are created using `new Promise(callback(fulfill, reject))`, either fulfill or reject will be called to indicate the outcome.
The fulfill function takes any values that he promise has yielded.

The **then** property is a function that takes 2 callback parameters, **onFulfilled** and **onRejected**, the relevant callback for the
then function is called once the main callback has finished. The then function can be called multiple times.

* use the .then().catch() form to catch any errors within promise chains
* use Promise.all(promise1, promise2).then() - this will pass the results of both promises as an array to the then block
* if you have promises dependent on previous results, nest with Promise.all
`firstThingAsync()  
  .then(function(result1) {
    return Promise.all([result1, secondThingAsync(result1)]);
  })
  .then(function(results) {
    // do something with results array: results[0], results[1]
  })
  .catch(function(err){ /* ... */ });
`

## Links
* [Anti Patterns](http://www.datchley.name/promise-patterns-anti-patterns/)
* [More antipatterns](http://taoofcode.net/promise-anti-patterns/)

## Testing - Jasmine
Keep your modules small
Always throw / return errors when you need to and check for them in your tests
- success, error, expected and unexpected data
Describe block - module
it block - function
  - passing the done param forces jasmine to assume it is an async function

mocks
- sinon, rewire, node-tap, require-inject
- should only be used when the final result is to call an external service ie db / external api
- used to verify a call against an external service

spies

doubles

Code coverage
- istanbul
