# TailwindCSS

- Tailwind CSS is a CSS framework that takes a unique "utility-first" approach to styling your web projects, where you apply small, single-purpose classes directly in your HTML markup to style elements.
Instead of writing custom CSS rules, you compose styles by combining various utility classes.

## Installing Tailwind

Either install **tailwind cli** or **tailwind with vite** &mdash; using framework guidelines. I'll go through with latter one using react.

### [Install Tailwind CSS with Vite &mdash; Documentation](https://tailwindcss.com/docs/guides/vite)

Also install ***Tailwind CSS Intellisense*** from vscode extensions and *[PrettierTailwind](https://github.com/tailwindlabs/prettier-plugin-tailwindcss)* extension from github and also follow the post installation instructions.

---

### Working with Colors in Tailwind CSS 🌈

- Easily apply text color using the `text-{color_name}` prefix.
- Tailwind offers a curated color palette. Explore it [here](https://tailwindcss.com/docs/customizing-colors).

  ```html
  <p class="text-blue-500">This text is in a vibrant blue color.</p>
  ```

**Next Steps:**

1. **Color Palette:** Tailwind provides an extensive color palette. Find your desired colors in the [official documentation](https://tailwindcss.com/docs/customizing-colors).

2. **Customization:** Tailwind allows you to customize colors to match your brand. Check the [customization guide](https://tailwindcss.com/docs/customizing-colors) for more details.

3. **Background Colors:** Apply background colors using the `bg-{color_name}` prefix. Experiment with backgrounds to enhance your designs.

    ```html
    <div class="bg-green-300 p-4 rounded-md">
        <p class="text-white">This box has a pleasant green background.</p>
    </div>
    ```

Start playing with colors in Tailwind and bring your designs to life! 🚀🎨

---

### Styling Text

- Font size determines the physical dimensions, while font weight influences the thickness and darkness of the characters.

  | Tailwind Class                  | CSS Respective Property Name |
  |----------------------------------|------------------------------|
  | `tracking-[2px]`                 | `letter-spacing`             |
  | `text-center` / `text-right`     | `text-align`                 |
  | `font-semibold` / `font-bold`    | `font-weight`                |
  | `text-xl` / `text-xs`             | `font-size`                  |
  | `text-[100px]` (arbitrary value) | `font-size`                  |

#### Tailwind Arbitrary Values

When using arbitrary values in Tailwind CSS, such as `property-name-[value]` or `text-[100px]`, it allows you to apply custom measurements that may not be covered by default Tailwind classes. In the example of `text-[100px]`, it's a way to set a specific font size of 100 pixels, providing flexibility beyond the predefined sizes.

---

### Box Model: Spacing, Borders and Display

1. **Spacing Utilities:**
   - Use `space-x-4` and `space-y-4` for **space between** elements.
   - `space-x-4`: Horizontal space (left and right).
   - `space-y-4`: Vertical space (top and bottom).

      ```html
      <div class="space-x-4">
        <!-- Elements with space between horizontally -->
      </div>
      ```

2. **Margin Utilities:**
   - Use `mx-5` for a margin of 5 on both right and left.
   - `mx`: Margin on the x-axis (horizontal).

      ```html
      <div class="mx-5">
        <!-- Element with margin 5 on both right and left -->
      </div>
      ```

3. **Text Transformation:**
   - Use `uppercase` to transform text to uppercase.

      ```html
      <p class="uppercase">This text is in uppercase.</p>
      ```

4. **Border Properties:**
   - Tailwind provides various border utilities to add borders to elements.

      ```html
      <div class="border border-solid border-black">
        <!-- Element with a solid black border -->
      </div>
      ```

5. **Display Property:**
   - Tailwind offers display utilities such as `hidden`, `block`, `inline`, and more.

      ```html
      <div class="hidden">
        <!-- Element with display: none -->
      </div>

      <span class="inline-block">
        <!-- Element with display: inline-block -->
      </span>
      ```

These Tailwind CSS utilities provide a quick and easy way to manipulate the box model and enhance the layout and styling of your elements! 🎨🚀

### Responsive Design

Responsive design is a core concept in Tailwind CSS, allowing you to create websites that adapt to different screen sizes.

1. **Breakpoints:**
   - Tailwind comes with five default breakpoints: `sm`, `md`, `lg`, `xl`, and `2xl`.
   - These breakpoints are based on media queries and represent small to extra-large screen sizes.

2. **Mobile-First Approach:**
   - Tailwind follows a mobile-first approach, meaning styles without breakpoints apply to all screen sizes by default.
     - What this means is that unprefixed utilities (like uppercase) take effect on all screen sizes, while prefixed utilities (like md:uppercase) only take effect at the specified breakpoint and above.
   - For example, `my-4` sets margin-y (top and bottom) to 4 on all screen sizes.

3. **Responsive Classes:**
   - Use breakpoints as prefixes to Tailwind classes to apply styles at specific screen sizes.
   - Example: `sm:my-4` sets margin-y to 4, starting from the small screen size and up.

4. **Understanding Breakpoints:**
   - `sm:my-4` starts applying from the small screen size and continues to larger screen sizes.
   - It does not mean it works on a viewport less than 640px; it begins applying when the viewport is greater than or equal to 640px.
   - The default minimum screen width for the `sm` breakpoint is 640px.

5. **Mobile-First Development:**
   - It's recommended to build websites with a mobile-first approach, focusing on small screens initially.
   - Use media queries and responsive classes to enhance the design for larger screens.
  
      ```html
      <!-- Width of 16 by default, 32 on medium screens, and 48 on large screens -->
      <img class="w-16 md:w-32 lg:w-48" src="...">
      ```

Start with a mobile-first mindset, and use responsive classes to tailor the design for larger screens as needed. 🌐📱

Certainly! Here's a guide on using Flexbox with Tailwind CSS:

---

### Using Flexbox with Tailwind CSS

#### 1. *Display Flex

- Use `flex` to enable the Flexbox container on an element.

    ```html
    <div class="flex">
      <!-- Child elements will be arranged in a row by default -->
    </div>
    ```

#### 2. Flex Direction

- Control the direction of the flex container using `flex-row`, `flex-col`, `flex-row-reverse`, or `flex-col-reverse`.

    ```html
    <div class="flex flex-col">
      <!-- Child elements will be arranged in a column -->
    </div>
    ```

#### 3. Flex Wrap

- Use `flex-wrap` to allow flex items to wrap onto multiple lines.

    ```html
    <div class="flex flex-wrap">
      <!-- Child elements will wrap onto the next line when needed -->
    </div>
    ```

#### 4. Justify Content &mdash; `justify-between`

- Control the alignment of flex items along the main axis using `justify-start`, `justify-end`, `justify-center`, `justify-between`, and `justify-around`.

    ```html
    <div class="flex justify-center">
      <!-- Child elements will be centered along the main axis -->
    </div>
    ```

#### 5. Align Items &mdash; `items-center`

- Align flex items along the cross axis using `items-start`, `items-end`, `items-center`, `items-baseline`, and `items-stretch`.

    ```html
    <div class="flex items-end">
      <!-- Child elements will be aligned to the bottom of the container -->
    </div>
    ```

#### 6. Align Content

- Control the alignment of wrapped lines in a multi-line flex container using `content-start`, `content-end`, `content-center`, `content-between`, and `content-around`.

    ```html
    <div class="flex flex-wrap content-between">
      <!-- Lines of wrapped items will be spaced evenly -->
    </div>
    ```

#### 7. Responsive Flexbox

- Apply Flexbox utilities responsively using breakpoints (`sm`, `md`, `lg`, `xl`, `2xl`) as prefixes.

    ```html
    <div class="flex flex-col lg:flex-row">
      <!-- On large screens and above, child elements will be arranged in a row -->
    </div>
    ```
