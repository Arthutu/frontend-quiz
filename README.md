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

## Question 3 - What is the CSS `display` property and can you give a few examples of its use?

The CSS `display` property determines both how an element itself is displayed and the layout used for its children, such as flow layout, flexbox, or grid. 

Formally, the `display` property specifies two types:
1. **Outer Display Type**: Determines how the element participates in the document's layout (e.g., block or inline).
2. **Inner Display Type**: Defines the layout context for the element's children (e.g., flex, grid).

For example, the behavior of `display: flex` is fully described in the CSS Flexible Box Layout specification, while `display: grid` is detailed in the CSS Grid Layout specification.

### Common Values for the `display` Property

| `display` Value  | Description                                                                                   |
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
