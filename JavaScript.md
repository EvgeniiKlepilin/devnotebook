# JavaScript
## JavaScript Data Types and Data Structures
### Number Type
The Number type is a double-precision 64-bit binary format IEEE 754 value. It is capable of storing positive floating-point numbers between 2-1074 (`Number.MIN_VALUE`) and 21024 (`Number.MAX_VALUE`) as well as negative floating-point numbers between -2-1074 and -21024, but it can only safely store integers in the range -(253 − 1) (`Number.MIN_SAFE_INTEGER`) to 253 − 1 (`Number.MAX_SAFE_INTEGER`).

Outside this range, JavaScript can no longer safely represent integers; they will instead be represented by a double-precision floating point approximation. You can check if a number is within the range of safe integers using `Number.isSafeInteger()`.

Values outside the range ±(2-1074 to 21024) are automatically converted:
- Positive values greater than `Number.MAX_VALUE` are converted to `+Infinity`.
- Positive values smaller than `Number.MIN_VALUE` are converted to `+0`.
- Negative values smaller than `-Number.MAX_VALUE` are converted to `-Infinity`.
- Negative values greater than `-Number.MIN_VALUE` are converted to `-0`.

Slight caveat to watch out for:
```javascript
console.log(42 / +0); // Infinity
console.log(42 / -0); // -Infinity
```

`NaN` ("Not a Number") is a special kind of number value that's typically encountered when the result of an arithmetic operation cannot be expressed as a number. It is also the only value in JavaScript that is not equal to itself.

### BigInt Type
With BigInts, you can safely store and operate on large integers even beyond the safe integer limit (`Number.MAX_SAFE_INTEGER`) for Numbers. A BigInt is created by appending `n` to the end of an integer or by calling the `BigInt()` function.

```javascript
// BigInt
const x = BigInt(Number.MAX_SAFE_INTEGER); // 9007199254740991n
x + 1n === x + 2n; // false because 9007199254740992n and 9007199254740993n are unequal
// Number
Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2; // true because both are 9007199254740992
```

BigInt values are neither always more precise nor always less precise than numbers, since BigInts cannot represent fractional numbers, but can represent big integers more accurately. Neither type entails the other, and they are not mutually substitutable. A `TypeError` is thrown if BigInt values are mixed with regular numbers in arithmetic expressions, or if they are implicitly converted to each other.

### Symbol type
A Symbol is a unique and immutable primitive value and may be used as the key of an Object property (see below). In some programming languages, Symbols are called "atoms". The purpose of symbols is to create unique property keys that are guaranteed not to clash with keys from other code.

### Objects
 In JavaScript, objects are the only mutable values. **Functions** are, in fact, also objects with the additional capability of being *callable*.

 There are two types of object properties: The data property and the accessor property. Each property has corresponding attributes. Each attribute is accessed internally by the JavaScript engine, but you can set them through `Object.defineProperty()`, or read them through `Object.getOwnPropertyDescriptor()`.

**Data property**: Data properties associate a key with a value. It can be described by the following attributes:
- `value` - The value retrieved by a get access of the property. Can be any JavaScript value.
- `writable` - A boolean value indicating if the property can be changed with an assignment.
- `enumerable` - A boolean value indicating if the property can be enumerated by a `for...in` loop. See also Enumerability and ownership of properties for how enumerability interacts with other functions and syntaxes.
- `configurable` - A boolean value indicating if the property can be deleted, can be changed to an accessor property, and can have its attributes changed.

**Accessor property**: An accessor property has the following attributes:
- `get` - A function called with an empty argument list to retrieve the property value whenever a get access to the value is performed. See also getters. May be `undefined`.
- `set` - A function called with an argument that contains the assigned value. Executed whenever a specified property is attempted to be changed. See also setters. May be `undefined`.
- `enumerable` - A boolean value indicating if the property can be enumerated by a `for...in` loop. See also Enumerability and ownership of properties for how enumerability interacts with other functions and syntaxes.
- `configurable` - A boolean value indicating if the property can be deleted, can be changed to a data property, and can have its attributes changed.

Objects are ad-hoc key-value pairs, so they are often used as maps. However, there can be ergonomics, security, and performance issues. Use a `Map` for storing arbitrary data instead. The `Map` reference contains a more detailed discussion of the pros & cons between plain objects and maps for storing key-value associations.

### Keyed collections: Maps, Sets, WeakMaps, WeakSets
These data structures take object references as keys. `Set` and `WeakSet` represent a collection of unique values, while `Map` and `WeakMap` represent a collection of key-value associations.

You could implement `Map`s and `Set`s yourself. However, since objects cannot be compared (in the sense of < "less than", for instance), neither does the engine expose its hash function for objects, look-up performance would necessarily be linear. Native implementations of them (including `WeakMap`s) *can have look-up performance that is approximately logarithmic to constant time*.

Usually, to bind data to a DOM node, one could set properties directly on the object, or use `data-*` attributes. This has the downside that the data is available to any script running in the same context. `Map`s and `WeakMap`s make it easy to privately bind data to an object.

`WeakMap` and `WeakSet` only allow object keys, and the keys are allowed to be garbage collected even when they remain in the collection. They are specifically used for memory usage optimization.

