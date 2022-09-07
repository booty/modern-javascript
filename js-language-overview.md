# This Document

Notes on:

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)

# Overview

JavaScript supports OOP with object prototypes instead of classes.

JavaScript supports functional programming: functions are objects and may be stored in variables, passed around like any other object.

## Language and Runtime

JavaScript

## Types

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

## Numbers

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

They're UTF-16 code units. Each Unicode character is either 1 or 2 code units.

```
'hello'.length;                           // 5
'hello'.charAt(0);                        // "h"
'hello, world'.replace('world', 'mars');  // "hello, mars"
'hello'.toUpperCase();                    // "HELLO"
```

New-school interpolation

```
let x = 42;
let name = "John";
console.log(`${name} is ${x} years old.`);
```

## `null` vs. `undefined`

`null` is a deliberate non-value

`undefined` is an uninitialized variable (actually a constant)


## Boolean

`true` and `false` are keywords

Things that evaluate to `false`:

- `false`
- `0`
- empty strings (`""`)
- `NaN`
- `null`
- `undefined`

Things that evaluate to `true`

- Everything else

Can convert explicitly with `Boolean()`

## Variables

Traditionally, blocks had no scope. But starting with ECMAScript2015, `let` and `const` were introduced.

`let` declares block-level variables

`const` is also block-level

`var` function level (is hoisted)

## Equality

`==` and `!=` perform type coercion

`===` and `!==` do not perform type coercion (prefer these ✅)


## Operators

`&&` and `||` are short-circuit.

💡Checking for null objects before accessing their attribs:

```
const name = o && o.getName();
```

💡"Caching" values

```
// assumes falsey values can't happen :-(
const name = cachedName || (cachedName = getName());
```

## Control Structures

if/else

```
let name = 'kittens';

if (name === 'puppies') {
  name += ' woof';
} else if (name === 'kittens') {
  name += ' meow';
} else {
  name += '!';
}
```

while and do/while

```
while(true) {
  // infinity!
}

let input;
do {
  input = get_input();
} while (inputIsNotValid(input));
```

for

```
for (let i = 0; i < 5; i++) {
  // runs 5 times
}
```

for/of

```
for (const value of array) {
  // do something with value
}
```

for/in

```
for (const prop in object) {
  // do something with prop
}
```

switch

```
// comparisons made with ===
switch (action) {
  case 'sketch':  // will fallthrough
  case 'draw':
    drawIt();
    break;
  case 'eat';
    eatIt();
    break;
  default:
    takeNap();
}
```

## Objects

JavaScript objects can be thought of as name-value pairs.

Similar to hashes in Perl, Ruby.

Name is always a JavaScript string.

```
const obj = new Object();   // same as below
const obj = {};             // "object literal syntax", preferred
```

Accessing elements:

```
const obj = {
  name: 'Carrot',
  _for: 'Max', // 'for' is a reserved word, use '_for' instead.
  details: {
    color: 'orange',
    size: 12
  }
};

obj.details.color;        // orange
obj['details']['size'];   // 12
```

Iterate through with `for (const x in obj) { x }`

## Object Prototypes

```
// prototype
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// instance
const you = new Person('You', 46)''

// you can still define arbitrary attributes
you.hair = true;
```

## Arrays

They work like regular objects but have a magic property called `length.`

```
const a = ['dog', 'cat', 'hen'];
a[100] = 'fox';
a.length;                             // 101
typeof a[90];                         // undefined
```

Old-fashioned iteration

```
for (let i = 0; i < a.length; i++) {
  // Do something with a[i]
}
```

ES2015 style iteration

```
const ary = ['dog', 'cat', 'hen'];
ary[10] = 'fox';
ary.length; // 111

for (const x of ary) {
  console.log(x);
}

/*
dog
cat
hen
undefined
undefined
undefined
undefined
undefined
undefined
undefined
fox
*/
```

ECMAScript5

```
['dog', 'cat', 'hen'].forEach(function(currentValue, index, array) {
  console.log((index + 1) + '/' + array.length + ' ' + currentValue);
});

/*
1/3 dog
2/3 cat
3/3 hen
 */
```

## Functions

You can call a function without the parameters it expects -- all parameters are optional. Omitted params will `undefined`.

Can also pass in extra params.

```
function add(x,y) {
  return x + y
}

add();          // NaN
add(2);         // NaN
add(2, 3, 4);   // 5 (the last arg was ignored)
```

`arguments` - An array of all arguments.

```
function add() {
  let sum = 0;
  for (const x of arguments) {
    sum += x;
  }
  return sum;
}

add(1, 2, 3, 4);    // 10
```

### Rest Parameter Operator

```
function foo(a, ...otherCrap) {
  console.log('a is ' + a + ' and there were ' + otherCrap.length + ' args I do not care about');
}

foo(42);                          // a is 42 and there were 0 args I do not care about
foo(42, "your", "mom", "hello");  // a is 42 and there were 3 args I do not care about
```

### Apply

```
function cool() {
  console.log('got ' + arguments.length + ' args');
}

my_ary = [1, 2, 3];

cool(my_ary);                 // got 1 args
cool.apply(null, my_ary);     // got 3 args
```

### Anonymous Functions

```
let sum = function() {
  return arguments[0] + arguments[1];
}

sum(1, 2);      // 3
```

### Arrow Functions

```
function times(ary, x) {
  return ary.map((num) => num * x);
}

times([1, 5, 99], 2);
```

### Inner Functions

N.B.: Nested functions in JavaScript have access to variables in the parent function's scope.

```
function multiply(x) {
  function inner1(inner1x) {
    function inner2(inner2x) {
      return multiplier * inner2x;
    }

    return inner2(x);
  }
  let multiplier = 2;

  return inner1(x);
}
```

## Classes

JavaScript classes:

- Just functions
- Instantiate with `new`
- `new` returns an object containing methods + properties that the class specified


```
class Person {
  constructor(name) {
    this.name = name;
  }
  sayHello() {
    return `Hello, I'm ${this.name}`;
  }
}

const p = new Person("Durango");  // error - Durango is actually a dog
p.sayHello();
```

### Classes - Mixin Pattern

```
const withAuthentication = (cls) =>
  class extends cls {
    authenticate() {
      console.log("Fuckin' authenticated! 🤘🎸");
      return true;
    }
  }
class Admin extends withAuthentication(Person) { }

boss.authenticate(); // Fuckin' authenticated! 🤘🎸
```

## Async Programming

Callback-based. You know, like `setTimeout()`.

The core language doesn't specify any async features, but it's crucial when interacting w/ external world.

```
fs.readFile(filename, (err, content) => {
  // this callback invoked when file read, which could take a while
  if (err) {
    throw err;
  }

  console.log(content);
});
```

Promise-based.

```
fs.readFile(filename)
  .then((content) => {
    console.log(content);
  }).catch((err) => {
    throw err;
  });
```

Async/await. This is a syntactic sugar for Promises.

```
async function readFile(filename) {
  const content = await fs.readFile(filename);
  console.log(content);
}
```

If you have an async value, it's not possible to get its value synchronously.

- In a promise, can only access the value in `then()` method.
- `await` can only be used in an async context
  - Usually an async function or module
- Promises are never blocking
- Promises similar to `monads` from functional programming

Single-threaded model has made Node.js a popular choice for server-side programming.


## Paralleling

You can't.

If you truly need it, you probably need `workers`.


## Modules

Supported by most runtimes.

```
import { foo } from "./foo.js";

// Unexported, so local to the module
const b = 2;

export const a = 1;
```

