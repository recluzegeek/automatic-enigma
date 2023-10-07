# Box Model

- Devtools box model color code
  - `blue` actual content
  - `green` padding
  - `orange` margin

## Border

- Border Shorthand
  - `border: width | style | color`
  - `border-width: vertical | horizontal`
  
## Padding

- Space between actual content and the border
  - To handling fragile item in real world, we place it in a wooden box and we may place foam inside box(which is padding) and then we place the actual item. Foam makes space between the item and box's walls.
- [Taken from mdn](https://developer.mozilla.org/en-US/docs/Web/CSS/padding)
  - When one value is specified, it applies the same padding to all four sides.
  - When two values are specified, the first padding applies to the top and bottom, the second to the left and right.
  - When three values are specified, the first padding applies to the top, the second to the right and left, the third to the bottom.
- Padding Shorthand
  - `padding: top and bottom (Vertical) | left and right (Horizontal)`
  - `padding: top | right | left | bottom`

## Margin

- Space outside the element border and between other elements
  - Think of it as ruler/margin, we use to draw lines on our exam papers and we won't write anything outside that margin line
- Margin Shorthand Property
  - All four sides
    - `margin: 10px;`
  - Vertical (Top and Bottom) | Horizontal (Left and Right)
    - `margin: 5px 10px;`
  - top | right | bottom | left
    - `margin: 5px 1px 0 2px;`

## Display Property

[Link to MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/display)

- Inline Display
  - **Width** & **height** is ignored. **Margin** & **Padding** push elements away horizontally but not vertically.
- Block Display
  - Block elements break the flow of the documents. **Width**, **Height**, **Padding** & **Margins** are respected.
- Inline-Block Display
  - Behave like an inline element execpt **Width**, **Height**, **Margin** and **Padding** are respected.

## CSS Units

| Relative   | Absolute    |
|--------------- | --------------- |
| EM   | PX   |
| REM   | PT   |
| VH   | CM   |
| VW   | IN   |
| %    | MM   | 

- EM's - Set the font size relative to the parent
  - and certain html attributes such as `border-radius`, `padding`, `line-height` then all be relative to the `font-size`
  - **`<h2>`** will have font size of **60px**.

  ```html
  <article>
    <h2>I'm an h2!!!</h2>
  </article>
  ```

  ```css
    h2{
      font-size: 2ems;
    }

    article{
      font-size: 30px;
    }
  ```

- REM's - Relative to the e
  - If the root element size is 20px then 1 rem is going to be 20px and 2 rem is going to be 40px.

## Topics to Ponder

- `box-sizing: border-box`