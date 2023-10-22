---
title: express_creating_dynamic_html_with_templating
updated: 2023-10-18 21:14:22Z
---

## Templating

- Templating allows you to define preset patterns for web pages and dynamically modify them.
  - Instead of writing static HTML, we can embed information and logic so we can repeat parts of template over and over again.
- It's useful for embedding information and logic into web pages, enabling the creation of dynamic content.
- Templating engines like EJS, Handlebars, Jade (now called Pug), and Nunjucks are commonly used with Express for generating dynamic HTML.

### EJS - Embedded Javascript Templating

- EJS is one of the templating engines that can be used with Express.
- Install `ejs` using `npm i ejs`.
  - We don't need to `require ejs`. Express behind the scene will `require ejs`
- In Express, we need to set the templating engine to EJS using `app.set('view engine', 'ejs')`.
  - `app.set('view engine', 'ejs')` accepts two arguments, one the property (key) we want to set and the corresponding value
  - By default, Express looks for views (templates) in a directory named "views," but you can change this directory using `app.set('views', '/newViews')`.
- EJS files must have a `.ejs` file extension and should be placed inside the specified views directory.
- In `*.ejs` files, we can write Standard HTML with embedded Javascript.
  - To send an EJS file as a response, we use `res.render('filename')` instead of `res.send('some response')`.
    - `res.render()` accepts an `ejs` file, while `res.send()` accepts a string.
      - `res.render('home')` to render home.ejs
      - We don't need to set the relative path to `home.ejs`. Default place express looks for `ejs` has been defined by us.
- When rendering the template, EJS evaluates any JavaScript code within the template and generates the corresponding HTML.

#### Setting the Views Directory

- Express, by default, sets the views directory to `process.cwd() + '/views'`. This works well when running the Express app from the project's root directory.
- However, if we switch to a different directory and attempt to run the application, Express will raise an error, stating that the `/views` directory cannot be found. To mitigate this, it's advisable to run the application from the same directory as the application file. You can achieve this using the `path.join()` method, combining `__dirname` with `'/views'` or another path that suits your project structure.
  - `__dirname` returns the directory name of the current module, providing the absolute path to the directory containing the JavaScript file where it's used.
    - It's commonly used to reference files or set paths relative to the current module's location in Node.js applications.

  ```js
  const express = require('express');
  const app = express();  // express instance
  const path = require('path');
  const port = 3000;

  // Set the view engine and views directory
  app.set('view engine', 'ejs');
  app.set('views', path.join(__dirname, 'views'));

  app.get('/', (req, res) => {
    res.render('home');
  });

  app.listen(port, () => {
    console.log(`listening on port ${port}`);
  });
  ```

### EJS Interpolation Syntax

| Syntax                | Example                          | Functionality                               |
|-----------------------|----------------------------------|---------------------------------------------|
| `<% code %>`          | `<% if (loggedIn) { %> ... <% } %>` | Embeds JavaScript code for control flow and logic within the template. No output is produced. |
| `<%= variable %>`     | `<p>Hello, <%= username %>!</p>` | Embeds the value of a JavaScript variable or expression into the HTML output. Used for dynamic data insertion. |
| `<%- code %>`           | `<p><%- htmlContent %></p>`           | Embeds raw HTML or unescaped content into the HTML output. |
| `<%# comment %>`        | `<%# This is a comment %>`            | Adds comments within the EJS template (comments are not rendered in the final output). |
| `<% include(filename) %>`| `<% include('header') %>`            | Includes another EJS template (useful for reusable components). |
| `<%- include(filename) %>`| `<%- include('footer') %>`          | Includes another EJS template with unescaped HTML content. |

### Passing Data to Templates

- When building web applications using Node.js and the EJS view engine, it's a common need to display dynamic data in the HTML templates. This dynamic data can come from various sources, such as a database, user input, or an external API. To achieve this, we can pass data from server-side JavaScript code to the EJS templates.

#### Using the `res.render()` Method

- In Express route handler, when rendering an EJS template, we can use the `res.render()` method. This method takes two main arguments:
  - The name of the EJS template to render.
  - An object containing the data you want to pass to the template.
- For example, consider a route that displays user information
  
  ```js
  app.get('/', (req, res) => {
    const random = Math.floor(Math.random() * 100) + 1;
    res.render('homepage', { number: random });
  });

  app.get('/user', (req, res) => {
    const userData = {
        username: 'john_doe',
        email: 'john@example.com',
    };
    res.render('user', { data: userData });
  });
  ```

  In this code, we're rendering the `user` template and passing an object called `data` (key) containing user-related information(value) to it.

#### Accessing Data in EJS Template

- Inside the EJS template, we can access the data we've passed using the `<%= %>` syntax. For instance, in the `'user.ejs'` template, we can display the username and email like this

  ```html
  <p>User Name: <%= data.username %></p>
  <p>Email: <%= data.email %></p>
  ```

### Conditionals in EJS

