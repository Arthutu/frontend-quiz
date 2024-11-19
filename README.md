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
