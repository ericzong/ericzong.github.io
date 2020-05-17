<p align=center><a href=https://nelabs.dev><img src=https://nelabs.dev/logo.svg width=450 alt=nelabs.dev></a></p>

# A case for using `void` in modern JavaScript

The [`void` operator] has historically had a few use cases:
 - Used with `<a href="javascript:void(0)">` to implement dynamic buttons, but this is a poor and dated practice 
 - Used to access the primitive `undefined` value in the bad old days prior to ES5 when it was possible to overwrite the identifier `undefined`
 - Used to save a few characters and shorten `undefined` to just `void 0`; this is still used by minifiers

None of these use cases are particularly relevant to modern JavaScript, except for a new case that's been introduced with arrow functions in ES6:

## Using `void` to explicate non-value-returning (void) arrow functions

ES6 arrow function expressions allow omitting the braces from the function body to make code more terse, but it creates an ambiguity about when the return value is or isn't intended to be used, because the function might be intended just for side effects:
```js
// omitting braces
const id = x => x;

// function called just for side effects
const log = x => console.log(x);
```
The Console API is widely familiar, so this particular example isn't ambiguous, but it illustrates the principle.

Intentional void arrow functions can be made explicit by adding braces, but this makes them less terse:
```js
const log = x => {
  console.log(x);
}
```
The function could still be formatted in a single line, but automatic formatting tools like Prettier will usually add line breaks, and the intention to not return a value is not as clear as when using the `void` operator:
```js
const log = x => void console.log(x);
```
The most noticeable benefit of using `void` like this is *reducing the line count*; especially in programming styles like CPS that rely on void callbacks often.

### Type systems and [void type]

Many type systems, including TypeScript's, have the concept of a void type, which makes the use of the `void` operator for non-value-returning functions complementary and more recognizable.

### React [`useEffect`] hook

A common practical use case for `void` is with the `useEffect` hook in React:
```js
useEffect(() => void (document.title = 'example'));
```
It's a particularly salient example because the `useEffect` hook can use its callback's return value for cleaning up the effect, so accidentally returning a function could be a runtime bug.

## Using `void` with async IIFES

There's an additional but less important use case for the `void` operator with async immediately-invoked function expressions (IIFES). Traditionally IIFES were used to implement modular scopes, since JS didn't have block scope or actual modules, but modern JS has both, so IIFES are largely not relevant except for one case: using the`await` or `for-await-of` syntax at [the top level of modules][await]:
```js
void (async () => {
  await fetch(something);
})();
```
This is a popular enough use case that there exists a stage 3 proposal for it. Parenthesis are still required around the function expression due to precedence rules, but using `void` solves the problem that leading parenthesis don't work with ASI (more specifically, it breaks without explicit semicolon line terminators).

## Style guides and [`no-void`]

Popular style guides like Airbnb's and Standard JS use the `no-void` ESLint rule to prohibit using  the `void` operator, but it's arguably an oversight and should be overridden by users.

 - [Airbnb issue about removing `no-void`][airbnb]
 - [Standard JS issue about removing `no-void`][standard]
 - [ESLint `no-void` rule change request][eslint]

[void type]: https://en.wikipedia.org/wiki/Void_type
[`void` operator]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/void
[await]: https://2ality.com/2019/12/top-level-await.html
[`no-void`]: https://eslint.org/docs/rules/no-void
[airbnb]: https://github.com/airbnb/javascript/issues/2145
[standard]: https://github.com/standard/standard/issues/1464
[eslint]: https://github.com/eslint/eslint/issues/12688
[`useEffect`]: https://reactjs.org/docs/hooks-effect.html
