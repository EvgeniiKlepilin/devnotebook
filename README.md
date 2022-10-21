# devnotebook
A place where I record things that I find useful, helpful, or insightful for a software developer. Most of the notes are excerpts from already existing material on the web. This is a summarized collection that I found useful or interesting to take a note of.

## Dev Notes

### JavaScript Types
#### Number Type
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

#### BigInt Type
With BigInts, you can safely store and operate on large integers even beyond the safe integer limit (`Number.MAX_SAFE_INTEGER`) for Numbers. A BigInt is created by appending `n` to the end of an integer or by calling the `BigInt()` function.

```javascript
// BigInt
const x = BigInt(Number.MAX_SAFE_INTEGER); // 9007199254740991n
x + 1n === x + 2n; // false because 9007199254740992n and 9007199254740993n are unequal
// Number
Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2; // true because both are 9007199254740992
```

BigInt values are neither always more precise nor always less precise than numbers, since BigInts cannot represent fractional numbers, but can represent big integers more accurately. Neither type entails the other, and they are not mutually substitutable. A `TypeError` is thrown if BigInt values are mixed with regular numbers in arithmetic expressions, or if they are implicitly converted to each other.

#### Symbol type
A Symbol is a unique and immutable primitive value and may be used as the key of an Object property (see below). In some programming languages, Symbols are called "atoms". The purpose of symbols is to create unique property keys that are guaranteed not to clash with keys from other code.

### JavaScript Closures
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
