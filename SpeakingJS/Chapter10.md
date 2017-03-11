# Converting to Boolean

Values | Converted to boolean
--- | ---
undefined | false
null | false
A boolean | Same as input (nothing to convert) 
A number | 0, NaN -> false; other numbers -> true
A string | '' -> false, other strings -> true
An Object | true (always)


## Manually Converting to Boolean

* Boolean(value)
* value ? true : false
* !!value

## Truthy and Falsy Values

Following values are converted to false:

* undefined, null
* Boolean: false
* Number: 0, NaN
* String: ''

All other values including **objects** are **truty**.

## Logical And (&amp;&amp;)

If the first operand can be converted to false, return it. Otherwise, return the second operand

## Logical OR (||)

If the first operand can be converted to true, return it. Otherwise, return the second operand.


The characeristic of **OR** can be used to provide a default value in some scenarios. 

Example 1, a default for parameter:

``` 
function saveText(text){
	text = text || '';
	...
}
```

Example 2, a default for a property

```
setTitle(options.title || 'Untitled');
```

Example 3, a default for the result of a function:

```
function countOccurrences(regex, str){
	return ( str.match(regex) || []).length;
}
```

The problem is that **match()** either returns an array or null.

## Logical Not (!)

## The function Boolean

Boolean() can be used as a function

Boolean(0)
// false

or a constructor

new Boolean(true)
// true, this is an object

