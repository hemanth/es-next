<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Relationshiops](#relationshiops)
- [String.prototype.at](#stringprototypeat)
- [Reflect.isCallable](#reflectiscallable)
- [Reflect.isConstructor](#reflectisconstructor)
- [Additional metaproperties](#additional-metaproperties)
- [Function Bind Syntax](#function-bind-syntax)
- [Do Expressions](#do-expressions)
- [64-Bit Integer Operations](#64-bit-integer-operations)
- [Method Parameter Decorators](#method-parameter-decorators)
- [Function Expression Decorators](#function-expression-decorators)
- [Zones](#zones)
- [Explicit syntactic opt-in for Tail Calls](#explicit-syntactic-opt-in-for-tail-calls)
- [Object enumerables](#object-enumerables)
- [Unicode property escapes in RE](#unicode-property-escapes-in-re)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

> `stage-0` and `stage-4` features.


__STAGE 0:__


#Defensible Classes

```js
// const class

const class Point { 
  constructor(x, y) {
    public getX() { return x; }
    public getY() { return y; }
  }
  toString() { 
    return `<${this.getX()}, ${this.getY()}>`;
  }
}
```

# Relationshiops

```js
x @ r // The object x is in the r relationship with what value?
x @ r = y; // Store that x is in the r relationship with value y.
```

# String.prototype.at

```js
'abc\uD834\uDF06def'.at(3), '\uD834\uDF06'
```

# Reflect.isCallable

```js
Reflect.isCallable(argument);
```

# Reflect.isConstructor

```js
Reflect.isConstructor(argument)
```

# Additional metaproperties

```js
function.callee; // function object that is currently being evaluated by the running execution context.
```

```js
function.count; // number of arguments pass to the function. 
```

```js
function.arguments; // array containing the actual arguments passed to the function.
```

# Function Bind Syntax

```js
// :: which performs this binding and method extraction.

Promise.resolve(123).then(::console.log);

```

# Do Expressions

```js
// do all the flexible things you can do with statements while still producing a useful result and plugging that back into an expression context.

x = do { let t = f(); t * t + 1 };

```

# 64-Bit Integer Operations

```js
// return the high 32 bit part of the 64 bit addition of (hi0, lo0) and (hi1, lo1)
Math.iaddh(lo0, hi0, lo1, hi1);

// return the high 32 bit part of the 64 bit subtraction of (hi0, lo0) and (hi1, lo1)
Math.isubh(lo0, hi0, lo1, hi1);

// return the high 32 bit part of the signed 64 bit product of the 32 bit numbers a and b
Math.imulh(a, b);

// return the high 32 bit part of the unsigned 64 bit product of the 32 bit numbers a and b
Math.umulh(a, b);

```

# Method Parameter Decorators

//decorators that operate on method and constructor parameters.

```js
class MyComponent {
  refresh(@lastRefreshTime timeStamp) { … }
}

export function lastRefreshTime(...) {
  // at minimum, the arguments of this function should contain:
  // - reference to owner of the parameter (the method)
  // - parameter index
  // - parameter name
  // - is parameter a rest parameter?

  // store parameter metadata using the same storage mechanism
  // as the one used for methods
}
```

# Function Expression Decorators

```js
scheduleForFrequentReexecution(@memoize function(value) { 
  value++
});

export function memoize(...) {
  // at minimum, the arguments of this function should contain:
  // - reference to the decorated function expression
  // - arguments passed into the memoize function (if any)

  // wrap the decorated function expression memoization implementation and return it
}

```

# Zones

```js
//a primitive for context propagation across multiple logically-connected async operations

class Zone {
  constructor({ name, parent });

  name;
  get parent();

  fork({ name });
  run(callback);
  wrap(callback);

  static get current();
}

const loadZone = Zone.current.fork({ name: "loading zone" });
window.onload = loadZone.wrap(e => { ... });

```

# Explicit syntactic opt-in for Tail Calls

```js
let factorial = (n, acc = 1) =>
  n == 1 ? acc
         : continue factorial(n - 1, acc * n);

```

# Object enumerables

```js
Object.enumerableKeys(obj); // Ordered list of keys.
```

```js
Object.enumerableValues(obj); // Ordered list of Values.
```

```js
Object.enumerableEntries(obj); //Ordered list of key value pairs.
```

# Unicode property escapes in RE

```js
const regexGreekSymbol = /\p{Script=Greek}/u;
regexGreekSymbol.test('π');
```
