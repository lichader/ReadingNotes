# Primitive Values Versus Objects
JavaScript makes a somewhat arbitrary distinction between values:

* The primitive values are booleans, numbers, strings, null, and undefined.
* All other values are objects.

## Primitive Values

* Compared by value
* Always immutable

## Objects

All nonprimitive values are objects. The most common kinds of objects are:

* Plain objects
* Arrays
* Regular Expression

**Characteristics**
* Compared by reference
* Mutable by default

## undefined and null

JavaScript has two "nonvalues": undefined and null:

### undefined
* undefined means "no value". For example, uninitialized variables are **undefined**
* Missing parameters are **undefined**
* Nonexistent property is also **undefined**

### null

* null means "no object". It is used as a nonvalue whenever an object is expected. 

**undefined** and **null** have no properties, not even standard methods such as toString().

### Checking for undefined or null

```JavaScript
if (x === undefined || x === null){
	...
}
```

alternatively, both **undefined** and **null** are considred **false**:

```JavaScript
if (!x){
	...
}
```

At last, **false, 0, NaN, and ''** are also considred **false**.

## Categorizing Values Using typeof and instanceof

### typeof

All results of typeof:

**Operand** | **Result**
--- | ----
undefined | 'undefined'
null | 'object' (This is a bug!! can't be fixed as it will break existing code
Boolean value | 'boolean'
Number value | 'number'
String value | 'string'
Function | 'function'
All other normal values | 'object'
(Engine created value) | JavaScript engines are allowed to create values for which typeof returns arbitrary strings (different from all results listed in the table).

### insanceof

``` 
value instanceof Constr
```

It returns true if **value** is an object that has been created by the constructor **Constr**.

## Booleans

* true
* false

Following operators produce booleans:

* &&, ||
* !
* ===, !===, ==, !=
* >, >=, <, <=

Following values will be considered as false when JavaScript expects a boolean value:

* undefined, null
* Boolean: false
* Number: 0, NaN
* String: ''

All other values (including all objects) are considred **true**.

Binary logical operators in JavaScript are short-circuiting. That is, if the first operand suffices for determining the result, the second operand is not evaluated. 

### Equality Operators
JavaScript has two kinds of equality:

* Normal, or “lenient,” (in)equality: == and !=
* Strict (in)equality: === and !==
* 
Normal equality considers (too) many values to be equal (the details are explained in Normal (Lenient) Equality (==, !=)), which can hide bugs. Therefore, **always using strict equality is recommended.**


## Numbers

All numbers in JavaScript are floating-point.

### Special Numbers:

#### NaN ("**Not A Number**")

e.g 
Number('xyz') -> NaN

#### Infinity

3 / 0 -> Infinity

**Infinity** is larger than any other number (exception NaN). Similarly, **-infinity** is smaller than any other number (except Nan).

### Operators

Addition: number1 + number2
Subtraction: number1 - number2
Multiplication: number1 * number2
Division: number1 / number2
Remainder: number1 % number2
Increment: ++variable, variable++
Decrement: --variable, variable--
Negate: -value
Convert to number: +value

## Strings

Strings can be created directly via literals. Those literals are delimited by single or double quotes. The backslash (\) escapes charactes and produces a few control characters.

Single characters can be accessed via square brackets:

```
var str = 'abc';
str[1]
```

Property **lengh** counts the number of characters.

Strings are immutable.

### String operators

Strings are concatenated via the plus (+) operator

To concatenate strings in multiple steps, use the += operator

### String methods

sub string: **slice()**

trim whitespace: **trim()**

upper case: **toUpperCase()**

search string: **indexof('')**

## The Special Variable arguments

You can call any function in JavaScript with an arbitrary amount of arguments; the language will never complain. It will, however, make all parameters available via the special variable **arguments** that will look like an array, but has none of the array methods.

### Too Many or Too Few Arguments

* Additional parameters will be ignored
* Missing parameters will get the value **undefined**

### Optional Parameters

The following is a common pattern for assigning default values to parameters:

```
function pair(x, y) {
    x = x || 0;  // (1)
    y = y || 0;
    return [ x, y ];
}
```

In line (1), the || operator returns x if it is truthy (not null, undefined, etc.). Otherwise, it returns the second operand

This is because JavaScript will evaulate first operand in boolean operators. 

### Enforcing an Arity

If yo want to **enfoce** an arity, you can check arguments.lengh

``` 
function pari(x, y){
	if (arguments.lenght !=== 2){
		throw new Error('Need exactly 2 arguments');
	}
}
```

### Converting arguments to an Array

**arguments** is not an array, it is only *array-like*.

Use:

```
Array.prototype.slice.call(arrayLikeObject);
```

to convert these *array-like* objects.

## Exception handling

```
try {
	...
} catch (exception) {
	...
}
```

## Strict mode

Strict mode enables more warning and makes JavaScript a cleaner language. 

To switch it on, type the following line first in JavaScript file or a &lt;script&gt; tag:

> 'use strict';

You can also enable strict mode per function

## Variable Scoping and Closures

* Variables are function-scoped
* Variables are Hoisted. Each variable declaration is hoisted: the declaration is moved to the begining of the function, but assignments that it makes stay put.

### Closures

Each function stays connected to the variables of the functions that surround it, even after it leaves the scope in which it was created.

A ***closure*** is a function plus the connection to the variable of its surrounding scopes.

### The IIFE Pattern

An IIFE is a function expression that is called **immediately** after you define it. The IIFE stands for **Immediately Invoked Function Expression**. This can be used to introduce a new variable scope.

**Use case**

*Use closure*

```
var result = [];
for (var i=0; i < 5; i++) {
    result.push(function () { return i });  // (1)
}
console.log(result[1]()); // 5 (not 1)
console.log(result[3]()); // 5 (not 3)
```

This didn't give us the value that we want.

*Use IIFE*

```
for (var i=0; i < 5; i++){
	(function(){
		var i2 = i; // copy current i
		result.push(function() {return i2});
	}());
}
```

This way can keep a snapshot of the value of variable: i.

## Objects and Constructors

### Single Objects

Object can be considered as a set of properties, where each property is a (key, value) pair. The key is a string, and the value is any JavaScript value.

The *in* operator checks whether a property exists:

``` 
'newProperty' in jane
```

If you ready a property that doesn't exist, you get the value **undefined**.

The *delete* operator removes a property

### Arbitrary Property Keys

A property key can be any string.You can also use a square to get and set the property.

### Extracting Methods

If you extract a method, it loses its connection with the object. On its own, the function is not a method anymore.

Example:
```
'use strict';
var jane = {
	name: 'Jane',
	
	describe: function(){
		return 'Person named ' + this.name;
	}
};
```

``` 
var func = jane.describe;
func() 
```
**TypeError: Cannot read property 'name' of undefined**

The solution is to use the method **bind()** that all functions have.

``` 
var func2 = jane.describe.bind(jane);
func2()
```

### Functions inside a method

Every function has its own special variable **this**. This is inconvinient if you nest a function inside a method and want to access the **this** in the method.

```
var jane = {
    name: 'Jane',
    friends: [ 'Tarzan', 'Cheeta' ],
    logHiToFriends: function () {
        'use strict';
        this.friends.forEach(function (friend) {
            // `this` is undefined here
            console.log(this.name+' says hi to '+friend);
        });
    }
}
```

However, there are two solutions:

1. Assign this to a variable

```
logHiToFriends: function () {
    'use strict';
    var that = this;
    this.friends.forEach(function (friend) {
        console.log(that.name+' says hi to '+friend);
    });
}
```

2. forEach has a second parameer that allows you to provide a value for this:

```
logHiToFriends: function () {
    'use strict';
    this.friends.forEach(function (friend) {
        console.log(this.name+' says hi to '+friend);
    }, this);
}
```

## Constructors: Factories for Objects

Functions can become constructors if they are invoked via the **new** operator.

```
// Set up instance data
function Point(x, y) {
    this.x = x;
    this.y = y;
}
// Methods
Point.prototype.dist = function () {
    return Math.sqrt(this.x*this.x + this.y*this.y);
};
```

## Arrays

### Array Literals

```
var arr = ['a', 'b', 'c'];
```

You can access these elments via int indices.

THe length property indicates how many elements an array has. You can use it to **append** elements and to **remove** elements.

The **in** operator works for arrays too.

Arrays are also objects and can thus have object properties.

### Array Methods

Array has many methods

* slice(start, end) // copy elements
* push( ) //append an element
* pop() // remove last element
* shift() // remove first element
* unshift('') // prepend an element
* indexof('') // find the index of an element
* join('') // all elements in a single string

### Iterating over arrays

* forEach iterates over an array and hands the current element and its index to a function
* map creates a new array by applying a function to each element of an existing array

## Regular Expression

JavaScript has built-in support for regular expressions. They are delimited by slashes:

```
/^abc$/
/[A-Za-z0-9]+/
```

Methods:

* test(): is there a match. e.g /^a+b+$/.test('aaab')
* exec(): match and capture groups.
* replace(): Search and Replace. The first parameter of *replace* must be a regular expression with a /g flag; otherwise, only the first occurence is replaced

## Math

* Math.abs()
* Math.pow()
* Math.max()
* Math.round()
* Math.PI // pre-defined constant for n
* Math.cos(Math.PI)

## Other Functionality of the Stand Library

* Date, a constructor for dates whose main functionality is parsing and creating date strings and accessing the components of a date
* JSON, an object with functions for parsing and generating JSON data
* console.**methods*, browser-specific methods