# Expression Versus Statements

## Expressions

An Expression produces a value and can be written whever a value is expected.

## Statements

A *Statement* performs an action.


**In order to prevent ambiguity during parsing, JavaScript does not let you use object literals and function expressions as statements**. That is, expression statements must not start with:

* A curly brace
* The keyword **function**

## Evaluating an object literal via eval()

*eval* parses its argument in statement context. You have to put **parentheses** around an object literal if you want eval to return an object:

&gt; eval('{ foo: 123}')

123

&gt; eval('({ foo: 123})')

{ foo: 123 }

## Immediately invoking a function expression

**IIFE**

✅ &gt; (function () { return 'abc' }())

'abc'

❌ &gt; function () {return 'abc'} ()

SyntaxError: function statement requires a name

❌ &gt; function foo() {return 'abc'}()

SyntaxError: function statement requies a name

**Whatever follows a function declaration must be a legal statement and () isn't.**

# Control Flow Statements and Blocks

N/A

# Rules for Using Semicolons

Semicolons are optional in JavaScript. Missing semicolons are added via so-called automatic semicolon insertion (ASI; see Automatic Semicolon Insertion). However, that feature doesn’t always work as expected, which is why you should always include semicolons.

*Reader: because I am from a Java background, I always add a semicolon at the end of each statement.*

ASI dictates that a statement also ends if:

* A line terminator (e.g., a new line) is followed by an illegal token (Reader: ???)
* A closing brace is encountered.
* The end of the file has been reached.

ASI can cause issue in the following code:

```
return 
{
	name: 'John'
};
```

AS turns it into:

```
return;
{
	name: 'John'
};
```

# Legal Identifiers

N/A

**Reader: A good programmer should always know how to make his variables' name correct and meaningful.**


# Invoking Methods on Number Literals


❌ 1.toString()

✅

* 1..toString()
* 1 .toString()
* (1).toString()
* 1.0.toString()


# Strict Mode

## Switching on Strict Mode

**'use strict';**

Note that JavaScript engines that don’t support ECMAScript 5 will simply ignore the preceding statement.

You can also switch on strict mode per function. Type the statement at the beginning of your function body.

## Strict Mode: Recommended, with Caveats

Generally, changes checked by *strict mode* are all for the better. Hence, it is highly recommended to use in new code. However, there are two caveats:

* Enabling strict mode for existing code may break it
* Package with care

## Variables must be declared in Strict mode

## Functions in strict mode

* Functions must be declared at the top level of a scope


* Using the same parameter name twice for function is forbidden


* The arguments objects has fewer properties


* *this* keyword is *undefined* in nonmethod functions

for examnple:

```
function strictModeOff(){
	console.log(this === window); // true
}

function strictModeOn(){
	console.log(this === undefined); // true
}
```

This is useful for contstructors. Due to strict mode, you get a warning when you accidentally forget new and call it as a function

```
function Point(x, y) {
    'use strict';
    this.x = x;
    this.y = y;
}

var pt = Point(3, 1);
```

TypeError: Cannot set Property 'x' of undefined

## Setting and Deleting Immutable Properties Fails with an Exception in Strict mode

## Unqualified Identifiers Can't Be Deleted in Strict Mode

## eval() is cleaner in strict mode

## Features That are Forbindden in Strict mode

* The *with* statement is not allowed
* No more octal number. In sloppy mode, an interger with a leading zero is interpreted as octal while in strict mode, you get a syntax error.




