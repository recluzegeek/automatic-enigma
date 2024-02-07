# Mongoose With Express

## Mongoose + Express Setup

- Install express, ejs and mongoose via npm.
- Setup the mongoose connection logic, express and ejs

    ```js
    const express = require('express');
    const app = express();
    const mongoose = require('mongoose');
    const path = require('path');

    mongoose.connect('mongodb://127.0.0.1:27017/test')
        .then(() => {
            console.log('Mongoose Connection Established');
        })
        .catch(err => {
            console.log('Error Establishing Mongoose Connection');
            console.log(err);
        })

    app.set('views', path.join(__dirname, 'views'));
    app.set('view engine', 'ejs');

    // for handling html form data via POST request
    app.use(express.urlencoded({ extended: true }));
    app.use(methodOverride('_method')); // for handling put, patch, delete requests  
    app.use(express.json());

    app.get('/', (req, res) => {
        res.send('Homepage');
    });
    
    app.listen(3000, () => {
        console.log('Listening on port 3000');
    });
    ```

### View All Products &mdash; Read in CRUD

- All Products Route Handler

    ```js
    app.get('/', async (req, res) => {
        const products = await Product.find({});
        res.render('products/index', { products })
    });
    ```

    ```html
    <!-- products/index -->
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>All Products</title>
    </head>
    <body>
        <h1>All Products!</h1>
        <ul>
            <% products.forEach(element => { %>
            <li>
                <a href="/product/show/<%= element._id%>"><%= element.name %></a>
            </li>    
            <% }) %>
        </ul>
        <a href="/product/new">Add New Product</a>
    </body>
    </html>
    ```

#### View Single Product Details

- Route handler for viewing details of single product

```js
app.get('/product/show/:id', async (req, res) => {
    const { id } = req.params;
    const product = await Product.findById(id);
    res.render('products/show', { product })
});
```

```html
<!-- products/show -->
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>
        <%= product.name %> Info
    </title>
</head>

<body>
    <h3>
        <%= product.name %> Info
    </h3>
    <ul>
        <li>Price: $<%= product.price %>
        </li>
        <li>Category: <%= product.category %>
        </li>
    </ul>
    <form action="/product/edit/<%= product._id%>" method="get">
        <button type="submit">Edit Product Info</button>
    </form>
    <form action="/product/delete/<%= product._id%>?_method=DELETE" method="post">
        <button type="submit">Delete Product</button>
    </form>
    <a href="/">Back to Products Homepage</a>
</body>

</html>
```

### Add new Products &mdash; Create in CRUD

- Route Handler for Product creation html form

    ```js
    app.get('/product/new', (req, res) => {
        res.render('products/new');
    });
    ```

    ```html
    <!-- products/new -->
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Add New Product</title>
    </head>
    <body>
        <h1>Add new Product</h1>
        <form action="/product" method="post">
            <div>
                <label for="name">Product Name</label>
                <input type="text" name="product-name" id="name" placeholder="Product Name">
            </div>
            <div>
                <label for="price">Price (Unit) </label>
                <input type="number" name="product-price" id="price" placeholder="Product Price" step="any">
            </div>
            <div>
                <label for="category">Select Product Category</label>
                <select name="category" id="category">
                    <option value="vegetable">Vegetable</option>
                    <option value="fruit">Fruit</option>
                    <option value="dairy">Dairy</option>
                </select>
            </div>
            <button type="submit">Submit</button>
        </form>
    </body>
    </html>
    ```

### Update Products &mdash; Update in CRUD

- Editing form is available when viewing details of a single product. On clicking, it'll be redirected to `update.ejs`
  - For updating, `PUT` request is used, which typically ain't available in the html forms. So, using `method-override` of npm to handle other types of requests.

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Add New Product</title>
    </head>
    <body>
        <h1>Update <%= product.name %> </h1>
        <form action="/product/<%= product._id%>?_method=PUT" method="post">
            <div>
                <label for="name">Product Name</label>
                <input type="text" name="product-name" id="name" placeholder="Product Name" value="<%= product.name %>">
            </div>
            <div>
                <label for="price">Price (Unit) </label>
                <input type="number" name="product-price" id="price" placeholder="Product Price" step="any" value="<%= product.price %>">
            </div>
            <div>
                <label for="category">Select Product Category</label>
                <select name="category" id="category">
                    <option value="vegetable">Vegetable</option>
                    <option value="fruit">Fruit</option>
                    <option value="dairy">Dairy</option>
                </select>
            </div>
            <button type="submit">Submit</button>
        </form>
    </body>
    </html>
    ```

### Deleting Product &mdash; Delete in CRUD

- Route is handled via detailed product page.

    ```js
    app.delete('/product/delete/:id', async (req, res) => {
        const { id } = req.params;
        await Product.findByIdAndDelete(id);
        res.redirect('/');
    });
    ```