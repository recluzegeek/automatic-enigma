# Mongoose

## ODM

- Object Data Mapper or Object Documnet Mapper
- Mongoose maps data coming from database into usable javascrvipt objects while **ORM**  --- Object Relational Mapping for SQL or Relational Databases.
- Offers easy way to validate data and build complex queries from using the comfort of JS

### Connecting with Mongoose

```js
const mongoose = require('mongoose');

// Connect to the MongoDB database named "movies" running on the local machine at mongodb://127.0.0.1:27017/movies
mongoose.connect('mongodb://127.0.0.1:27017/movies');

// Improved error handling using connection events
mongoose.connection.on('connected', () => {
    console.log('Connected Successfully');
});

mongoose.connection.on('error', (err) => {
    console.log('Error Encountered');
    console.error(err);
});
```

- For authenticating with MongoDB Atlas

    ```js
    // Replace 'username' and 'password' with your MongoDB Atlas credentials
    const username = 'your_username';
    const password = 'your_password';

    // Connect to the MongoDB database named "movies" with authentication
    mongoose.connect(`mongodb://${username}:${password}@127.0.0.1:27017/movies`, { useNewUrlParser: true, useUnifiedTopology: true });
    ```

### Creating Mongoose Model

- **Define a Schema** -- Begin by describing the schema for your collection or table using the Mongoose constructor. The schema serves as a blueprint for what will be stored in the collection. It is an object passed to the mongoose. Schema static constructor.
  
  ```js
  const movieSchema = new mongoose.Schema({
      title: String, 
      year: Number,
      score: Number,
      rating: String,
  });
  ```

- **Create Model** --- Model your schema to a JavaScript class. The first argument is the name of the class, capitalized and singular. Mongoose automatically creates a collection from this name in lowercase and pluralized. The second argument is the defined schema.

    ```js
    const Movie = mongoose.model('Movie', movieSchema);
    ```

- **Instantiate and Store** --- Now, you can create instances of the model. Ensure that the class name is capitalized and singular. Instances are constructed on the JavaScript side but are not stored in the database until you explicitly save them.

    ```js
    const amadeus = new Movie({title: 'Amadeus', year: 1985, score: 9.3, rating: 'R'});
    ```

- **Save to Database** --- To persist the instance in the database, call the save method on the instance. Remember that the save method is asynchronous, so handle the returned promise appropriately, and consider using a try-catch block for error handling.

    ```js
    const mongoose = require('mongoose');

    // Define the schema for the "movies" collection
    const movieSchema = new mongoose.Schema({
        title: String,
        year: Number,
        score: Number,
        rating: String,
    });

    // Create a Mongoose model based on the schema
    const Movie = mongoose.model('Movie', movieSchema);

    // Create a new movie instance
    const amadeus = new Movie({ title: 'Amadeus', year: 1985, score: 9.3, rating: 'R' });

    // Save the movie instance to the database
    amadeus.save()
        .then(() => {
            console.log('Movie saved successfully');
        })
        .catch((err) => {
            console.error('Error saving movie:', err);
        });

    // Saving Movie Asynchronously
    try {
        await amadeus.save();
        console.log('Movie saved successfully');
    } catch (err) {
        console.error('Error saving movie:', err);
    }
    ```
