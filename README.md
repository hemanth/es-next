ECMAScript 7 is the next evolution of the ECMA-262 standard, this is at a very early stage, you may want to checkout [paws-on-es6](https://github.com/hemanth/paws-on-es6) as well.

__List of ES7 features:__

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Exponentiation Operator](#exponentiation-operator)
- [Array comprehensions](#array-comprehensions)
- [Generator comprehensions](#generator-comprehensions)
- [Async functions](#async-functions)
- [Async generators](#async-generators)
- [Object Observe](#object-observe)
- [Object.getOwnPropertyDescriptors](#objectgetownpropertydescriptors)
- [Array.prototype.includes](#arrayprototypeincludes)
- [Typed Objects](#typed-objects)
- [Trailing commas in function syntax.](#trailing-commas-in-function-syntax)
- [Class decorators.](#class-decorators)
- [Class properties](#class-properties)
- [Map.prototype.toJSON](#mapprototypetojson)
- [Set.prototype.toJSON](#setprototypetojson)
- [String.prototype.at](#stringprototypeat)
- [Object rest properties](#object-rest-properties)
- [Object spread properties](#object-spread-properties)
- [String.prototype.lpad](#stringprototypelpad)
- [String.prototype.rpad](#stringprototyperpad)
- [Bind Operator](#bind-operator)
- [Reflect.Realm](#reflectrealm)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# Exponentiation Operator
> Performs exponential calculation on operands. Same algorithm as `Math.pow(x, y)`

> Stage: Draft.

```js

let cubed = x => x ** 3;

cubed(2) // 8;
```

# Array comprehensions
> Declarative form for creating computed arrays with a literal syntax that reads naturally.

> Stage: Strawman.

```js

let numbers = [ 1, 4, 9 ];

[for (num of numbers) Math.sqrt(num)];
// => [ 1, 2, 3 ]

[for (x of [ 1, 2, 3]) for (y of [ 3, 2, 1 ]) x*y];
// => [ 3, 2, 1, 6, 4, 2, 9, 6, 3 ]

[for (x of [ 1, 2, 3, 4, 5, 6 ]) if (x%2 === 0) x];
// => [ 2, 4, 6 ]
```

# Generator comprehensions
> Create a generator function based on an existing iterable object.

> Stage: Strawman.

```js
(for (i of [ 2, 4, 6 ]) i*i );
// generator function which yields 4, 16, and 36
```



# Async functions
> Deferred Functions

> Stage: Proposal.

```js
function wait(t){
  return new Promise((r) => setTimeout(r, t));
}

async function asyncMania(){
  console.log("1");
  await wait(1000);
  console.log("2");
}

asyncMania()
.then(() => alert("3"));

// logs: 1 2 3
```

# Async generators
> Deferred generators.

> Stage: Proposal.

```js
// provider
async function* nums() {
  yield 1;
  yield 2;
  yield 3;
}

// consumer
async function printData() {
  for(var x on nums()) {
    console.log(x);
  }
}
```

# Object Observe
> Asynchronously observing the changes to an object.

> Stage: Draft.

```js

var obj = {};

Object.observe( obj,function(changes) {console.log(changes);} );

obj.name = "hemanth";

// Would log -> [ { type: 'new', object: { name: 'hemanth' }, name: 'name' } ]
```

# Object.getOwnPropertyDescriptors
> Returns a property descriptor for an own property.

> Stage: Strawman.

```js
// Creating a shallow copy.
var shallowCopy = Object.create(
  Object.getPrototypeOf(originalObject),
  Object.getOwnPropertyDescriptors(originalObject)
);
```

# Array.prototype.includes
> Determines whether an array includes a certain element or not.

> Stage: Draft.

```js
[1, 2, 3].includes(3, 0, 7); // true
[1, 2, NaN].includes(NaN); // true
[0,+1,-1].includes(42); // false
```

# Typed Objects
> Portable, memory-safe, efficient, and structured access to contiguously allocated data.

> Stage: Proposal.

```js
var Point = new StructType({
  x: int32,
  y: int32
});

var point = new Point({
  x: 42,
  y: 420
});
```

# Trailing commas in function syntax.
> Trailing commas in parameter and argument lists.

> Stage: Proposal

```js
var meow = function (cat1, cat2,) {}

Math.max(4,2,0,);
```

# Class decorators.
> Annotate and modify classes and properties at design time.

> Stage: Proposal.

```js
class Person {
  @cantEnum
  get kidCount() { return this.children.length; }
}

function cantEnum(target, name, descriptor) {
  descriptor.enumerable = false;
  return descriptor;
}
```

# Class properties
> Properties of class.

> Stage: Strawman.

```js
class Cat {
  name = 'Garfield';
  static says = 'meow';
}
new Cat().name; // Garfield
Cat.says; // meow
```

# Map.prototype.toJSON
> `toJSON` for Maps.

> Stage: Strawman.

```js
var myMap = new Map();
myMap.set(NaN, "not a number");

console.log(myMap.toJSON()); // {"NaN":"not a number"}
```

# Set.prototype.toJSON
> `toJSON` for Sets.

> Stage: Strawman.

```js
var mySet = new Set();
mySet.add(NaN);
mySet.add(1);
console.log(mySet.toJSON()) // {"1":1,"NaN":null}
```
# String.prototype.at
> String containing the code point at the given position

> Stage: Strawman.

```js
'abc\uD834\uDF06def'.at(1) // 'b'
'abc\uD834\uDF06def'.at(3) // '\uD834\uDF06'
```

# Object rest properties
> Rest properties for object destructuring assignment.

> Stage: Strawman.

```js
let { fname, lname, ...rest } = { fname: "Hemanth", lname: "HM", location: "Earth", type: "Human" };
fname; //"Hemanth"
lname; //"HM"
rest; // {location: "Earth", type: "Human"}
```

# Object spread properties
> Spread properties for object destructuring assignment.

> Stage: Strawman.

```js
let info = {fname, lname, ...rest};

info; // { fname: "Hemanth", lname: "HM", location: "Earth", type: "Human" }

```

# String.prototype.lpad
> left justify and pad strings.

> Stage: Strawaman.

```js
"hello".lpad(4)            #=> "hello"
"hello".lpad(20)           #=> "hello               "
"hello".lpad(20, '1234')   #=> "hello123412341234123"
```

# String.prototype.rpad
> right justify and pad strings.

> Stage: Strawaman.

```js
"hello".rpad(4)            #=> "hello"
"hello".rpad(20)           #=> "               hello"
"hello".rpad(20, '1234')   #=> "123412341234123hello"
```


# Bind Operator
> `::` Function binding and method extraction

> Stage: Strawan.

```js
let log = (level, msg) => ::console[level](msg);

import { map, takeWhile, forEach } from "iterlib";

getPlayers()
  ::map(x => x.character())
  ::takeWhile(x => x.strength > 100)
  ::forEach(x => console.log(x));
```

# Reflect.Realm
> TDB