- #### Using <% if ... %> ... <% else %> ... <% endif %>
  
  ```ejs
  <ul>
   <% if (user.isAdmin) { %>
       <li>Admin Options</li>
   <% } else { %>
       <li>User Options</li>
   <% } %>
  </ul>

  <!-- Another one -->

  <% if (randNum % 2 === 0) { %>
        <h3>It's an even Number...</h3>
  <% } else { %>
        <h3>It's an Odd one...</h3>
  <% } %>
  ```

- #### Using Ternary Operator `(<%= condition ? 'trueValue' : 'falseValue' %>)

  ```ejs
  <p><%= user.isAdmin ? 'Admin' : 'User' %></p>
  
  <!-- another one -->
  <!-- Escape the html tags outside the `<%=  %>` -->

  <h4><%= randNum % 2 === 0 ? 'Even Num' : 'Odd Num' %></h4>
  ```

### Loops in EJS

- #### Using <% for ... %> ... <% endfor %>

  ```ejs
  <ul>
   <% for (let i = 0; i < items.length; i++) { %>
       <li><%= items[i] %></li>
   <% } %>
  </ul>
  ```

- #### Using <% for ... in ... %> ... <% endfor %>

  ```ejs
  <ul>
   <% for (let key in user) { %>
       <li><%= key %>: <%= user[key] %></li>
   <% } %>
  </ul>
  ```

### Serving Static Assets in Express

In Express, we can serve static assets, such as HTML files, images, CSS files, JavaScript files, and more, using the built-in `express.static` middleware. This middleware makes it easy to serve files from a directory of your choice.

1. **Create a Project Structure**

