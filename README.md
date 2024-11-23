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
