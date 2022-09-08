# Closures

From: [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

A **closure** is the combination of:

- a function
- references to its surrounding state (the **lexical environment**)

Closure gives you access to outer function's scope from an inner function.

Every function in JavaScript is a closure.

## Lexical Scoping


Before ES6, there was only:

- function scope
- global scope

Variables were either function-scoped or global-scoped, depending on where they were declared.

```javascript
// old school! 💩
// x is global scope, because it's not declared in a function

if (Math.random() > 0.5) {
  var x = 1;
} else {
  var x = 2;
}
console.log(x);
```

ES6 introduced `let` and `const`.

Blocks are finally treated as scopes.

```javascript
if (Math.random() > 0.5) {
  const x = 1;
} else {
  const x = 2;
}
console.log(x); // ReferenceError: x is not defined
```

## Closure

```javascript
function makeFunc() {
  const name = 'Mozilla';

  function displayName() {
    console.log(name);
  }

  return displayName;
}

const myFunc = makeFunc();
myFunc();  // works
```

The function above is returned *before* being executed - what's returned is an object.

## Practical Closures

💡Great for event-based code

```html
<p>Some paragraph text</p>
<h1>some heading 1 text</h1>
<h2>some heading 2 text</h2>

<a href="#" id="size-12">12</a>
<a href="#" id="size-14">14</a>
<a href="#" id="size-16">16</a>
```

```javascript
function makeSizer(size) {
  return function() {
    document.body.style.fontSize = size + 'px';
  };
}

var size12 = makeSizer(12);
var size14 = makeSizer(14);
var size16 = makeSizer(16);

document.getElementById('size-12').onclick = size12;
document.getElementById('size-14').onclick = size14;
document.getElementById('size-16').onclick = size16;
```

### Module Design Pattern

Yay, [the module design pattern](https://www.google.com/search?q=javascript+module+pattern).


What is good about this crap?
- A way to implement "class variables?"
- A way to implement singletons


```javascript
// The anonymous function is executed as soon as it's defined
const counter = (function () {
  let privateCounter = 0;

  console.log("I'm an anonymous function. I was just born!"); // executes immediately

  function changeBy(val) {
    privateCounter += val;
  }

  return {
    increment() {
      changeBy(1);
    },

    decrement() {
      changeBy(-1);
    },

    value() {
      return privateCounter;
    },
  };
})();

// Note how they all have access to a single class instance -
// All share a common lexical environment
console.log(counter.value()); // 0.
counter.increment();
counter.increment();
console.log(counter.value()); // 2.
counter.decrement();
console.log(counter.value()); // 1.
```

### Independent Lexical Contexts

```javascript
const makeCounter = function (name) {
  let privateCounter = 0;

  console.log(`makeCounter: ${name} was just born`);

  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment() {
      changeBy(1);
    },

    decrement() {
      changeBy(-1);
    },

    value() {
      return `${name}: ${privateCounter}`;
    },
  };
};

const counter1 = makeCounter('Fred'); 		// makeCounter: Fred was just born
const counter2 = makeCounter('Wilma'); 		// makeCounter: Wilma was just born
console.log("We're done making things.");
console.log(counter1.value()); 						// Fred: 0

counter1.increment();
counter1.increment();
console.log(counter1.value()); 						// Fred: 2

counter1.decrement();
console.log(counter1.value()); 						// Fred: 1
console.log(counter2.value()); 						// Wilma: 0
```

### Closure Scope Chain

Closures have three scopes:

1. Local scope (own scope)
2. Enclosing scope (block, function, module scope)
3. Global scope

### Performance Considerations

Each function manages its own scope and closure.

Don't create functions within functions if closures are not needed: there are CPU/mem drawbacks.







































