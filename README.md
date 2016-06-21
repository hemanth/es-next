> `stage-0` and `stage-4` features.

__TOC:__

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Defensible Classes](#defensible-classes)
- [Relationships](#relationships)
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
- [export * as ns from "mod"; statements](#export--as-ns-from-mod-statements)
- [export v from "mod"; statements](#export-v-from-mod-statements)
- [Class and Property Decorators](#class-and-property-decorators)
- [Observable](#observable)
- [String.prototype.{trimLeft,trimRight}](#stringprototypetrimlefttrimright)
- [Class Property Declarations](#class-property-declarations)
- [String.prototype.matchAll](#stringprototypematchall)
- [Private Fields](#private-fields)
- [WeakRefs](#weakrefs)
- [Frozen Realms](#frozen-realms)
- [Cancelable Promises](#cancelable-promises)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


__Stage 0:__

# Defensible Classes 
> :zero:

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

# Relationships
> :zero:

```js
x @ r // The object x is in the r relationship with what value?
x @ r = y; // Store that x is in the r relationship with value y.
```

# String.prototype.at
> :zero:

```js
'abc\uD834\uDF06def'.at(3), '\uD834\uDF06'
```

# Reflect.isCallable
> :zero:

```js
Reflect.isCallable(argument);
```

# Reflect.isConstructor
> :zero:

```js
Reflect.isConstructor(argument)
```

# Additional metaproperties
> :zero:

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
> :zero:

```js
// :: which performs this binding and method extraction.

Promise.resolve(123).then(::console.log);

```

# Do Expressions
> :zero:

```js
// do all the flexible things you can do with statements while still producing a useful result and plugging that back into an expression context.

x = do { let t = f(); t * t + 1 };

```

# 64-Bit Integer Operations
> :zero:

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
> :zero:

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
> :zero:

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
> :zero:

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
> :zero:

```js
let factorial = (n, acc = 1) =>
  n == 1 ? acc
         : continue factorial(n - 1, acc * n);

```

# Object enumerables
> :zero:

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
> :zero:

```js
const regexGreekSymbol = /\p{Script=Greek}/u;
regexGreekSymbol.test('π');
```

__Stage 1:__

# export * as ns from "mod"; statements
> :one:

```js
export * as ns from "mod";  // Exporting the ModuleNameSpace object as a named export.
```

# export v from "mod"; statements
> :one:

```js
export v, {x, y as w} from "mod";

export v, * as ns from "mod";
```

# Class and Property Decorators
> :one:

```js
class C {
  @writable(false)
  method() { }
}

function writable(value) {
  return function (target, key, descriptor) {
     descriptor.writable = value;
     return descriptor;
  }
}
```

# Observable
> :one:

```js
// Observable as a Constructor:
function listen(element, eventName) {
    return new Observable(observer => {
        // Create an event handler which sends data to the sink
        let handler = event => observer.next(event);

        // Attach the event handler
        element.addEventListener(eventName, handler, true);

        // Return a function which will cancel the event stream
        return () => {
            // Detach the event handler from the element
            element.removeEventListener(eventName, handler, true);
        };
    });
}

// Observable.of creates an Observable of the values provided as arguments
Observable.of("R", "G", "B").subscribe({
    next(color) {
        console.log(color);
    }
});

// Observable.from converts its argument to an Observable.
Observable.from(["R", "G", "B"]).subscribe({
    next(color) {
        console.log(color);
    }
});
```

# String.prototype.{trimLeft,trimRight}
> :one:

```js
String.prototype.trimLeft("     Meow"); // "Meow"

String.prototype.trimRight("Meow    "); // "Meow"
```

# Class Property Declarations
> :one:

```js

// Class instance field
class MyClass {
  myProp = 42;

  constructor() {
    console.log(this.myProp); // Prints '42'
  }
}


// Static property
class MyClass {
  static myStaticProp = 42;

  constructor() {
    console.log(MyClass.myStaticProp); // Prints '42'
  }
}
```

# String.prototype.matchAll
> :one:

```js
var str = 'Hello world!!!';
var regexp = /(\w+)\W*/g;
console.log(str.matchAll(regexp));

/*
[
  {
    0: "Hello ",
    1: "Hello"
    index: 0,
    input: "Hello world!!!"
  },
  {
    0: "world!!!",
    1: "world"
    index: 6,
    input: "Hello world!!!"
  }
]
*/

```

# Private Fields
> :one:

```js
class Point {
    #x = 0;
    #y = 0;

    constructor() {
        this.#x; // 0
        this.#y; // 0
    }
}
```

# WeakRefs
> :one: 

```js
// Make a new weak reference.
// The target is a strong pointer to the object that will be pointed
// at weakly by the result.
// The executor is an optional argument that will be invoked after the
// target becomes unreachable.
// The holdings is an optional argument that will be provided to the
// executor when it is invoked for target.
makeWeakRef(target, executor, holdings);
```

# Frozen Realms
> :one:

```js
class Realm {
  // From the prior Realm API proposal
  const global -> object                // access this realm's global object
  eval(stringable) -> any               // do an indirect eval in this realm

  // We expect the rest of earlier proposal to be re-proposed eventually in
  // some form, but do not rely here on any of the remainder.

  // New with this proposal
  static immutableRoot() -> Realm       // transitively immutable realm
  spawn(endowments) -> Realm            // lightweight child realm
}
```

# Cancelable Promises
> :one:

```js
new Promise((resolve, reject, cancel) => { ... })
Promise.cancel(cancelation)
promise.then(onFulfilled, onRejected, onCanceled)
promise.cancelCatch(cancelation => { ... })
```