- First, create a directory structure for your project. For example, you might have a directory structure like this:
  - **project/**
├── public/
│ ├── index.html
│ ├── css/
│ │ ├── style.css
│ ├── images/
│ │ ├── image.jpg
│ ├── js/
│ │ ├── script.js
├── server.js

- In this example, the `public` directory contains static assets like HTML files, styles, images, and scripts.

2. **Create an Express App**

- In the `server.js` file, create an Express app and configure it to serve static assets using the `express.static` middleware:

  ```javascript
  const express = require('express');
  const path = require('path');
  const app = express();

  // Serve static files from the 'public' directory
    // app.use(express.static('public'));
    // better approach
  app.use(express.static(path.join(__dirname, 'public')));

  // Define your routes and other middleware here

  const port = process.env.PORT || 3000;
  app.listen(port, () => {
    console.log(`Server is running on port ${port}`);
  });
  ```

- The `express.static` middleware is used to serve static files from the '**public**' directory. When a request is made for a file, Express will look in the '**public**' directory for that file.

### Accessing Static Assets

- Now, any file in the 'public' directory can be accessed directly via a URL. For example:

  - `http://localhost:3000/index.html`
  - `http://localhost:3000/styles/style.css`
  - `http://localhost:3000/images/image.jpg`

- Express will automatically handle serving these files.
  - Referring css files inside `ejs` from `express.static`

    ```html
    <!-- Reference the CSS file using a **relative path from the public folder** -->
    <link rel="stylesheet" type="text/css" href="/css/bootstrap.min.css">`
    ```

### EJS & Partials

- Partials are reusable components that you can include in your EJS templates.
- Create partials as separate EJS files. For example, create a `header.ejs` and `footer.ejs` file for the header and footer sections of pages.

#### Including Partials

- Use the `<%- include('partialName') %>` syntax to include a partial in your EJS templates. For example:

  ```ejs
  <%- include('header') %>
  <h1>Welcome to My Website</h1>
  <%- include('footer') %>
  ```

#### Passing Data to Partials

- We can pass data to partials to customize their content. For example, if we want to pass a pageTitle to the header partial:

    ```ejs
    <%- include('header', { pageTitle: 'Home' }) %>
    ```

  - In the `header.ejs`, we can use the pageTitle variable like this:

  ```ejs
  <title><%= pageTitle %></title>
  ```

- Partials are especially useful for headers, footers, navigation menus, and other components that appear on multiple pages of your website.
- They help us keep our templates organized and **DRY**, making it easier to maintain and update our sites.

#### Code Demonstration

- **index.js**

  ```js
  // EJS & Partials - index.js

  const express = require("express");
  const path = require("path");
  const subreddit_data = require("./data.json");
  const app = express();

  app.set("view engine", "ejs");
  app.set("views", path.join(__dirname, "views"));
  app.use(express.static(path.join(__dirname, "public")));

  app.get("/", (req, res) => {
    res.render("home", { page_title: "Homepage" });
  });

  app.get("/r/:subreddit_name", (req, res) => {
    const { subreddit_name } = req.params;
    const data = subreddit_data[subreddit_name];
    if (data) {
      res.render("subreddit", { subreddit_name, ...data });
    } else {
      res.status(404).render("404", { page_title: subreddit_name, type: "Subreddit" });
    }
  });

  app.get("/rand", (req, res) => {
    const randNum = Math.floor(Math.random() * 100) + 1;
    res.render("random", { page_title: "Random Number Generator", randNum });
  });

  app.use((req, res) => {
    const url = req.originalUrl.slice(1);
    res.status(404).render("404", { page_title: url, type: "Page" });
  });

  app.listen(3000, () => {
    console.log(`listening on port 3000`);
  });
  ```

- **header.ejs**

  ```ejs
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="/css/bootstrap.min.css">
    <script src="/js/jquery.min.js"></script>
    <script src="/js/bootstrap.min.js"></script>
    <!-- convert string to sentence case -->
    <title><%= page_title.toLowerCase().split(' ').map(word => word.charAt(0).toUpperCase() + word.slice(1)).join(' '); %></title>
  </head>
  <body>
  <%- include('./navbar') %>
  ```

- **navbar.ejs**  

  ```ejs
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <a class="navbar-brand" href="#">Express App Demo</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav">
        <li class="nav-item active">
          <a class="nav-link" href="/">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="/rand">Random Number</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="/r/chickens">Chickens</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="/r/soccer">Soccer</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="/r/mightyharvest">Mighty Harvest</a>
        </li>
      </ul>
    </div>
  </nav>
  ```

- **random.ejs**  

  ```ejs
  <%- include('../partials/header', {page_title}) %>
    <h1>New Number is <%= randNum %> </h1>
    <% if (randNum % 2 === 0) { %>
        <h3>It's an even Number...</h3>
    <% } else { %>
        <h3>It's an Odd one...</h3>
    <% } %>

    <h2>An alternate way using ternary operator</h2>

    <h4><%= randNum % 2 === 0 ? 'Even Num' : 'Odd Num' %></h4>   
  </body>
  </html>
  ```

- **subreddit.ejs**

  ```ejs
  <%- include('../partials/header', { page_title: subreddit_name }) %>
    <h4>Browsing the <%= subreddit_name %> subreddit!</h2>
    <h6><b>Subscribers Count - </b><%= subscribers %></h6>
    <h6><b>Description - </b> <%= description %></h6>
    <hr>

    <% for (let post of posts) { %>
        <article>
            <p><%= post.title %> - <b><%= post.author %></b></p>
            <% if (post.img) {%>
            <img src="<%= post.img %>" alt="" />
            <% } %>
        </article>
        <hr />
        <% } %>    
  </body>
  </html>
  ```

- **404.ejs**  

  ```ejs
  <%- include('../partials/header', { page_title: `404... ${page_title + ' ' + type } not found` }) %>
    <% const title_sen_case = page_title.toLowerCase().split(' ').map(word => word.charAt(0).toUpperCase() + word.slice(1)).join(' '); %>
    <h2><%= title_sen_case + " " + type %> not Found! <a href="/">Click here to redirect to homepage</a></h2>
  </body>
  </html>
  ```

- **data.json**  

  ```json
  {
    "soccer": {
        "name": "Soccer",
        "subscribers": 800000,
        "description": "The football subreddit. News, results and discussion about the beautiful game.",
        "posts": [
            {
                "title": "Marten de Roon to make pizza for more than 1,000 people in Bergamo if Atalanta win the Champions league.",
                "author": "joeextreme"
            },
            {
                "title": "Stephan Lichtsteiner has retired from professional football",
                "author": "odd person"
            },
            {
                "title": "OFFICIAL: Dani Parejo signs for Villareal.",
                "author": "joeextreme"
            }
        ]
    },
    "chickens": {
        "name": "Chickens",
        "subscribers": 23956,
        "description": "A place to post your photos, video and questions about chickens!",
        "posts": [
            {
                "title": "Naughty chicken hid under a shed for 3 weeks and came home with 14 chicks today!",
                "author": "joeextreme",
                "img": "https://preview.redd.it/sja35au7whd51.jpg?width=640&crop=smart&auto=webp&s=c39f8a99896b55fafc5d0ed882040a963ca54409"
            },
            {
                "title": "Had to kill my first chicken today. Does it get any easier?",
                "author": "sad boi"
            },
            {
                "title": "My five year old chicken set and hatched one baby. I guess she wanted to be a mama one more time.",
                "author": "tammythetiger",
                "img": "https://preview.redd.it/lervkuis3me51.jpg?width=640&crop=smart&auto=webp&s=6a18ab3c4daa80eccf3449217589b922fa443946"
            }
        ]
    },
    "mightyharvest": {
        "name": "Mighty Harvest",
        "subscribers": 44002,
        "description": "Feeding many villages and village idiots for 10s of days.",
        "posts": [
            {
                "title": "My first meyer lemon ripened today. Im so proud of the little guy. Banana for scale",
                "author": "proudmamma",
                "img": "https://preview.redd.it/1bz6we4j54941.jpg?width=640&crop=smart&auto=webp&s=a036ea99299f7737efde9f6c3bfa43893f5eaa00"
            },
            {
                "title": "I think I overestimated the harvest basket size I needed.",
                "author": "grower123",
                "img": "https://preview.redd.it/4h99osd25i351.jpg?width=640&crop=smart&auto=webp&s=d651250a345bbceeba7a66632e8c52a02d71bc73"
            }
        ]
    }
  }
  ```
