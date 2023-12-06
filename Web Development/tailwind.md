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

**Example:**

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

| Tailwind Class                  | CSS Respective Property Name |
|----------------------------------|------------------------------|
| `tracking-[2px]`                 | `letter-spacing`             |
| `text-center` / `text-right`     | `text-align`                 |
| `font-semibold` / `font-bold`    | `font-weight`                |
| `text-xl` / `text-xs`             | `font-size`                  |
| `text-[100px]` (arbitrary value) | `font-size`                  |

Font size determines the physical dimensions, while font weight influences the thickness and darkness of the characters.

#### Tailwind Arbitrary Values

When using arbitrary values in Tailwind CSS, such as `property-name-[value]` or `text-[100px]`, it allows you to apply custom measurements that may not be covered by default Tailwind classes. In the example of `text-[100px]`, it's a way to set a specific font size of 100 pixels, providing flexibility beyond the predefined sizes.

---
