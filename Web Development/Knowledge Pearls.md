# Knowledge Pearls

## New EJS Layout &mdash; EJS-Mate

- Working with layouts using `ejs-mate` makes life pretty easier. While `partials` makes things easier but they have their shortcomes.
- Install `ejs-mate` using `npm i ejs-mate`
- Make a `layout` directory inside the `views` directory. Add the following html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EJS Mate</title>
</head>
<body>
    <%- body %>
</body>
</html>
```

- In your working file, remove everything except what is inside of `<body>` tag. And add the line at top of the file calling `layout` of ejs-mate

```js
<% layout('views/filename.js') %>
```

## SQL Clients

- These are good alternatives to **`phpmyadmin`**
  - Beekeeper Studio
  - DBeaver
  - Navicat
