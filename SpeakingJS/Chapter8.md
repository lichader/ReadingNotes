All normal values in JavaScript have properties each of which has a key (or name) and a value. 

# JavaScript's Type System

## JavaScript's Types

JavaScript has only six types, according to the Chapter 8 of the ECMAScript spec:

* Undefined, Null
* Boolean, String, Number, and
* Object

**JavaScript is a Dynamic Typing language**

Statically type-checked languages perform this kind of check at compile time, while dynamically type-checked languages do so at runtime.

## Coercision

JavaScript does implicit type conversion but JavaScript's built-in conversion mechanisms only support the types: 

* Boolean
* Number
* String
* Object

# Primitive Values Versus Objects

In JavaScript:

* Booleans, numbers, strings, null and undefined are all primitive values
* All other values are *objects*

A major difference between the two is how they are compared. Each object has a unique identity and is only (**strictly**) equal to itself.

In contrast, all primitive values encoding the same value are considered the same.

# Primitive

* Compared by value
* Always immutable
* A fixed set of types

# Objects

* Plain objects can be created by object literals
* Arrays can be created by array literals
* Regular expressions can be created by regular expression literals

Characteristics:

* Compared by reference
* Mutable by default
* User-extensible

# undefined and null

* undefined means no "no value" (neither primitive nor object). Uninitialized variables, missing parameters, and missing properties have this nonvalue. And functions implicitly return it if nothing has been explicityly returned.
* null means "no object". It is used as a nonvalue where an object is expected.

undefined is also sometimes used as more of a metavalue that indicates nonexistence. In contrast, null indicates emptiness.

## Occurrences of undefined and null

* Unintialized variables are undefined
* Missing parameters are undefined
* Access to a nonexistent property
* funtion that doesn't return anything implicitly returns undefined


## Occurrences of null

* null is the last element in the prototype chain
* null is returned by RegExp.prototype.exec() if there was no match for the regular expression in the string

## Checking for undefined or null

### Checking for null

```
if (x ==== null) 
```

### Checking for undefined

```
if (x === undefined)
```

You can also check for undefined via the typeof operator but that is not recommended for most of situation where you can use the three equals approach.

### Checking for either undefined or null

```
if (x !== unefined && x !== null)
```

However, because both of them are considered false, so we can use:
```
if (x)
{
	// x has value
}
else 
{
	// x does not have a value
}
```

# Wrapper Objects for Primitives

The three primitive types boolean, number, and string have corresponding constructors: Boolean, Number, and String.

## Wrapper objects are different from primitives

## Wrapping and Unwrapping Primitives

* Wrapping primitives:

``` 
new Boolean(true)
new Number(123)
new String('abc')
```

* Unwrapping primitives:

``` 
new Boolean(true).valueOf()
new Number(123).valueOf()
new String('abc').valueOf()
```

## Primitives Borrow Their Methods from Wrappers

for example:

&gt; 'abc'.charAt === String.prototype.charAt
true

Sloppy mode and strict mode handle this borrowing differently. 

In sloppy mode, primitives are converted to wrappers on the fly. 

```
String.prototype.sloppyMethod = function () {
    console.log(typeof this); // object
    console.log(this instanceof String); // true
};
''.sloppyMethod(); // call the above method
```

In strict mode, methods from the wrapper prototype are used transparently. 

```
String.prototype.strictMethod = function () {
    'use strict';
    console.log(typeof this); // string
    console.log(this instanceof String); // false
};
''.strictMethod(); // call the above method
```

# Type Coercion

Type coercion means the implicit conversion of a value of one type to a value of another type. Most of JavaScript’s operators, functions, and methods coerce operands and arguments to the types that they need. 

drawback:

* type coercion can hide bugs

## Functions for Converting to Boolean, Number, String, and Object

### Boolean()

* undefined, null
* false
* 0, NaN
* ''

above are converted to false. 

### Number()

* undefined becomes NaN
* null becomes 0
* false becomes 0, true becomes 1
* Strings are parsed
* Objects are first converted to primitives, which are then converted to numbers.

### String()

### Object()

Converts objects to themselves, undefined and null to empty objects, and primitives to wrapped primitives.

## Algorithm: ToPrimitive() - Converting a Value to a Primitive

**ToPrimitive()** is an internal function that performs the conversion from object to an arbitrary primitive. This function is not accessible from JavaScript.

It has following signature:

> ToPrimitive(input, PreferredType)

The optional parameter PreferredType indicates the final type of the conversion: it is either Number or String, depending on whether the result of ToPrimitive() will be converted to a number or a string.

If PreferredType is Number, following steps are performed:

1. If input is primitive, return it
2. Otherwise, input is an object. Call input.valueOf(). If the result is primitive, return it
3. Otherwise, call input.toString(). If the result is primitive, return it
4. Otherwise, throw a TypeError (indicating the failure to convert input to a primitive)

If PreferredType is String, step 2 and 3 are swapped. 

The preferredType can be omitted; it is then considered to be String for dates and Number for all other values.

Note: the default implementation of valueOf() return *this*, while the default implementation of toString() returns type information.