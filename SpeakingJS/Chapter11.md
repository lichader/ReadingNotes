JavaScript has a single type for all numbers.

JavaScript numbers are double (64-bit) values.

# Number Literals

A number literal can be an integer, floating point, or (integer) hexadecimal:

* 35
* 3.141
* 0XFF


## Exponent

examples:

* 5e2
* 5e-2
* 0.5e2

## Invoking Methods on Literals:

* 123..toString()
* 123 .toString()
* 123.0.toString()
* (123).toString()


# Converting to Number

Values | Result
---|---
undefined | NaN
null | 0
A boolean | false -> 0, true -> 1
A number | same as input
A string | Parse the number in the string, the empty string is converted to 0
An object | call ToPrimitive(value, Number) and convert the result primitive

## Manually conversion to NUmber

use **Number()** function

There is also a global function called **parseFloat()** that also converts values to numbers. However, Number() is usually a better choice.

Comparing **parseFloat()** and **Number()**:

* Applying **parseFloat()** to a nonstring is less efficient, because it coerces its argument to a string before parsing it. As a consequence, many values that **Number()** converts to actual numbers are converted to **NaN** by **parseFloat()**
* **parseFloat()** parses the empty string as NaN
* **parseFloat()** parses until the last legal characgter, meaning you get a result where you may not want one
* parseFloat() ignores leading whitespace and stops before illegal characters (which include whitespace), **Number()** ignores both leading and trailing whitespace (but other illegal characters lead to NaN).


# Special Number Values

JavaScript has serveral special number values:

* Two error values, NaN and Infinity
* Two values for zero, +0 and -0.


## NaN

The error value **NaN** is a number value

&gt; typeof NaN
'number'

It is produced by errors such as the following:

* A number could not be parsed
* An operation failed
* One of the operands is NaN

### Pitfall: checking whether a value is NaN

**NaN** is the only value that is not equal to itself

Note: strict equality is also used by Array.prototype.indexOf. You therefore can't search **NaN** in an array via the method.

If you want to check whether a value is NaN, you have to use the global function **isNaN()**.

However, **isNaN** does not work properly with **nonnumbers**, because it first converts those to numbers. That conversion can produce **NaN** and then the function incorrectly returns true.

Thus, it is best to combine **isNaN** with a type check:

```
function myIsNaN(value) {
	return typeof value === 'number' && isNaN(value);
}
```

Alternatively, you can check whether the value is unequal to itself

> value !== value


## Infinity

**Infinity** is an error indicating one of two problems: a number can't be represented because its magnitude is too large, or a division by zero has happened.

**Infinity** is larger than any other number (except NaN). On the other hand, **-infinity** is smaller than any other number (except NaN). 

### Error: a number's magnitude is too large

How large a number's magnitude can become is determined by its internal representation, which is the arithmetic product of:

* A mantissa 
* 2 to the power of an exponent

The exponent must be between (and excluding) -1023 and 1024. If the exponent is too small, the number becomes 0. If the exponent is too large, it becomes **infinity**.

Example:

&gt; Math.pow(2, 1023)
8.98846567431158e+307

&gt; Math.pow(2, 1024)
infinity


### Error: division by zero

Dividing by zero produces **Infinity** as an error value

### Computing with infinity

## Checking for infinity

Strict and lenient equality work fine for **Infinity**

Additional, the global function isFinite() allows you to check whether a value is an actual number (neither infinite nor NaN).

## Two zeros

Because JavaScript numbers keep magnitude and sign seperate, seach nonnegative number has a negative, including 0.

### Best practice: pretend there's only one zero

In JavaScript, you normally write 0, which means +0. But -0 is also displayed as simply 0.

The standard toString() method converts both zeros to the same '0'.

Equality doesn't distinguish zeros. either. Not Even ===.

The ordering operators also consider the zeros to be equal.

### Distinguishing the two zeros

You can divide by zero to distinguish two zeros.

&gt; 3 / -0
-Infinity

&gt; 3 / +0
-Infinity

Another way to perform the division by zero is via Math.pow()

&gt; Math.pow(-0, -1)
-Infinity

&gt; Math.pow(+0, -1)
Infinity

Math.atan2() also reveals that the zeros are different.

The canonical way of telling the two zeros apart is the division by zero. 

# The Internal Representation of Numbers

JavaScript numbers have 64-bit precision, which is also called **double precision**. The 64 bits are distributed between a number's sign, exponent, and fraction as follows:

