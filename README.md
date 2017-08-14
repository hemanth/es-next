> stage-0 to stage-4 ECMAscript proposals.

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
  - [Method Parameter Decorators](#method-parameter-decorators)
  - [Function Expression Decorators](#function-expression-decorators)
  - [Zones](#zones)
  - [Nested import declarations](#nested-import-declarations)
- [Stage 1:](#stage-1)
  - [export * as ns from "mod"; statements](#export--as-ns-from-mod-statements)
  - [export v from "mod"; statements](#export-v-from-mod-statements)
  - [Observable](#observable)
  - [String.prototype.matchAll](#stringprototypematchall)
  - [WeakRefs](#weakrefs)
  - [Frozen Realms](#frozen-realms)
  - [Cancelable Promises](#cancelable-promises)
  - [ArrayBuffer.transfer](#arraybuffertransfer)
  - [Math Extensions](#math-extensions)
  - [`of` and `from` on collection constructors](#of-and-from-on-collection-constructors)
  - [Generator arrow functions.](#generator-arrow-functions)
  - [Date.parse fallback semantics](#dateparse-fallback-semantics)
  - [String#matchAll](#stringmatchall)
  - [Generator arrow functions (=>*)](#generator-arrow-functions-)
  - [Promise.try](#promisetry)
  - [64-Bit Integer Operations](#64-bit-integer-operations-1)
  - [`of` and `from` on collection](#of-and-from-on-collection)
  - [Dynamic Module Reform](#dynamic-module-reform)
  - [Null Propagation Operator](#null-propagation-operator)
  - [Math.signbit: IEEE-754 sign bit](#mathsignbit-ieee-754-sign-bit)
  - [Error Stacks](#error-stacks)
  - [do expressions](#do-expressions)
  - [Realms](#realms)
- [Stage 2:](#stage-2)
  - [Template Literal Revision](#template-literal-revision)
  - [function.sent Meta Property](#functionsent-meta-property)
  - [Class Property Declarations](#class-property-declarations)
  - [Public Class Fields](#public-class-fields)
  - [Class and Property Decorators](#class-and-property-decorators)
  - [String.prototype.{trimStart,trimEnd}](#stringprototypetrimstarttrimend)
  - [Promise.prototype.finally](#promiseprototypefinally)
  - [Legacy RegExp features](#legacy-regexp-features)
  - [Private Fields](#private-fields)
  - [Intl.Segmenter: Unicode segmentation](#intlsegmenter-unicode-segmentation)
- [Stage 3:](#stage-3)
  - [global](#global)
  - [Rest and Spread properties](#rest-and-spread-properties)
  - [Async-iteration](#async-iteration)
  - [Function.prototype.toString revision](#functionprototypetostring-revision)
  - [SIMD APIs](#simd-apis)
  - [Lifting Template Literal Restriction](#lifting-template-literal-restriction)
  - [Shared memory and atomics](#shared-memory-and-atomics)
  - [global](#global-1)
  - [import()](#import)
  - [RegExp Lookbehind Assertions](#regexp-lookbehind-assertions)
  - [Unicode property escapes in RE](#unicode-property-escapes-in-re)
  - [RegExp Named Capture Groups](#regexp-named-capture-groups)
  - [s (dotAll) flag for regular expressions](#s-dotall-flag-for-regular-expressions)
  - [Asynchronous Iterators](#asynchronous-iterators)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# Stage 0:

## Defensible Classes 
> Stage-0

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
> Stage-0

```js
x @ r // The object x is in the r relationship with what value?
x @ r = y; // Store that x is in the r relationship with value y.
```

## String.prototype.at
> Stage-0

```js
'abc𝌆def'.at(3)
// → '𝌆'
```

## Reflect.isCallable
> Stage-0

```js
Reflect.isCallable(argument);
```

## Reflect.isConstructor
> Stage-0

```js
Reflect.isConstructor(argument)
```

## Additional metaproperties
> Stage-0

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
> Stage-0

```js
// :: which performs this binding and method extraction.

Promise.resolve(123).then(::console.log);

```
## 64-Bit Integer Operations
> Stage-0

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
> Stage-0

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
> Stage-0

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
> Stage-0

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
## Object enumerables
> Stage-0

```js
Object.enumerableKeys(obj); // Ordered list of keys.
```

```js
Object.enumerableValues(obj); // Ordered list of Values.
```

```js
Object.enumerableEntries(obj); //Ordered list of key value pairs.
```

## Nested import declarations
> Stage-0

```js
describe("fancy feature #5", () => {
  import { strictEqual } from "assert";

  it("should work on the client", () => {
    import { check } from "./client.js";
    strictEqual(check(), "client ok");
  });

  it("should work on the client", () => {
    import { check } from "./server.js";
    strictEqual(check(), "server ok");
  });

  it("should work on both client and server", () => {
    import { check } from "./both.js";
    strictEqual(check(), "both ok");
  });
});
```

## is{Type} APIs
> Stage-0

```js
Builtin.is(Date, vm.runInNewContext('Date'));     // false


Builtin.typeOf([]);                             // 'Array'
Builtin.typeOf(new ArrayBuffer());              // 'ArrayBuffer'
Builtin.typeOf(async function foo() {}); 
// So on.

```

## Orthogonal Class Member Syntax
> Stage-0

```js
//A kitchen sink example
class Foo {
  //instance members
  own x=0, y=0;  // two data properties
  own #secret;   // a private field
                 // initial value undefined
  own *[Symbol.iterator](){yield this.#secret}
                 // a generator method
  own #callback(){}  //a private instance method  
  //class constructor members               
  static #p=new Set(), q=Foo.#p;
                // a private field and a property
                // of the class constructor                     
  static get p(){return Foo.#p} //accessor method     
  //prototype methods                
  setCallback(f){this.#callback=f}
  constructor(s){
     this.#secret = s;
  }
}
```

## Pattern Matching Syntax
> Stage-0

```js
let getLength = vector => match (vector) {
    { x, y, z }: Math.sqrt(x ** 2 + y ** 2 + z ** 2),
    { x, y }:    Math.sqrt(x ** 2 + y ** 2),
    [...]:       vector.length,
    else: {
        throw new Error("Unknown vector type");
    }
}
```

## Structured cloning and transfer
> Stage-0

```js
StructuredClone(input, transferList, targetRealm)
```

## WHATWG URL
> Stage-0

```js
const base = new URL('http://example.org/foo');
const url = new URL('bar', base);
```

# Stage 1:

## export v from "mod"; statements
> Stage-1

```js
export v, {x, y as w} from "mod";

export v, * as ns from "mod";
```

## Observable
> Stage-1

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

## String.prototype.matchAll
> Stage-1

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

## WeakRefs
> Stage-1 

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
> Stage-1

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
## Math Extensions
> Stage-1

```js
// Possible ones:
Math.map
Math.scale
Math.remap
Math.clamp
Math.constrain
Math.toDegrees(double angrad)
Math.toRadians(double angdeg)
```

## `of` and `from` on collection constructors
> Stage-1

```js
Map.of( ...items );
Map.from( ...items );

Set.of( ...items );
Set.from( ...items );

WeakMap.of( ...items );
WeakMap.from( ...items );

WeakSet.of( ...items );
WeakSet.from( ...items );
```

## Generator arrow functions.
> Stage-1

```js
let cat = *() => { yield 'meow'; }
```

## Date.parse fallback semantics
> Stage-1

```js
//  New grammar should be used as the "fallback" 
//  when date strings do not conform to the 
//  regular Date Time String Format.
```

## Generator arrow functions (=>*)	
> Stage-1

```js
// current
x => x * x;
(...) => { statements }
(...) => ( expr )

// proposed generator arrows...

// Irregular
() =*>

// Hostile to ! (async function)
() => * { ...yield... }

// Not good
() => * (yield a, yield b)

// Ok if 1 token
x *=> x * x;

// Bad (ASI)
*() => ...

// Hostile to !
(x) =* {...}
```

## Promise.try
> Stage-1

```js
// Promise.try(function() fn) -> Promise
```

## `of` and `from` on collection 
> Stage-1

```js
CollectionCreate ( C, source [ , mapfn [ , thisArg ] ] )

Map.of ( ...items )
Set.of ( ...items )
WeakMap.of ( ...items )
WeakSet.of ( ...items )

Map.from ( source [ , mapFn [ , thisArg ] ] )
Set.from ( source [ , mapFn [ , thisArg ] ] )
WeakMap.from ( source [ , mapFn [ , thisArg ] ] )
WeakSet.from ( source [ , mapFn [ , thisArg ] ] )
```
## Optional Chaining
> Stage-1

```js
obj?.prop         // optional property access
obj?.[expr]       // ditto
func?.(...args)   // optional function or method call
new C?.(...args)  // optional constructor invocation
```

## Math.signbit: IEEE-754 sign bit
> Stage-1

```js
Math.signbit(x);

/*
Returns whether the sign bit of x is set.

If n is NaN, the result is false.
If n is -0, the result is true.
If n is negative, the result is true.
Otherwise, the result is false.
*/
```

## Error Stacks
> Stage-1

```js
Error.prototype.stack;
System.getStack;
System.getStackString;


Object.getOwnPropertyDescriptor(new Error(), 'stack');
Object.getOwnPropertyDescriptor(Error.prototype, 'stack');
```

## do expressions
> Stage-1

```js
let x = do {
  let tmp = f();
  tmp * tmp + 1
};

let x = do {
  if (foo()) { f() }
  else if (bar()) { g() }
  else { h() }
};
```

## Realms
> Stage-1

```js
let realm = new Realm();

let outerGlobal = window;
let innerGlobal = realm.global;

let f = realm.evalScript("(function() { return 17 })");

f() === 17 // true

Reflect.getPrototypeOf(f) === outerGlobal.Function.prototype // false
Reflect.getPrototypeOf(f) === innerGlobal.Function.prototype // true


class EmptyRealm extends Realm {
  constructor(...args) { super(...args); }
  init() { /* do nothing */ }
}


class FakeWindow extends Realm {
  init() {
    super.init(); // install the standard primordials
    let global = this.global;

    global.document = new FakeDocument(...);
    global.alert = new Proxy(fakeAlert, { ... });
    ...
  }
}
```

# Stage 2:

## Template Literal Revision
> Stage-2 

```js
// The proposal is about fixing those Illegal token errors, avoid restrictions on escape sequences. 
let document = latex`
\newcommand{\fun}{\textbf{Fun!}}  // works just fine
\newcommand{\unicode}{\textbf{Unicode!}} // Illegal token!
\newcommand{\xerxes}{\textbf{King!}} // Illegal token!

Breve over the h goes \u{h}ere // Illegal token!
```

## function.sent Meta Property
> Stage-2

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

## Class Property Declarations
> Stage-2

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

## Public Class Fields
> Stage-2

```js
// Class Instance Fields
class ClassWithoutInits {
  myProp;
}

class ClassWithInits {
  myProp = 42;
}
```

```js
// Class Static Properties

class MyClass {
  static myStaticProp = 42;

  constructor() {
    console.log(MyClass.myStaticProp); // Prints '42'
  }
}
```

## Class and Property Decorators
> Stage-2

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

## String.prototype.{trimStart,trimEnd}
> Stage-2

```js
"    Hey JS!".trimStart(); // "Hey JS!"

"    Hey JS!    ".trimEnd();// "    Hey JS!"

// P.S: trimLeft/trimRight are aliases.
```

## Promise.prototype.finally
> Stage-2

```js
somePromise()
.then(() => {})
.catch(() => {})
.finally(() => {})
```

## Legacy RegExp features
> Stage-2

```js
RegExpAlloc( newTarget );

RegExpBuiltInExec( R, S );

RegExp.input;

RegExp.prototype.compile( pattern, flags ); // modifications
```

## Private Fields
> Stage-2

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

## Intl.Segmenter: Unicode segmentation
> Stage-2

```js
// Create a segmenter in your locale
let segmenter = Intl.Segmenter("fr", {type: "word"});

// Get an iterator over a string
let iterator = segmenter.segment("Ceci n'est pas une pipe");

// Iterate over it!
for (let {segment, breakType} of iterator) {
  console.log(`segment: ${segment} breakType: ${breakType}`);
  break;
}

// logs the following to the console:
// index: Ceci breakType: letter
```

## export * as ns from "mod"; statements
> Stage-2

```js
export * as ns from "mod";  // Exporting the ModuleNameSpace object as a named export.
```

# Stage 3:

## global
> Stage-3

```js
// global to rule them all.

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

## Rest and Spread properties
> Stage-3

```js
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 }; // Rest.


let n = { x, y, ...z }; // Spread.

```

## Async-iteration
> Stage-3

```js
asyncIterator
  .next()
  .then(({ value, done }) => /* ... */);
```

## Function.prototype.toString revision
> Stage-3

```js
// String's parse must contains the same
// function body and parameter list as the original.

O.gOPD({ get a(){} }, "a").get // "function a(){}"

O.gOPD({ set a(b){} }, "a").set // "function a(b){}"

```

## SIMD APIs
> Stage-3

```js
/*a meta-variable ranging over all SIMD types:
  Float32x4, Int32x4, Int16x8 Int8x16, Uint32x4, 
  Uint16x8, Uint8x16, Bool32x4, Bool16x8 and Bool8x16. */
```

## Lifting Template Literal Restriction
> Stage-3

```js
function tag(strs) {
  strs[0] === undefined
  strs.raw[0] === "\\unicode and \\u{55}";
}
tag`\unicode and \u{55}`

let bad = `bad escape sequence: \unicode`; // throws early error
```

## Shared memory and atomics
> Stage-3

```js
var sab = new SharedArrayBuffer(1024);  // 1KiB shared memory

w.postMessage(sab, [sab])

// In the worker:

var sab;
onmessage = function (ev) {
   sab = ev.data;  // 1KiB shared memory, the same memory as in the parent
}
```

## global
> stage 3

```js
typeof global; // object, helps in writing a portable code.
```

## import()
> stage 3

```js
import(`./language-packs/${navigator.language}.js`) // import(specifier)
```

## RegExp Lookbehind Assertions
> Stage-3

```js
const str = '1947';

// (?<=(\d+)(\d+))$/ => (947) and (1)
// Greediness proceeds from right to left


// match[1] => 947 and match[2] => 1
// Numbering capture groups

// /(?<=\1(.))/
// Referring to capture groups

// /(?<!.)/
// Negative assertions
```

## Unicode property escapes in RE
> Stage-3

```js
const regexGreekSymbol = /\p{Script=Greek}/u;
regexGreekSymbol.test('π');
```

## RegExp Named Capture Groups
> Stage-3

```js
let {one, two} = /^(?<one>.*):(?<two>.*)$/u.exec('foo:bar');
console.log(`one: ${one}, two: ${two}`);  // prints one: foo, two: bar
```

## s (dotAll) flag for regular expressions
> Stage-3

```js
const re = /foo.bar/s; // Or, `const re = new RegExp('foo.bar', 's');`.
re.test('foo\nbar');
// → true
re.dotAll
// → true
re.flags
// → 's'

/foo.bar/s.test('foo\nbar');
// → true
```

## Asynchronous Iterators
> Stage-3

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
