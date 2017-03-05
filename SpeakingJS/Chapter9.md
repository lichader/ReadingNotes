# Operators and Objects

All operators coerce their operands to appropriate types. Most operators only work with primitive values.

**Note: there is no way to *overload* or customize operators in JavaScript**

# Assignment Operators

N/A

## Compound Assignment Operators

* Arithmetic operations: *=, /=, %=, +=, -=\
* Bitwise operations: <<=, >>=, >>>=, &=. ^=, |=
* String concatenation: +=

# Equality Operators: === Versus ==

* Strict equality (===) and strict inequality (!==) consider only values that have the same type to be equal
* Normal equality (==) and normal inequality (!=) try to convert values of different types before comparing them as with strict (in)equality.

Always use strict equality because lenient equality has two problems:

1. How it performs conversion is confusing
2. due to the operators being so forgiving, type errors can remain hidden longer.

## Strict Equality (===, !==)

Values with different types are never strictly equal. If both values have the same type, then the following assertions hold:

* undefined === undefined
* null === null
* Two numers:
	* x === x 
	* +0 === -0
	* NaN !== NaN
* Two booleans, two String
* Two objects: x === y if and only if x and y are same object.
* Everything else: not strictly equal

### Pitfall: NaN

The special number value NaN is not equal to itself.

## Normal (lenient) Equality (==, !=)

If both operands have same type, then compare them via strict equality.

Otherwise, if the operands are:

1. undefined and null, then they are considred leniently equal
2. A string and a number, then convert the string to a number and compare both operands via strict equality
3. A boolean and a nonboolean, then convert the boolean to a number and compare leniently
4. An object and a number or a string, then try to convert the object to a primitive and compare leniently
5. Otherwise, the result is false

# Ordering Operators

* &gt;
* &gt;=
* &lt;
* &lt;=

They work for **numbers** and **strings**

## The Algorithm

Comparison steps:

1. Ensure that both operands are primitives. Objects are converted to primitives via the internal operation **ToPrimitive(obj, Number)**, which calls obj.valueOf() and, possiblu, obj.toString() to do so.
2. If oth operands are strings, then compare them by lexicographically comparing the 16-bit code units that represent the JavaScript characters of the strings.
3. Otherwise, convert both operands to numbers and compare them numerically

# The Plus Operator (+)

The plus operator examines its operands. If one of them is a string, the other is also converted to a string and both are concatednated. Otherwise, both operands are converted to numbers.

# Operators for Booleans and Numbers

Same as other language

# Special Operators

## The conditional Operator ( ? :)

condition ? if_true : if_false

## The comma operator

%left%, %right%

it evaluates both operands and returns the result of right. This is confusing. Hence, it is recommended to avod not using it.

## The void operator

void %expr%

it evaluates **expr** and returns **undefined**.

**What is void used for?**

It is rarely useful under ECMAScript 5. Its main use cases are:

* void 0 **as a synonym for ** undefined
* Discarding the result of an expression
* Prefixing an IIFE


# Categorizing Values via typeof and instanceof

* The **typeof** operator distinguishes primitives from objects and determines the types of primitives.
* The **instanceof** operator determines whether an object is an instance of a given constructor.

## typeof: Categorizing primitivies

In the past chapters, there is a chapter named: JavaScript's Types that has a table listing all returning result of "typeof". Refer to that for more detIls. There is a pitfall that the result of **typeof null** is **object**. This is considered as a bug but can't be fixed as it will break existing code.

## instanceof: Checking Whether an Object Is an Instance of a Given Constructor

**%value% instanceof %Constr%**

&gt; {} instanceof Object
true

&gt; [] instanceof Array
true

&gt; [] instanceof Object
true

&gt; undefined instanceof Object
false

&gt; null instanceof Object 
false // this is correct