## Type coercion
### Primitive coercion

The primitive coercion process is used *where a primitive value is expected, but there's no strong preference for what the actual type should be*. This is usually when a `string`, a `number`, or a `BigInt` are equally acceptable. For example:

- The `Date()` constructor, when it receives one argument that's not a `Date` instance — strings represent date strings, while numbers represent timestamps.
- The `+` operator — if one operand is a string, string concatenation is performed; otherwise, numeric addition is performed.
- The `==` operator — if one operand is a primitive while the other is an object, the object is converted to a primitive value with no preferred type.

This operation does not do any conversion if the value is already a primitive. Objects are converted to primitives by calling its `[@@toPrimitive]()` (with "default" as hint), `valueOf()`, and `toString()` methods, in that order. Note that primitive conversion calls `valueOf()` before `toString()`, which is similar to the behavior of number coercion but different from string coercion.

The `[@@toPrimitive]()` method, if present, must return a primitive — returning an object results in a `TypeError`. For `valueOf()` and `toString()`, if one returns an object, the return value is ignored and the other's return value is used instead; if neither is present, or neither returns a primitive, a `TypeError` is thrown. For example, in the following code:

```javascript
console.log({} + []); // "[object Object]"
```

Neither `{}` nor `[]` has a `[@@toPrimitive]()` method. Both `{}` and `[]` inherit `valueOf()` from `Object.prototype.valueOf`, which returns the object itself. Since the return value is an object, it is ignored. Therefore, `toString()` is called instead. `{}.toString()` returns "[object Object]", while `[].toString()` returns "", so the result is their concatenation: "[object Object]".

The `[@@toPrimitive]()` method always takes precedence when doing conversion to any primitive type. Primitive conversion generally behaves like number conversion, because `valueOf()` is called in priority; however, objects with custom `[@@toPrimitive]()` methods can choose to return any primitive. `Date` and `Symbol` objects are the only built-in objects that override the `[@@toPrimitive]()` method. `Date.prototype[@@toPrimitive]()` treats the "default" hint as if it's "string", while `Symbol.prototype[@@toPrimitive]()` ignores the hint and always returns a symbol.

### Numeric coercion

There are two numeric types: `number` and `BigInt`. Sometimes the language specifically expects a `number` or a `BigInt` (such as `Array.prototype.slice()`, where the index must be a `number`); other times, it may tolerate either and perform different operations depending on the operand's type.

Numeric coercion is nearly the same as number coercion, except that `BigInt`s are returned as-is instead of causing a `TypeError`. Numeric coercion is used by all arithmetic operators, since they are overloaded for both `number`s and `BigInt`s. The only exception is unary plus, which always does number coercion.

### Other coercions

All data types, except `Null`, `Undefined`, and `Symbol`, have their respective coercion process. As you may have noticed, there are three distinct paths through which objects may be converted to primitives:

- Primitive coercion: `[@@toPrimitive]("default")` → `valueOf()` → `toString()`
- Numeric coercion, `number` coercion, `BigInt` coercion: `[@@toPrimitive]("number")` → `valueOf()` → `toString()`
- String coercion: `[@@toPrimitive]("string")` → `toString()` → `valueOf()`

In all cases, `[@@toPrimitive]()`, if present, must be callable and return a primitive, while `valueOf` or `toString` will be ignored if they are not callable or return an object. At the end of the process, if successful, the result is guaranteed to be a primitive. The resulting primitive is then subject to further coercions depending on the context.

## Closures
Blocks don't create scopes for `var`, the `var` statements here actually create a global variable.

In essence, blocks are finally treated as scopes in ES6, but only if you declare variables with `let` or `const`.

A *closure* is the combination of a function and the lexical environment within which that function was declared. This environment consists of any local variables that were in-scope at the time the closure was created. Closures are useful because they let you associate data (the lexical environment) with a function that operates on that data. JavaScript, prior to classes, didn't have a native way of declaring private methods, but it was possible to emulate private methods using closures.

Each function instance manages its own scope and closure. Therefore, it is unwise to unnecessarily create functions within other functions if closures are not needed for a particular task, as it will negatively affect script performance both in terms of processing speed and memory consumption.

A simple example to remember the use of closures:

```javascript
function makeAdder(x) {
  return function (y) {
    return x + y;
  };
}
const add5 = makeAdder(5);
const add10 = makeAdder(10);
console.log(add5(2)); // 7
console.log(add10(2)); // 12
```

A practical example of use of closures in web development:

```javascript
function makeSizer(size) {
  return function () {
    document.body.style.fontSize = `${size}px`;
  };
}
const size12 = makeSizer(12);
const size14 = makeSizer(14);
const size16 = makeSizer(16);
```
Emulating private methods with closures:

```javascript
const counter = (function () {
  let privateCounter = 0;
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
console.log(counter.value()); // 0.
counter.increment();
counter.increment();
console.log(counter.value()); // 2.
counter.decrement();
console.log(counter.value()); // 1.
```

An example of closure over modules:

```javascript
// myModule.js
let x = 5;
export const getX = () => x;
export const setX = (val) => {
  x = val;
}
```
## References
- https://developer.mozilla.org/en-US/docs/Web/JavaScript