Sign | Exponent | Fraction
--- | --- | ---
1 bit | 11 bits | 52 bits
Bit 63 | Bits 62-52 | Bits 51-0

## Special Exponents

-1023 and 1024 are special exponents:

1024 is used for error values such as NaN and Infinity

-1023 is used for: 

* Zero (if the fraction is 0)
* Small numbers close to zero

# Handling Rounding Errors

JavaScript's numbers are usually entered as decimal floating-point numbers, but they are internally represented as binary floating-point numbers, which leads to imprecision. In the decimal system, all fractions are a mantissa *m* divided by a power of 10. So, in the denominator, there are only tens. That's why 1/3 cannot e expressed precisely as a decimal floating-point number as there is no way to get a 3 into the denominator. Binary floating-point numbers can be represented well as binary and which can't.

Due to rounding errors, as a best practice you should not compare nonintegers directly. Instead, take an upper bound for rounding errors into consideration. Such an upper bound is called a **machine epsilon**.

example:

```
var EPSILON = Math.pow(2, -53);
function epsEqu(x, y) {
	return Math.abs(x - y) < EPSILON;
}
```

# Integers in JavaScript

JavaScript has only floating-point numbers. Integers appear internally in two ways.

* Most JavaScript engines store a small enough number without a decimal fraction as an integer and maintain that representation as long as possible. They have to switch back to a floating-point representation if a number's magnitude grows too large or if a decimal fraction appears
* The ECMAScript specification has integer operators: namely, all of the bitwise operators. Those operators convert their operands to 32-bit integers and return 32-bit integers. For the specification, integer only means that the numbers don't have a decimal fraction, and 32-bit means that they are within a certain range. 

## Ranges of Integers

Internally, the following ranges of integers are important in JavaScript:

* Safe integers, the largest practically usable range of integers that JavaScript supports: 53 bits plus a sign, range from $$-2^{53}$$  to $$2^{53}$$ 
* Array indices, 32 bits, unsiged, Maximum length: $$2^{32}$$ minus 1 (excluding)
* Bitwise operands, unsigned right shift operator (&gt;&gt;&gt;): 32 bits, unsigned, range [0, $$2^{32}$$), all other bitwise operators: 32 bits, including a sign, range [$$-2^{31}$$, $$2^{31}$$)
* "Char codes," UTF-16 code units as numbers:
	* Accepted by String.fromCharCode()
	* REturned by String.prototype.charCodeAt()
	* 16 bits, unsigned


## Representing integers as Floating-Point Numbers

JavaScript can only handle integer values up to a magnitude of 53 bits (the 52 bits of the fraction plus 1 indirect bit, via the exponent).

### Best practice

If you work with integers of up to 53 bits magnitude, you are fine. Unfortunately, you’ll often encounter 64-bit unsigned integers in programming (Twitter IDs, databases, etc.). These must be stored in strings in JavaScript. If you want to perform arithmetic with such integers, you need special libraries. There are plans to bring larger integers to JavaScript, but that will take a while.

## Safe Integers

JavaScript can only safe represent integers between: $$-2^{53}$$ and  $$2^{53}$$ (both excluding).

In this ragne, JavaScript integers are safe: there is a one-to-one mapping between mathmatical integers and their representations in JavaScript.

Beyond this range, JavaScript integers are unsafe: two or more mathmatical integers are represened as the same JavaScript integer. 

### Definitions in ECMAScript 6

**ECMAScript 6** will provide the following constants:

> Number.MAX_SAFE_INTEGER = Math.pow(2, 53) - 1;
> Number.MIN_SAFE_INTEGER = -Number.MAX_SAFE_INTEGER;

It will also provide a function for determining whether an integer is safe:

``` 
Number.isSafeInteger = function (n) {
	return (typeof n === 'number' &&
		Math.round(n) === n &&
		Number.MIN_SAFE_INTEGER <= 0 &&
		n <= Number.MAX_SAFE_INTEGER);
}
```

# Converting to Integer

In JavaScript, all numbers are floating point. Integers are floating-point numbers without a fraction. Converting a number *n* to an integer means finding the integer that is "closest" to *n*. There are several options for performing this conversion:

* The Math functions: Math.floor(), Math.ceil(), and Math.round()
* The custom function ToInteger()
* Binary bitwise operators
* The global function parseInt()

> Math.floor() converts its argument to the cloest lower integer
> Math.ceil() converts its argument to the cloest higher integer
> Math.round() converts its argument to the closest integer

