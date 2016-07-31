> `stage-0` to `stage-4` features.

__TOC:__

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Stage 0:](#stage-0)
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
- [Stage 1:](#stage-1)
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
- [Stage 2:](#stage-2)
  - [Template Literal Revision](#template-literal-revision)
  - [System.global](#systemglobal)
  - [Shared memory and atomics](#shared-memory-and-atomics)
  - [Rest and Spread properties](#rest-and-spread-properties)
  - [function.sent Meta Property](#functionsent-meta-property)
  - [Asynchronous Iterators](#asynchronous-iterators)
- [Stage 3:](#stage-3)
  - [Function.prototype.toString revision](#functionprototypetostring-revision)
  - [Trailing commas in function parameter lists and calls](#trailing-commas-in-function-parameter-lists-and-calls)
  - [Async Functions](#async-functions)
  - [SIMD APIs](#simd-apis)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# Stage 0:

## Defensible Classes 
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

## Relationships
> :zero:

```js
x @ r // The object x is in the r relationship with what value?
x @ r = y; // Store that x is in the r relationship with value y.
```

## String.prototype.at
> :zero:

```js
'abc𝌆def'.at(3)
// → '𝌆'
```

## Reflect.isCallable
> :zero:

```js
Reflect.isCallable(argument);
```

## Reflect.isConstructor
> :zero:

```js
Reflect.isConstructor(argument)
```

## Additional metaproperties
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

## Function Bind Syntax
> :zero:

```js
// :: which performs this binding and method extraction.

Promise.resolve(123).then(::console.log);

```

## Do Expressions
> :zero:

```js
// do all the flexible things you can do with statements while still producing a useful result and plugging that back into an expression context.

x = do { let t = f(); t * t + 1 };

```

## 64-Bit Integer Operations
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

## Method Parameter Decorators
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

## Function Expression Decorators
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

## Zones
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

## Explicit syntactic opt-in for Tail Calls
> :zero:

```js
let factorial = (n, acc = 1) =>
  n == 1 ? acc
         : continue factorial(n - 1, acc * n);

```

## Object enumerables
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

# Stage 1:

## export * as ns from "mod"; statements
> :one:

```js
export * as ns from "mod";  // Exporting the ModuleNameSpace object as a named export.
```

## export v from "mod"; statements
> :one:

```js
export v, {x, y as w} from "mod";

export v, * as ns from "mod";
```

## Class and Property Decorators
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

## Observable
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

## String.prototype.{trimLeft,trimRight}
> :one:

```js
String.prototype.trimLeft("     Meow"); // "Meow"

String.prototype.trimRight("Meow    "); // "Meow"
```

## String.prototype.matchAll
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

## Private Fields
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

## WeakRefs
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

## Frozen Realms
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

## Cancelable Promises
> :one:

```js
new Promise((resolve, reject, cancel) => { ... })
Promise.cancel(cancelation)
promise.then(onFulfilled, onRejected, onCanceled)
promise.cancelCatch(cancelation => { ... })
```

## ArrayBuffer.transfer
> :one:

```js
//returns a new ArrayBuffer whose contents are taken from oldBuffer
var buf1 = new ArrayBuffer(40);
new Int32Array(buf1)[0] = 42;
 
var buf2 = ArrayBuffer.transfer(buf1, 80);
assert(buf1.byteLength == 0);
assert(buf2.byteLength == 80);
assert(new Int32Array(buf2)[0] == 42);
 
var buf3 = ArrayBuffer.transfer(buf2, 0);
assert(buf2.byteLength == 0);
assert(buf3.byteLength == 0);
```

## Unicode property escapes in RE
> :one:

```js
const regexGreekSymbol = /\p{Script=Greek}/u;
regexGreekSymbol.test('π');
```

## Math Extensions
> :one:

```js
// Possible ones:
Math.map
Math.scale
Math.remap
Math.clamp
Math.constrain
Math.toDegress(dobule, angrad)
Math.toRadians(double, arndeg)
```

# Stage 2:

## Template Literal Revision
> :two: 

```js
// The proposal is about fixing those Illegal token errors, avoid restrictions on escape sequences. 
let document = latex`
\newcommand{\fun}{\textbf{Fun!}}  // works just fine
\newcommand{\unicode}{\textbf{Unicode!}} // Illegal token!
\newcommand{\xerxes}{\textbf{King!}} // Illegal token!

Breve over the h goes \u{h}ere // Illegal token!
```

## System.global
> :two:

```js
// System.global to rule them all.

var getGlobal = function () {
    // the only reliable means to get the global object is
    // `Function('return this')()`
    // However, this causes CSP violations in Chrome apps.
    if (typeof self !== 'undefined') { return self; }
    if (typeof window !== 'undefined') { return window; }
    if (typeof global !== 'undefined') { return global; }
    throw new Error('unable to locate global object');
};

```

## Shared memory and atomics
> :two: 

```js
var sab = new SharedArrayBuffer(1024);  // 1KiB shared memory

w.postMessage(sab, [sab])

// In the worker:

var sab;
onmessage = function (ev) {
   sab = ev.data;  // 1KiB shared memory, the same memory as in the parent
}
```

## Rest and Spread properties
> :two:

```js
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 }; // Rest.


let n = { x, y, ...z }; // Spread.

```

## function.sent Meta Property
> :two:

```js
// Avoid ingnoring the first `next` call.
function *adder(total=0) {
   let increment=1;
   do {
       switch (request = function.sent){
          case undefined: break;
          case "done": return total;
          default: increment = Number(request);
       }
       yield total += increment;
   } while (true)
}

let tally = adder();
tally.next(0.1); // argument no longer ignored
tally.next(0.1);
tally.next(0.1);
let last=tally.next("done");
console.log(last.value);  //0.3
```

## Asynchronous Iterators
> :two:

```js
asyncIterator.next().then(result => console.log(result.value));


for await (let line of readLines(filePath)) {
    print(line);
}

async function *readLines(path) {

    let file = await fileOpen(path);

    try {

        while (!file.EOF)
            yield file.readLine();

    } finally {

        await file.close();
    }
}
```

## Class Property Declarations
> :two:

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

## Promise.prototype.finally
> :two:

```js
Promise.resolve(2).finally(() => {})

Promise.reject(3).finally(() => {})
```

# Stage 3:

## Function.prototype.toString revision
> :three:

```js
// String's parse must contains the same
// function body and parameter list as the original.

O.gOPD({ get a(){} }, "a").get // "function a(){}"

O.gOPD({ set a(b){} }, "a").set // "function a(b){}"

```

## Trailing commas in function parameter lists and calls
> :three:

```js
function foo(
        param1,
        param2,
      ) {}
```

## Async Functions
> :three:

```js
async function chainAnimationsAsync(elem, animations) {
  let ret = null;
  try {
    for(const anim of animations) {
      ret = await anim(elem);
    }
  } catch(e) { /* ignore and keep going */ }
  return ret;
}
```

## SIMD APIs
> :three:

```js
/*a meta-variable ranging over all SIMD types:
  Float32x4, Int32x4, Int16x8 Int8x16, Uint32x4, 
  Uint16x8, Uint8x16, Bool32x4, Bool16x8 and Bool8x16. */
```
