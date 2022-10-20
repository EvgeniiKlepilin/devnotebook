# devnotebook
A place where I record things that I find useful, helpful, or insightful for a software developer

#### Dev Notes:
- Blocks don't create scopes for `var`, the `var` statements here actually create a global variable.
- In essence, blocks are finally treated as scopes in ES6, but only if you declare variables with `let` or `const`.
- A *closure* is the combination of a function and the lexical environment within which that function was declared. This environment consists of any local variables that were in-scope at the time the closure was created. Closures are useful because they let you associate data (the lexical environment) with a function that operates on that data.
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
