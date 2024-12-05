# Frontend Quiz

This document contains general questions about frontend development. Use these as flash cards to aid your study.

## Question 1 - Explain the box model

The CSS box model applies to block boxes and defines how the various parts of a box — `margin`, `border`, `padding`, and `content` — work together to create a visual structure on a page. Inline boxes also utilize aspects of the box model, but their behavior is slightly different.

The box model is responsible for calculating:

1. How much space a block element occupies.
2. Whether borders and/or margins overlap or collapse.
3. The overall dimensions of a box.

### Parts of the Box Model

A block box in CSS consists of the following layers:

1. **Content box**: The area where your content is displayed. You can size it using properties like `width`, `height`, `inline-size`, or `block-size`.
2. **Padding box**: The padding surrounds the content, providing whitespace inside the box. Use `padding` and related properties to adjust its size.
3. **Border box**: The border wraps the content and any padding. Size it using `border` and related properties.
4. **Margin box**: The outermost layer of the box, the margin creates space between this box and adjacent elements. Control it using `margin` properties.

Below is a diagram illustrating these layers:

![Box Model Layers](https://github.com/user-attachments/assets/30ba81f8-0135-41e1-b6a6-ca544a1615e5)

### Box Model Rules

- **Dimension Calculation**: The dimensions of a block element are determined by its `width`, `height`, `padding`, and borders.
- **Height**: If no `height` is specified, a block element's height is determined by its content plus any padding (except when floats are involved).
- **Width**: If no `width` is defined, a non-floated block element will expand to fit its parent's width minus its padding.
  - Some block-level elements (e.g., `table`, `figure`, and `input`) have inherent or default widths and may not expand to fill their parent's width.
  - Inline-level elements (e.g., `span`) do not have default widths and will only take up as much space as their content requires.
- **Margins**: Margins affect the space outside the box but are not included in the box's actual dimensions. The box's size stops at the border and does not extend into the margin.
- **Default Behavior**: By default (`box-sizing: content-box`), the `width` and `height` properties only apply to the content box, excluding padding and borders.

### Box Sizing: Different Calculation Methods

1. **`content-box`** (default): The `width` and `height` properties apply only to the content box. Padding and borders are added outside these dimensions.
2. **`border-box`**: The `width` and `height` properties include the content, padding, and borders, but not the margins. This approach is often more intuitive, and many CSS frameworks (e.g., Bootstrap, and Tailwind) globally apply `* { box-sizing: border-box; }` to ensure consistent behavior across elements.

Example:

```css
.content-box {
  width: 350px;
  height: 150px;
  margin: 10px;
  padding: 25px;
  border: 5px solid black;
}

.border-box {
  box-sizing: border-box;
  width: 350px;
  inline-size: 350px;
  height: 150px;
  block-size: 150px;
  margin: 10px;
  padding: 25px;
  border: 5px solid black;
}
```

In the standard CSS box model, the actual space taken up by the box will be 410px wide (350 + 25 + 25 + 5 + 5) and 210px high (150 + 25 + 25 + 5 + 5):

![Standard CSS Box Model](https://github.com/user-attachments/assets/6e53df9d-edde-4427-b437-11bf902ae5dc)

In the alternative box model, the actual space taken up by the box will be 350px in the inline direction and 150px in the block direction.

![Alternativd Box model](https://github.com/user-attachments/assets/aa997e7d-d738-4489-beb9-828e5facaccc)

### References

1. [The Box Model | MDN](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/The_box_model#block_and_inline_boxes)

## Question 2 - What is CSS Selector Specificity and How Does It Work?

CSS selector specificity determines which styles are applied to an element when multiple rules match. The browser calculates specificity as a four-part value (`INLINE, ID, CLASS, TYPE`), with each part derived from the type of selectors used:

1. **`INLINE`**: Represents whether inline styles are used. Inline styles (e.g., `style="property: value;"`) have the highest specificity. If an inline style is present, `INLINE = 1`; otherwise, `INLINE = 0`.
2. **`ID`**: Counts the number of ID selectors.
3. **`CLASS`**: Counts the number of class selectors, such as `.myClass`, attribute selectors like `[type="radio"]`, and pseudo-classes, such as `:hover`.
4. **`TYPE`**: Counts the number of tag selectors, such as `p`, `h1`, and `td`, and pseudo-elements like `::before`, `::placeholder`, and all other selectors with double-colon notation

### Specificity Calculation

Specificity is represented as an array of values (`INLINE, ID, CLASS, TYPE`). When comparing two rules:

- Compare values column by column from left to right (`INLINE`, then `ID`, then `CLASS`, then `TYPE`).
- A higher value in a column outweighs all lower columns combined. For example:
- Specificity `0, 1, 0, 0` (1 ID selector) is higher than `0, 0, 10, 10` (10 classes and 10 tags).

Below are examples of selectors and their respective specificity values:

| Selector                | Specificity (`INLINE, ID, CLASS, TYPE`) |
|-------------------------|----------------------------|
| `div`                  | `0, 0, 0, 1`              |
| `.example`             | `0, 0, 1, 0`              |
| `#id`                  | `0, 1, 0, 0`              |
| `div .example`         | `0, 0, 1, 1`              |
| `#id .example span`    | `0, 1, 1, 1`              |
| `style="color: red;"`  | `1, 0, 0, 0`              |

### Rule Precedence with Equal Specificity

If two rules have the same specificity, the rule that appears *last* in the CSS (or the closest one, if styles are inherited) takes precedence. For example, if the same rule is declared twice in a stylesheet, the one further down will override the earlier one.

In the example below, The `#id` selector takes precedence due to its higher specificity (0, 1, 0, 0).

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <style>
    #id { color: blue; }          /* Specificity: 0, 1, 0, 0 */
    .example { color: green; }    /* Specificity: 0, 0, 1, 0 */
    div { color: red; }           /* Specificity: 0, 0, 0, 1 */
  </style>
</head>
<body>
  <div id="id" class="example">This text is blue.</div>
</body>
</html>
```

### Best Practices for Specificity

- **Use Low Specificity**: Write rules with lower specificity to make them easier to override.
  - Example: Avoid using ID selectors or deeply nested rules unless absolutely necessary.
- **Component Libraries**: When designing reusable CSS for component libraries, aim for low specificity. This ensures that library users can easily override styles without resorting to high-specificity selectors or `!important`.

### References

1. [Specificity | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)

## Question 3 - What is the CSS `display` Property and Can You Give a Few Examples of Its Use?

The CSS `display` property determines both how an element itself is displayed and the layout used for its children, such as flow layout, flexbox, or grid. 

Formally, the `display` property specifies two types:
1. **Outer Display Type**: Determines how the element participates in the document's layout (e.g., block or inline).
2. **Inner Display Type**: Defines the layout context for the element's children (e.g., flex, grid).

For example, the behavior of `display: flex` is fully described in the CSS Flexible Box Layout specification, while `display: grid` is detailed in the CSS Grid Layout specification.

### Common Values for the `display` Property

| `display`  | Description                                                                                   |
|------------------|-----------------------------------------------------------------------------------------------|
| `none`           | The element is not displayed at all. It is removed from the document flow, and its children are also hidden. |
| `block`          | The element takes up the full width of its container, stacking vertically with other elements. |
| `inline`         | The element flows inline with other content, like text, and does not start a new line.        |
| `inline-block`   | Combines inline flow with the ability to set `width` and `height`.                            |
| `flex`           | Behaves as a block-level container using the flexbox model.                                   |
| `grid`           | Behaves as a block-level container using the grid layout model.                               |
| `table`          | Behaves like a `<table>` element, with table layout applied.                                  |
| `table-row`      | Behaves like a `<tr>` element, used within a `table`.                                         |
| `table-cell`     | Behaves like a `<td>` element, used within a `table-row`.                                     |
| `list-item`      | Behaves like a `<li>` element, allowing list-specific properties like `list-style`.           |

### Notes

The default `display` value for most elements depends on their type. For example:

- `<div>`: `block`
- `<span>`: `inline`
- `<li>`: `list-item`

Changing the display value can drastically alter an element's behavior and layout.

### References

1. [CSS Display | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/display)

## Question 4 - What's the Difference Between `relative`, `fixed`, `absolute`, `sticky`, and `static` Positioning in CSS?

The `position` CSS property specifies how an element is positioned in a document. Alongside this property, the `top`, `right`, `bottom`, and `left` properties are used to determine the final position of an element.

### Categories of Positioned Elements

A "positioned" element is one whose `position` property is set to `relative`, `absolute`, `fixed`, or `sticky`. Here's an overview of the different values:

| `position` | Description |
|------------------|-----------------------------------------------------------------------------------------------|
| **`static`**     | Default value. The element is positioned according to the normal document flow. The `top`, `right`, `bottom`, `left`, and `z-index` properties do not apply. |
| **`relative`**   | The element's position is adjusted relative to itself, without affecting the layout of surrounding elements. The space the element would occupy in the normal flow remains. |
| **`absolute`**   | The element is removed from the normal document flow and is positioned relative to its closest positioned ancestor (non-static). If no ancestor is positioned, it is positioned relative to the initial containing block. |
| **`fixed`**      | The element is removed from the normal document flow and is positioned relative to the viewport. It does not move when the page is scrolled. |
| **`sticky`**     | A hybrid of relative and fixed positioning. The element is treated as `relative` until it crosses a specified scroll threshold, after which it behaves as `fixed`. |

### Notes

1. Positioning Context:
   - `absolute` and `fixed` elements are removed from the normal document flow and do not affect the layout of other elements.
   - `relative` and `sticky` elements remain part of the normal document flow but behave differently under certain conditions.
2. Z-Index:
   - Only positioned elements (non-static) can have their `z-index` property applied.
3. Sticky Limitations:
   - `position: sticky` only works if the parent container has a defined height.
   - The sticky behavior may not work in all scenarios if overflow settings or parent height restrictions are misconfigured.

### References

1. [CSS Position | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/position)

## Question 5 - What's the Difference Between `inline`, `inline-block`, and `block`?

The `display` property determines how an element is rendered in the document flow. The differences between `inline`, `inline-block`, and `block` elements are significant for layout and styling.

### Comparison Table

| Property                       | `block`                                                                                         | `inline-block`                                                                                          | `inline`                                                                                  |
|--------------------------------|-------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| **Size**                       | Fills the entire width of its parent container (default behavior).                              | Width and height depend on its content, but `width` and `height` can be explicitly set.                 | Size depends entirely on content, and `width` and `height` cannot be set.               |
| **Positioning**                | Starts on a new line and occupies the full width of its container.                              | Flows with inline content, allowing other elements beside it.                                           | Flows with inline content, allowing other elements beside it.                           |
| **Can Specify `width` and `height`** | Yes                                                                                             | Yes                                                                                                     | No. `width` and `height` will be ignored.                                               |
| **Alignment with `vertical-align`** | Not applicable. Vertically aligns based on its parent's block layout.                         | Yes. Can be aligned vertically within a line using `vertical-align`.                                    | Yes. Can be aligned vertically using `vertical-align`.                                  |
| **Margins and Paddings**       | All sides respected.                                                                             | All sides respected.                                                                                     | Horizontal margins and paddings are respected, but vertical ones depend on `line-height`.|
| **Float Behavior**             | Supports floating, allowing placement beside other elements.                                    | Supports floating, behaving like a block element while respecting width and height settings.            | Floats convert it to a block-like element where vertical paddings and margins apply.     |

### Detailed Descriptions

#### 1. **`block`**
- Block-level elements span the full width of their container by default.
- They always start on a new line.
- Examples: `<div>`, `<p>`, `<header>`, `<footer>`.

```html
<div style="display: block; background: lightblue;">
  This is a block element.
</div>
```

#### 2. **`inline`**
- Inline elements flow with surrounding text and do not start on a new line.
- Width and height cannot be set explicitly.
- Examples: `<span>`, `<a>`, `<strong>`, `<em>`.

```html
<span style="display: inline; background: lightgreen;">
  This is an inline element.
</span>
```

#### 3. **`inline-block`**
- Similar to `inline` but allows setting `width`, `height`, and vertical alignment.
- Flows with other inline elements but behaves like a block in terms of size control.
- Examples: Often used for buttons, images, or custom layout elements.

```html
<div style="display: inline-block; width: 100px; height: 50px; background: lightcoral;">
  This is an inline-block element.
</div>
```

### Notes

1. Use Cases:
   - `block`: Best for structural elements and major containers in your layout.
   - `inline`: Ideal for styling parts of text or inline-level elements like links.
   - `inline-block`: Useful for combining inline flow with block-like styling, such as buttons, custom badges, or grid layouts.
2. Vertical Margins in inline Elements:
   - Vertical padding and margins are applied visually but do not affect layout or spacing. The vertical space is influenced by `line-height`.
3. Common Misconception:
   - Floats do not inherently change the `display` property but can alter layout behavior by removing the element from the normal document flow.

### References

1. [CSS Display | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/display)

## Question 6 - Explain the Concept of "Hoisting" in JavaScript

[Hoist](https://dictionary.cambridge.org/dictionary/english/hoist) means lift something heavy with a rope or a machine. In Javascript, **Hoisting** refers to the behavior where variable and function declarations are moved to the top of their containing scope during compilation. This process allows one to use variables and functions before they are declared in the code. However, only declarations are hoisted; initializations or assignments remain in place.

### Hoisting with `var`

Variable declarations using `var` are hoisted and initialized with `undefined`.

```javascript
var foo;
console.log(foo); // undefined
foo = 1;
console.log(foo); // 1
```

### Hoisting with `let`, `const`, and `class`

These are hoisted but are not initialized. Accessing them before declaration results in a `ReferenceError` due to the [Temporal Dead Zone (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#temporal_dead_zone_tdz)

```javascript
console.log(y); // ReferenceError
let y = 'local';

console.log(z); // ReferenceError
const z = 'local';

console.log(Foo); // ReferenceError
class Foo {}
```

### Hoisting with Function Expressions

Only the variable declaration is hoisted, not the function definition.

```javascript
console.log(bar); // undefined
bar(); // Uncaught TypeError: bar is not a function

var bar = function () {
  console.log('Hello');
};
```

### Hoisting with Function Declarations

Both the declaration and definition are hoisted. You can call the function before its declaration.

```javascript
console.log(foo); // [Function: foo]
foo(); // 'Hello'

function foo() {
  console.log('Hello');
}
```

The same applies to generators (`function*`), async functions (`async function`), and async function generators (`async function*`).

### Hoisting with Import Statements

Import declarations are hoisted, and their side effects occur before the module’s code executes.

```javascript
foo.doSomething(); // Works as expected

import foo from './modules/foo';
```

### Under the hood

In reality, JavaScript creates all variables in the current scope before it even tries to executes the code. Variables created using `var` keyword will have the value of `undefined` where variables created using let and const keywords will be marked as `<value unavailable>`. Thus, accessing them will cause a `ReferenceError` preventing you to access them before initialization.

In ECMAScript specifications `let` and `const` declarations are [explained as below:](https://tc39.es/ecma262/#sec-let-and-const-declarations)

> The variables are created when their containing Environment Record is instantiated but may not be accessed in any way until the variable's LexicalBinding is evaluated.

However, this statement is a [litle bit different for the `var` keyword:](https://tc39.es/ecma262/#sec-variable-statement)

> Var variables are created when their containing Environment Record is instantiated and are initialized to undefined when created.

### Modern practices

In practice, modern code bases avoid using `var` and use `let` and `const` exclusively. It is recommended to declare and initialize your variables and import statements at the top of the containing scope/module to eliminate the mental overhead of tracking when a variable can be used.

ESLint is a static code analyzer that can find violations of such cases with the following rules:

- `no-use-before-define`: This rule will warn when it encounters a reference to an identifier that has not yet been declared.
- `no-undef`: This rule will warn when it encounters a reference to an identifier that has not yet been declared.

### Summary

| Declaration                    | Accessing before declaration |
| ------------------------------ | ---------------------------- |
| `var foo`                      | `undefined`                  |
| `let foo`                      | `ReferenceError`             |
| `const foo`                    | `ReferenceError`             |
| `class Foo`                    | `ReferenceError`             |
| `var foo = function() { ... }` | `undefined`                  |
| `function foo() { ... }`       | Normal                       |
| `import`                       | Normal                       |

### References

1. [Hoisting | MDN](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)

## Question 7 - What are the differences between JavaScript variables created using `let`, `var` or `const`?

In JavaScript, `let`, `var`, and `const` are keywords used to declare variables, but they differ significantly in terms of **scope**, **initialization**, **redeclaration**, **reassignment**, and **hoisting** behavior.

### Scope

- **`var`**: Scoped to the **function** in which it is declared or to the **global object** if declared outside any function.
- **`let` and `const`**: Scoped to the nearest **block** (e.g., function, loop, or conditional statement).

```javascript
function foo() {
  // All variables are accessible within functions.
  var bar = 1;
  let baz = 2;
  const qux = 3;

  console.log(bar); // 1
  console.log(baz); // 2
  console.log(qux); // 3
}

console.log(bar); // ReferenceError (block-scoped)
console.log(baz); // ReferenceError (block-scoped)
console.log(qux); // ReferenceError (block-scoped)
```

In the following example, `bar` is accessible outside of the `if` block but `baz` and `qux` are not.

```javascript
if (true) {
  var bar = 1;
  let baz = 2;
  const qux = 3;
}

console.log(bar); // 1 (accessible, as `var` is function/global scoped)
console.log(baz); // ReferenceError (block-scoped)
console.log(qux); // ReferenceError (block-scoped)
```

### Initilization

- `var` and `let`: Can be declared without being initialized.
- `const`: Must be initialized at the time of declaration.

```javascript
var foo; // Ok
let bar; // Ok
const baz; // SyntaxError: Missing initializer in const declaration
```

### Redeclaration

- `var`: Allows redeclaration of the same variable within the same scope.
- `let` and `const`: Do not allow redeclaration within the same scope.

```javascript
var foo = 1;
var foo = 2;
console.log(foo); // 2

let baz = 3;
let baz = 4; // Uncaught SyntaxError: Identifier 'baz' has already been declared

const qux = 5;
const qux = 6; // Uncaught SyntaxError: Identifier 'qux' has already been declared
```

### Reassignment

- `var` and `let`: Allow reassignment of values.
- `const`: Does not allow reassignment. However, if the variable holds an object, the object's properties can be modified.

```javascript
var foo = 1;
foo = 2; // OK

let bar = 3;
bar = 4; // OK

const baz = 5;
baz = 6; // TypeError: Assignment to constant variable

const obj = { key: 'value' };
obj.key = 'new value'; // OK (properties of the object can be modified)
```

### Accessing Before Declaration (Hoisting)

- `var`: Hoisted and initialized with undefined.
- `let` and `const`: Hoisted but remain uninitialized in the [Temporal Dead Zone (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#temporal_dead_zone_tdz), resulting in a `ReferenceError` if accessed before declaration.

```javascript
console.log(foo); // undefined
var foo = 'foo';

console.log(baz); // ReferenceError: can't access lexical declaration 'baz' before initialization
let baz = 'baz';

console.log(bar); // ReferenceError: can't access lexical declaration 'bar' before initialization
const bar = 'bar';
```

### Best Practices

- Use `const` by default to promote immutability.
- Use `let` if the variable needs to be reassigned.
- Avoid using `var`, as its function/global scope and hoisting behavior can lead to unexpected bugs.
- Use a transpiler like Babel to convert `let`/`const` into older syntax for compatibility with older browsers.

### Summary

| Behavior | `var` | `let` | `const` |
| --- | --- | --- | --- |
| Scope | Function or Global | Block | Block |
| Initialization | Optional | Optional | Required |
| Redeclaration | Yes | No | No |
| Reassignment | Yes | Yes | No |
| Accessing before declaration | `undefined` | `ReferenceError` | `ReferenceError` |

### References

1. [var | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)
2. [let | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
3. [const | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

## Question 8 - What is the Difference Between `==` and `===` in JavaScript?

In JavaScript, `==` and `===` are used to compare values for equality. However, their behavior differs in terms of type coercion and strictness.

| **Operator**       | **`==` (Equality)**    | **`===` (Strict Equality)** |
|---------------------|------------------------|-----------------------------|
| **Type coercion**   | Yes                   | No                          |
| **Compares value**  | Yes                   | Yes                         |
| **Compares type**   | No                    | Yes                         |


### Equality operator (`==`)

The `==` operator compares two values for equality **after performing type coercion**. Type coercion means JavaScript converts the operands to a common type before comparing them.

```javascript
42 == '42';     // true (string converted to number)
0 == false;     // true (false converted to 0)
null == undefined; // true (special case)
'' == false;    // true (empty string converted to 0)
```

However, when using `==`, unintuitive results can happen:

```javascript
1 == [1];       // true (array converted to number)
0 == '';        // true (empty string converted to 0)
0 == '0';       // true (string converted to number)
'' == '0';      // false (empty string ≠ non-empty string)
```

A common scenario for using `==` is when checking for `null` or `undefined`, as `a == null` is `true` if `a` is either `null` or `undefined`:

```javascript
let a = null;
console.log(a == null);      // true
console.log(a == undefined); // true
```

### Strict equality operator (`===`)

The `===` operator compares two values for equality without performing type coercion. It ensures that both the value and the type are the same.

```javascript
42 === '42';    // false (different types: number ≠ string)
0 === false;    // false (different types: number ≠ boolean)
null === undefined; // false (different types)
[] === false;   // false (different types: object ≠ boolean)
```

Use `===` when you want to ensure a strict comparison of both value and type. It avoids unexpected results caused by type coercion:

```javascript
// Comparison with type coercion (==)
console.log(42 == '42'); // true
console.log(0 == false); // true
console.log(null == undefined); // true

// Strict comparison without type coercion (===)
console.log(42 === '42'); // false
console.log(0 === false); // false
console.log(null === undefined); // false
```

### `Object.is()`

In addition to `==` and `===`, JavaScript offers `Object.is()` for comparing values. It is similar to `===` but handles edge cases differently:

| Comparison          | `===`   | `Object.is()` |
|---------------------|---------|---------------|
| `NaN === NaN`      | false    | true          |
| `-0 === +0`         | true    | false         |

```javascript
console.log(NaN === NaN);        // false
console.log(Object.is(NaN, NaN)); // true

console.log(-0 === +0);          // true
console.log(Object.is(-0, +0));  // false
```

### Best Practices

1. Prefer `===` over `==`:
   - The strict equality operator avoids unexpected behavior due to type coercion.
   - It makes your code more predictable and less error-prone.
2. Use `==` only for nullish checks:
   - When you need to check if a value is null or undefined:
     ```javascript
     if (value == null) {
        // Value is either `null` or `undefined`
      }
     ```
3. Enable Linting Rules:
   - Use ESLint’s `eqeqeq` rule to enforce `===` and `!==` in your code, except when comparing to `null` for nullish checks.

### References
- [Equality (==) | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality)
- [Strict equality (===) | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality)
- [Object.is() | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is)

## Question 9 - What is the event loop in JavaScript runtimes?

The **event loop** is a mechanism in JavaScript that enables asynchronous programming in a single-threaded runtime. It ensures that the JavaScript engine can handle multiple operations, including I/O tasks and user interactions, without blocking the execution of the main program.

### Key Components of the Event Loop

#### Call stack

- The **call stack** is a data structure that keeps track of the currently executing functions in the JavaScript runtime.
- Functions are added to the top of the stack when called and removed when they complete.
- JavaScript is single-threaded, so only one function can execute at a time.

#### Web APIs/Node.js APIs

- Asynchronous tasks like `setTimeout`, HTTP requests, or DOM events are offloaded to **Web APIs** (in browsers), **Node.js APIs** or any other JavaScript runtime, like [Bun](https://bun.sh/).
- These APIs run on separate threads and notify the JavaScript runtime when tasks are complete.

#### Task queue / Macrotask queue / Callback queue

- The **task queue** holds callbacks from macrotasks like:
  - `setTimeout()`, `setInterval()`
  - DOM event handlers (e.g., click, scroll)
  - MessageChannel messages
- These tasks are executed after all synchronous code and microtasks have been processed.


#### Microtasks queue

- The **microtask queue** has higher priority than the task queue and is processed before the macrotasks.
- Microtasks include:
  - Promise callbacks (`then`, `catch`, `finally`)
  - `queueMicrotask()`
  - `MutationObserver` callbacks
  - Function body execution following `await` 

### How the Event Loop Works

1. **Synchronous Code**:
   - The engine executes all synchronous code on the call stack.

2. **Handling Asynchronous Operations**:
   - When an asynchronous operation (e.g., `setTimeout`) is encountered:
     - It is sent to the corresponding Web API/Node.js API for processing.
     - Once completed, its callback is added to either the **task queue** or the **microtask queue**.

3. **Processing Tasks**:
   - When the call stack is empty:
     - The event loop first processes all callbacks in the **microtask queue**.
     - Then, it processes the first callback from the **task queue** (macrotasks).

4. **Priority Order**:
   - Microtasks are always executed **before** the next macrotask.
   - If new microtasks are added while processing a macrotask, the event loop will prioritize those microtasks.

### Example

```javascript
console.log('Start');

setTimeout(() => {
  console.log('Timeout 1');
}, 0);

Promise.resolve().then(() => {
  console.log('Promise 1');
});

setTimeout(() => {
  console.log('Timeout 2');
}, 0);

console.log('End');

// Console output:
// Start
// End
// Promise 1
// Timeout 1
// Timeout 2
```

**Explanation:**

1. `Start` and `End` are logged first because they are part of the initial synchronous code.
2. The `Promise` 1 callback (a microtask) is executed next.
3. Finally, `Timeout` 1 and `Timeout 2` (macrotasks) are executed in the order they were added.

### Summary

The event loop enables asynchronous, non-blocking execution in JavaScript. It manages the interaction between the call stack, Web APIs, and task/microtask queues, processing tasks in a predictable order:

1. Execute synchronous code.
2. Process the microtask queue.
3. Process the task queue (macrotasks). This approach ensures smooth execution of both synchronous and asynchronous operations.

### References
- [The Event Loop | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop)
- [JavaScript Visualized - Event Loop, Web APIs (Micro)task Queue | Lydia Hallie](https://www.youtube.com/watch?v=eiC58R16hb8&ab_channel=LydiaHallie)
- [In the Loop | Jake Archibald](https://www.youtube.com/watch?v=cCOL7MC4Pl0&ab_channel=JSConf)

## Question 10 - Explain event delegation in JavaScript

**Event delegation** is a JavaScript design pattern that allows you to efficiently manage events by attaching a single event listener to a common ancestor element instead of adding individual listeners to each child element. It leverages **event bubbling** to intercept events on child elements as they propagate up the DOM tree.

### How Event Delegation Works

1. **Attach a Listener to the Ancestor**:
   - Instead of attaching multiple listeners to individual child elements, you attach a single listener to a common parent or ancestor element.

2. **Event Bubbling**:
   - When an event occurs on a child element, it bubbles up the DOM hierarchy, triggering event listeners on ancestor elements.

3. **Identify the Target**:
   - Within the ancestor’s event listener, use properties like `event.target` or `event.currentTarget` to identify the actual element that triggered the event.

4. **Perform Target-Specific Actions**:
   - Based on the target element, execute the appropriate logic within the shared event handler.

### Benefits of Event Delegation

1. **Improved Performance**:
   - Reduces the number of event listeners, saving memory and improving performance for large sets of elements.

2. **Dynamic Element Handling**:
   - Automatically supports events for dynamically added child elements without needing additional listeners.

3. **Simplified Code**:
   - A single, centralized event handler makes the code easier to maintain and debug.

### Example

Here's a simple example:

```javascript
// HTML:
// <ul id="item-list">
//   <li>Item 1</li>
//   <li>Item 2</li>
//   <li>Item 3</li>
// </ul>

const itemList = document.getElementById('item-list');

itemList.addEventListener('click', (event) => {
  if (event.target.tagName === 'LI') {
    console.log(`Clicked on ${event.target.textContent}`);
  }
});
```

**How It Works:**
  - The `click` event listener is attached to the `<ul>` element.
  - When an `<li>` is clicked, the event bubbles up to the `<ul>` element, triggering its event listener.
  - The `event.target` property identifies the `<li>` that was clicked.

### Use cases

####  Dynamic Content in SPAs

```javascript
// HTML:
// <div id="button-container">
//   <button>Button 1</button>
//   <button>Button 2</button>
// </div>
// <button id="add-button">Add Button</button>

const buttonContainer = document.getElementById('button-container');
const addButton = document.getElementById('add-button');

buttonContainer.addEventListener('click', (event) => {
  if (event.target.tagName === 'BUTTON') {
    console.log(`Clicked on ${event.target.textContent}`);
  }
});

addButton.addEventListener('click', () => {
  const newButton = document.createElement('button');
  newButton.textContent = `Button ${buttonContainer.children.length + 1}`;
  buttonContainer.appendChild(newButton);
});
```

**Explanation:**
  - The `click` listener on `#button-container` handles events for both existing and newly added buttons.
  - No extra listeners are required for dynamically added buttons.

### Limitations of Event Delegation

1. **Non-Bubbling Events**:
   - Events like `focus`, `blur`, `mouseenter`, and `mouseleave` do not bubble and cannot be delegated.

2. **Target Identification:**
   - Ensure accurate checks of `event.target` to avoid unintended behavior, especially with nested elements.

4. **Overhead for Complex Logic:**
   - Centralized event handling can introduce complexity when distinguishing between multiple child elements.

### Event Delegation in Frameworks

- **React**:
  - React uses a synthetic event system with event delegation. Events are handled at the root of the React DOM tree and distributed to components efficiently.
  - This reduces memory usage by avoiding individual DOM listeners for each component.

### Summary

Event delegation optimizes event handling by leveraging event bubbling. It simplifies managing events for multiple or dynamic elements by:

- Attaching a single listener to a parent element.
- Using the event object to identify the target element.

This approach improves performance, reduces memory usage, and supports dynamic content seamlessly. However, it’s essential to account for non-bubbling events and write clear logic for target identification.

### References

- [Event Delegation | MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#event_delegation)
- [React v17.0 Release Candidate: No New Features](https://legacy.reactjs.org/blog/2020/08/10/react-v17-rc.html#changes-to-event-delegation)

## Question 11 - Explain how `this` works in JavaScript

The `this` keyword in JavaScript refers to the execution context of a function. Its behavior depends on how and where the function is invoked.

### Key Rules for `this`:

1. **Global Context**:
   - In the global scope, `this` refers to the global object (e.g., `window` in browsers or `global` in Node.js).
   - In strict mode (`'use strict';`), `this` is `undefined` in the global scope.

2. **Function Context**:
   - For a regular function, `this` refers to the global object in non-strict mode and is `undefined` in strict mode.
   - Example:
     ```javascript
     function regularFunction() {
       console.log(this); // global object or undefined in strict mode
     }
     ```

3. **Object Method**:
   - When a method is called on an object, `this` refers to the object itself.
   - Example:
     ```javascript
     const obj = {
       value: 42,
       method() {
         return this.value;
       },
     };
     console.log(obj.method()); // 42
     ```

4. **Arrow Functions**:
   - Arrow functions **do not have their own `this`**. Instead, they inherit `this` from their enclosing lexical scope.
   - Use case: Arrow functions are particularly useful for callbacks where you want `this` to retain its value from the outer context.
   - Example:
     ```javascript
     const obj = {
       value: 42,
       arrow: () => console.log(this.value),
       regular() {
         console.log(this.value);
       },
     };
     obj.arrow(); // undefined (inherited from global scope)
     obj.regular(); // 42
     ```

5. **Class Context**:
   - Within a class constructor, `this` refers to the instance of the class.
   - Arrow functions in class properties bind `this` to the instance automatically.
   - Example:
     ```javascript
     class MyClass {
       constructor(value) {
         this.value = value;
       }
       method() {
         console.log(this.value);
       }
       arrow = () => {
         console.log(this.value);
       };
     }
     const instance = new MyClass(42);
     instance.method(); // 42
     instance.arrow(); // 42
     ```
6. **Event Handlers**:
   - When using DOM event listeners, `this` typically refers to the element that fired the event. Use arrow functions or `bind` to control its value if needed.
  
### Important Considerations:

- **Explicit Binding**:
  - Methods like `.call()`, `.apply()`, and `.bind()` allow explicit control over `this`.
  - Example:
    ```javascript
    function greet() {
      console.log(this.name);
    }
    const user = { name: 'Alice' };
    greet.call(user); // 'Alice'
    ```

- **Default Binding**:
  - Without explicit binding or a containing object, `this` defaults to the global object or `undefined` in strict mode.

- **Using `this` in Closures**:
  - To avoid confusion, use `bind`, arrow functions, or store a reference to `this` (e.g., `const self = this;`) in nested functions.

### Common Errors:

1. Arrow functions as object methods (`this` refers to the outer scope, not the object).
2. Losing `this` context when passing methods as callbacks.

### References

- [this - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)
- [The Simple Rules to `this` in Javascript](https://medium.com/m/global-identity-2?redirectUrl=https%3A%2F%2Fcodeburst.io%2Fthe-simple-rules-to-this-in-javascript-35d97f31bde3)

## Question 12 - Describe the difference between a cookie, `sessionStorage` and `localStorage` in browsers

Cookies, `localStorage`, and `sessionStorage` provide methods for storing data on the client. These mechanisms are often used for client-only state, such as access tokens, themes, or personalized layouts, ensuring users have a consistent experience across tabs and sessions.

### Shared Characteristics

All three mechanisms share some fundamental features:

- **Client-side access**: Data is generally accessible and modifiable by the client, except for `HttpOnly` cookies.
- **Key-value storage**: Values are stored as strings; other data types must be serialized (e.g., using `JSON.stringify()`).
- **Data security**: None of these mechanisms encrypt data by default, making it important to avoid storing sensitive information in plaintext.

### Cookies

Cookies are the only mechanism that supports automatic transmission to the server with each HTTP request. This makes them ideal for:

- **Small pieces of server-relevant data**: Examples include session IDs, authentication tokens, and tracking preferences (e.g., GDPR consent).
- **Persistent server interaction**: Cookies' automatic expiry (`Expires`, `Max-Age`) simplifies data lifecycle management.
- **Security features**: Attributes like `HttpOnly` and `Secure` enhance protection against XSS and transmission over insecure connections.

**Limitations**: Cookies have a low capacity (~4 KB total per domain) and require manual parsing in JavaScript.

**Example**:
```javascript
// Set a cookie
document.cookie = 'user_id=12345; expires=Fri, 31 Dec 2024 23:59:59 GMT; path=/';

// Retrieve all cookies
console.log(document.cookie); // 'user_id=12345'

// Delete a cookie
document.cookie = 'user_id=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/';
```

Modern Alternative: The [Cookie Store API](https://developer.mozilla.org/en-US/docs/Web/API/Cookie_Store_API) provides a promise-based approach for managing cookies but has limited browser support as of November 2024.

### `localStorage`

Designed for persistent client-side storage, `localStorage` is ideal for long-term data that doesn’t require server interaction, such as:

- User preferences: Themes, layouts, or settings.
- Non-sensitive app data: Caching or storing application states.

**Characteristics:**

- Data persists indefinitely until explicitly deleted.
- Accessible across all tabs and windows under the same origin.

**Example:**
```javascript
// Save data to localStorage
localStorage.setItem('theme', 'dark');

// Retrieve data
console.log(localStorage.getItem('theme')); // 'dark'

// Remove a specific item
localStorage.removeItem('theme');

// Clear all data
localStorage.clear();
```

### `sessionStorage`

`sessionStorage` is tailored for temporary data storage within a single browser session. Common use cases include:

- Form data preservation: Retain form inputs during accidental page reloads.
- Session-specific state: Track temporary preferences or actions that don’t persist across tabs or sessions.

**Characteristics:**

- Data is cleared when the tab or window is closed.
- Accessible only within the originating tab or window.

**Example:**
```javascript
// Save data to sessionStorage
sessionStorage.setItem('draft', 'Hello, world!');

// Retrieve data
console.log(sessionStorage.getItem('draft')); // 'Hello, world!'

// Remove a specific item
sessionStorage.removeItem('draft');

// Clear all data
sessionStorage.clear();
```

### Additional Notes

For advanced use cases, consider exploring [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API), which offers a powerful database-like solution for structured storage but requires more complex implementation.

### Summary

All of the following mechanisms allow storing data on the client (i.e., the user's browser). `localStorage` and `sessionStorage` both implement the [Web Storage API interface](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API).

- **Cookies**: Best suited for server-client communication. They have a small storage capacity, can be persistent or session-based, and are domain-specific. Cookies are sent to the server with every HTTP request.
- **`localStorage`**: Ideal for long-term storage, as data persists even after the browser is closed. Accessible across all tabs and windows of the same origin and has the highest storage capacity of the three.
- **`sessionStorage`**: Best for temporary data within a single browsing session. Data is cleared when the tab or window is closed and is more capacity-efficient than cookies.

| Property                           | Cookies                       | `localStorage`          | `sessionStorage`         |
|------------------------------------|-------------------------------|--------------------------|--------------------------|
| **Initiator**                      | Client or server (`Set-Cookie` header) | Client                | Client                |
| **Lifespan**                       | Specified by `Expires`/`Max-Age` | Until deleted           | Until tab is closed      |
| **Persistent across browser sessions** | If expiry is set             | Yes                      | No                       |
| **Sent to server with HTTP requests** | Yes, via `Cookie` header     | No                       | No                       |
| **Total capacity (per domain)**    | ~4 KB                        | ~5 MB                   | ~5 MB                   |
| **Access**                         | Across windows and tabs       | Across windows and tabs  | Same tab                |
| **Security**                       | `HttpOnly` cookies inaccessible to JavaScript | None              | None                   |


### References

- [MDN: Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)
- [What is the difference between localStorage, sessionStorage, session and cookies?](https://stackoverflow.com/questions/19867599/what-is-the-difference-between-localstorage-sessionstorage-session-and-cookies)
- [MDN: Cookie Store API](https://developer.mozilla.org/en-US/docs/Web/API/Cookie_Store_API)

## Question 13 - Describe the difference between `<script>`, `<script async>` and `<script defer>`

`<script>` tags embed or reference JavaScript files in an HTML document. The addition of `async` and `defer` attributes optimizes how browsers load and execute scripts, minimizing performance issues like render-blocking.

### `<script>` (Default Behavior)

A plain `<script>` tag halts HTML parsing to download and execute the script immediately. This can slow down page rendering if the script is large or if there are many scripts.

- **Use Case**: For essential scripts needed to render the page (e.g., polyfills or inline scripts that initialize the UI).
- **Downside**: Blocking behavior can degrade user experience.

```html
<!doctype html>
<html>
  <head>
    <title>Regular Script Example</title>
  </head>
  <body>
    <h1>Regular Script Example</h1>
    <p>Rendering halts until the script below is executed.</p>

    <script src="critical.js"></script>

    <p>This content appears only after the script runs.</p>
  </body>
</html>
```

### `<script async>`

Scripts with the `async` attribute load asynchronously and execute as soon as they are ready, potentially before HTML parsing completes. This non-blocking behavior improves perceived page speed.

- **Use Case:** Non-essential scripts that don’t depend on other scripts, like analytics or ads.
- **Caveat**: Unpredictable execution order if multiple async scripts are included.

```html
<!doctype html>
<html>
  <head>
    <title>Async Script Example</title>
  </head>
  <body>
    <h1>Async Script Example</h1>
    <p>Content rendering continues while the script loads.</p>

    <script async src="analytics.js"></script>

    <p>This content may appear before or after the script executes.</p>
  </body>
</html>
```

### `<script defer>`

Scripts with the `defer` attribute also load asynchronously but defer execution until after the HTML is fully parsed. Deferred scripts execute in the order they appear in the document.

- **Use Case:** Scripts that depend on a fully-parsed DOM (e.g., manipulating elements).
- **Advantage:** Predictable execution order, non-blocking HTML parsing.

### Notes and Best Practices

1. **Script Dependencies:** Use `defer` for interdependent scripts; avoid `async` in such cases.
2. **Inline Scripts:** The `async` and `defer` attributes are ignored for inline `<script>` tags (those without a `src` attribute).
3. **Performance:** Use tools like Lighthouse to identify render-blocking scripts.
4. **Avoid `document.write`:** Scripts with `async` or `defer` should not use `document.write()`, as it is ignored and deprecated in modern browsers.

### Summary

The `<script>`, `<script async>`, and `<script defer>` tags load JavaScript in different ways, each suited for specific scenarios:

- **`<script>`**: Blocks HTML parsing until the script is downloaded and executed. Use for critical scripts needed before rendering.
- **`<script async>`**: Downloads scripts asynchronously and executes them immediately upon availability, potentially interrupting parsing. Best for independent scripts like analytics.
- **`<script defer>`**: Downloads scripts asynchronously but executes them only after HTML parsing is complete, in order of appearance. Ideal for DOM-dependent scripts.

### Key Differences

| Feature                   | `<script>`           | `<script async>`       | `<script defer>`        |
|---------------------------|----------------------|------------------------|-------------------------|
| **HTML Parsing**          | Blocks              | Continues             | Continues              |
| **Execution Timing**      | Immediate           | As soon as available  | After HTML is parsed    |
| **Execution Order**       | Sequential          | Unpredictable         | Sequential             |
| **DOM Dependency**        | No                  | No                    | Yes                    |

### References

- [`<script>`: The Script element | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script#defer)
- [async vs defer attributes](https://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html)

## Question 14 - What's the difference between a JavaScript variable that is: `null`, `undefined` or undeclared?

### Undeclared

An **undeclared** variable is a variable that has not been declared using `var`, `let`, or `const`. Assigning a value to such a variable makes it globally accessible. However, in strict mode, trying to assign or access an undeclared variable throws a `ReferenceError`.

#### Example

```javascript
function example() {
  x = 5; // Throws ReferenceError in strict mode
}
example();
console.log(x); // 5 (global variable outside strict mode)
```

### `undefined`

A variable is `undefined` if it has been declared but not assigned a value. Functions with no explicit return value also return `undefined`.

```javascript
let myVar;
console.log(myVar); // undefined

function noReturn() {}
console.log(noReturn()); // undefined
```

#### Checking for `undefined`

```javascript
let testVar;
console.log(testVar === undefined); // true
console.log(typeof testVar === 'undefined'); // true
```

### `null`

`null` represents the intentional absence of value. It must be explicitly assigned to a variable.

```javascript
let emptyVar = null;
console.log(emptyVar); // null
console.log(typeof emptyVar); // "object"
```

#### Checking for `null`:

```javascript
let anotherVar = null;
console.log(anotherVar === null); // true
```

### Comparing `null` and `undefined`

Use `===` (strict squality) to avoid false positives when comparing.

#### Loose Equality
```javascript
console.log(null == undefined); // true
```

#### Strict Equality
```javascript
console.log(null === undefined); // false
```

### Summary

| Trait               | `null`                              | `undefined`                        | Undeclared                           |
|---------------------|-------------------------------------|------------------------------------|--------------------------------------|
| **Meaning**         | Explicitly set to indicate no value | Declared but not assigned a value | Not declared at all                  |
| **Type**            | `object`                           | `undefined`                       | Throws `ReferenceError`              |
| **Equality Check**  | `null == undefined` → `true`       | `undefined == null` → `true`      | Throws `ReferenceError`              |

### References

- [MDN Web Docs: null](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null)
- [MDN Web Docs: undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)
- [MDN Web Docs: ReferenceError](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError)

## Question 15 - What's the difference between `.call` and `.apply` in JavaScript?

Both `.call` and `.apply` are methods of `Function.prototype` used to invoke a function with a specified `this` context. Their key difference lies in how they accept arguments:

- `.call`: Accepts arguments individually, as a comma-separated list.
- `.apply`: Accepts arguments as an array or an array-like object.

**Remember:**
- C for call and comma-separated arguments.
- A for apply and array arguments.

**Example:**

```javascript
function multiply(a, b) {
  return a * b;
}

console.log(multiply.call(null, 3, 4)); // 12
console.log(multiply.apply(null, [3, 4])); // 12
```

### Use Cases

#### Context Management

`.call` and `.apply` explicitly set the this value when invoking a function.

```javascript
const person = {
  name: 'John',
  introduce() {
    console.log(`Hi, I'm ${this.name}.`);
  },
};

const anotherPerson = { name: 'Alice' };

// Using call:
person.introduce.call(anotherPerson); // Hi, I'm Alice.

// Using apply:
person.introduce.apply(anotherPerson); // Hi, I'm Alice.
```

#### Function Borrowing

Both `.call` and `.apply` allow borrowing methods from one object for use on another.

```javascript
const numbers = [1, 2, 3];
const maxNumber = Math.max.apply(null, numbers);
console.log(maxNumber); // 3
```

#### Alternative syntax to call methods on objects

`.apply` can be used with methods like `Array.prototype.push` for merging arrays.

```javascript
const arr1 = [1, 2];
const arr2 = [3, 4];
Array.prototype.push.apply(arr1, arr2);
console.log(arr1); // [1, 2, 3, 4]
```

#### ES6+ Spread Operator

With modern JavaScript, the spread operator `(...)` can replicate `.apply` functionality for argument arrays, offering improved readability.

```javascript
function sum(a, b, c) {
  return a + b + c;
}

const numbers = [1, 2, 3];
console.log(sum(...numbers)); // 6
```

### Key Differences from `.bind`
- `.call` and `.apply`: Invoke the function immediately with a specified this.
- `.bind`: Returns a new function with a fixed this but does not invoke it immediately.

### Summary

| Trait             | `.call`                                    | `.apply`                                  |
|-------------------|--------------------------------------------|-------------------------------------------|
| **Argument Style**| Comma-separated: `.call(thisArg, arg1, arg2, ...)` | Array: `.apply(thisArg, [arg1, arg2, ...])` |
| **Use Case**      | When you know arguments individually       | When you have arguments as an array       |

### Example:
```javascript
function add(a, b) {
  return a + b;
}

console.log(add.call(null, 1, 2)); // 3
console.log(add.apply(null, [1, 2])); // 3
```

### References
- [Function.prototype.call | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
- [Function.prototype.apply | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
