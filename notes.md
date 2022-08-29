# This Document

Notes on:

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)

# Overview

JavaScript supports OOP with object prototypes instead of classes.

JavaScript supports functional programming: functions are objects and may be stored in variables, passed around like any other object.

# Types

- Number
- BigInt
- String
- Boolean
- Object
  - Function
  - Array
  - Date
  - RegExp
- null
- undefined
- some built-in Error types

# Numbers

Number is a double-precision 64-bit IEE754 value.

When we say "integer" we mean "a representation of an int using a Number value."

An *apparent integer* is actually a float. 💩

```
0.1 + 0.2 == 0.30000000000000004;
```

## Math Object

`Math` provides advanced functions and constants e.g. `Math.sin` and `Math.PI`.


## Parsing / Converting

`parseInt` and `parseFloat` are built in functiona.

```
// Second arg is an optional base.
parseInt('123', 10);    // 123
parseInt('123yourmom', 10);    // 123
parseInt('010', 10);    // 10
parseInt('hello', 10);  // NaN


// Always base 10
parseFloat('123.45');   // 123

// Or use the unary operator 💩
+ '42';                 // 42
+ '010';                // 10
+ '0x10';               // 16
```

## NaN

`NaN` is toxic. (So is JavaScript, but that's another story....)

```
NaN + 5; // NaN
```

Test with `Number.isNaN(foo)`

But don't use `isNaN` because it sucks for stupid legacy reasons. 💩 lmao

```
isNaN('hello');       // true
isNaN('1');           // false
isNaN(undefined);     // true
isNaN({});            // true
isNaN([1])            // false
isNaN([1,2])          // true
```

To Infinity and beyond.

```
1 / 0;                // Infinity
-1 / 0;               // -Infinity
isFinite(1 / 0);      // false
isFinite(-Infinity);  // false
isFinite(NaN);        // false
```

## Strings