## Custom function ToInteger()

```
function ToInteger(x) {
	x = Number(x);
	return x < 0 ? Math.ceil(x) : Math.floor(x);
}
```

## 32-bit integers via Bitwise Operators

Binary bitwise operators convert one of their operands to a 32-bit integer that is then manipulated to produce a result that is also a 32-bit integer. 

### Bitwise Or (|)

If the mask, the second operand, is 0, you don't change any bits and the result is the first operand, coerced to a signed 32-bit integer.

```
function ToInt32(x) {
	return x | 0;
}
```

### Shift operators 

The same trick that worked for bitwise OR also works for shift operators: if you shift by zero bits, the result of a shift operation is the first operand, coerced to a 32-bit integer.

```
// Convert x to a signed 32-bit integer
function ToInt32(x) {
    return x << 0;
}

// Convert x to a signed 32-bit integer
function ToInt32(x) {
    return x >> 0;
}

// Convert x to an unsigned 32-bit integer
function ToUint32(x) {
    return x >>> 0;
}
```

### Should I use bitwise operators to coerce to integer?

You have to decide for yourself if the slight increase in efficiency is worth your code being harder to understand. Also note that bitwise operators artificially limit themselves to 32 bits, which is often neither necessary nor useful. Using one of the Math functions, possibly in addition to Math.abs(), is a more self-explanatory and arguably better choice.

## Integers via parseInt()

The parseInt() function:

> parseInt(str, radix?)

parses the string *str* as an integer. The function ignores leading whitespace and considers as many consecutive legal digits as it can find.

### The radix

The range of the radix is 2 <= radix <= 36. It determines the base of the number to be parsed. If the radix is greater than 10, lettes are used as digits, in addition to 0-9.

If radix is missing, then it is assumed to be 10, except if *str* begins with "0x" or "0X", in which case radix is set to 16 (hexadecimal)

Note: it is best to **always explicitly state the radix**, to always call parseInt() with two arguments.

Don't use parseInt() to convert a number to an integer because the first argument is first converted to a string. If the converted string is dispalyed in exponent, then the "e" is ignored during parsing.

for example:

&gt; parseInt(0.0000008, 10)

8

&gt; String(0.0000008)

'8e-7'

### Summary

parseInt() shouldn't be used to convert numbers to integers. Even it is useful for parsing strings, but you have to be aware that it stops at the first illegal digit.

# Arithmetic operators

Same as other language!

# Bitwise Operators

JavaScript has serveral bitwise operators that work with **32-bit** integers. That is, they convert their operands to 32-bit integers and produce a result that is a 32-bit integer.

parseInt() can parse a string in binary notation. On the other hand, num.toString(2) converts the number *num* to a string in binary notation.

# The Function Number

* Number(value) as a normal function - do the conversion
* new Number(value) as a constructor - create new instance of Number


# Number Constructor Properties

* Number.MAX_VALUE
* Number.MIN_VALUE
* Number.NaN
* Number.NEGATIVE_INFINITY
* Number.POSITIVE_INFINITY


# Number Prototype Methods

* Number.prototype.toFixed(fractionDigits?) returns an exponent-free representation of the number, rounded to *fractionDigits* digits. If the parameter is omitted, the value 0 is used. If the number is greater than or equal to 10 of power 21, then this method works the same as toString()
* Number.prototype.toPrecision(precision?) prunes the mantissa to *precision* digits before using a conversion algorithm similar to toString(). If no precision is given, *toString()* is used directly.
* Number.prototype.toString(radix?), the parameter *radix* indicates the base of the system in which the numer is to be displayed. The most common radices are *10 (decimal), 2 (binary), and 16 (hexadecimal)*. In addition, for the radix 10, toString() uses exponential notation in two cases. 

First, if there are more than 21 digits before the decimal point of a number:

&gt; 1234567890123456789012

1.2345678901234568e+21

&gt; 123456789012345678901

123456789012345680000

Second, if a number starts with 0. followed by more than five zeros and a non-zero digit:

&gt; 0.0000003

3e-7

&gt; 0.000003

0.000003

* Number.prototype.ToExponential(fractiondigits?) forces a number to be expressed in exponential notation. *fractionDigits* is a number between 0 and 20 that determines how many digits should be shown after the decimal point. If it is omitted, then as many significant digits are included as necessary to uniquely specify the number.

# Functions for Numers

* isFinite(number)
* isNaN(number)
* parseFloat(str)
* parseInt(str, radix?)