# Mongoose

## ODM

- Object Data Mapper or Object Document Mapper
- Mongoose maps data coming from database into usable javascript objects while **ORM**  &mdash; Object Relational Mapping for SQL or Relational Databases.
- Offers easy way to validate data and build complex queries from using the comfort of JS

## Connecting with Mongoose

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

- **Define a Schema** &mdash; Begin by describing the schema for your collection or table using the Mongoose constructor. The schema serves as a blueprint for what will be stored in the collection. It is an object passed to the mongoose. Schema static constructor.
  
  ```js
  const movieSchema = new mongoose.Schema({
      title: String, 
      year: Number,
      score: Number,
      rating: String,
  });
  ```

- **Create Model** &mdash; Model your schema to a JavaScript class. The first argument is the name of the class, capitalized and singular. Mongoose automatically creates a collection from this name in lowercase and pluralized. The second argument is the defined schema.

    ```js
    const Movie = mongoose.model('Movie', movieSchema);
    ```

- **Instantiate and Store** &mdash; Now, you can create instances of the model. Ensure that the class name is capitalized and singular. Instances are constructed on the JavaScript side but are not stored in the database until you explicitly save them.

    ```js
    const amadeus = new Movie({title: 'Amadeus', year: 1985, score: 9.3, rating: 'R'});
    ```

- **Save to Database** &mdash; To persist the instance in the database, call the save method on the instance. Remember that the save method is asynchronous, so handle the returned promise appropriately, and consider using a try-catch block for error handling.

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

### Insert Many &mdash; Insertion with Mongoose

- In Mongoose, the `insertMany` method is used for bulk insertion of documents into a MongoDB collection. It allows the insertion of an array of documents in a single operation, which can be more efficient than inserting them one by one.
- Assuming you have a Mongoose model for a "movies" collection with the following schema:

    ```js
    const movieSchema = new mongoose.Schema({
        title: String,
        year: Number,
        score: Number,
        rating: String,
    });
    ```

- Use `insertMany` to insert multiple movie documents into the "movies" collection:

    ```js
    const moviesToInsert = [
        { title: 'Inception', year: 2010, score: 8.8, rating: 'PG-13' },
        { title: 'The Shawshank Redemption', year: 1994, score: 9.3, rating: 'R' },
        { title: 'The Dark Knight', year: 2008, score: 9.0, rating: 'PG-13' }
    ];

    Movie.insertMany(moviesToInsert)
    .then((docs) => {
            console.log(`${docs.length} movies inserted successfully`);
    })
    .catch((err) => {
        console.error('Error inserting movies:', err);
    });
    ```

- The `insertMany` method takes an array of documents as its argument.
- It returns a promise that resolves to an array of inserted documents.
- The then block is executed if the insertion is successful, and the catch block handles errors.

### Finding with Mongoose &mdash; find(), findById(), findOne()

- In Mongoose, the `find()` method is used to query documents in a MongoDB collection. Allows the retrieval of documents based on specified criteria. Here's an example of how you can use the find method, assuming a Mongoose model for a "movies" collection:

    ```js
    // Find movies with a specific title
    Movie.find({ title: 'Inception' })
    .then((movies) => {
        console.log('Movies with title Inception:', movies);
    })
    .catch((err) => {
        console.error('Error finding movies:', err);
    });

    // Find movies released in 2021 with a score greater than 8.0
    const highRated2021Movies = await Movie.find({ year: 2021, score: { $gt: 8.0 } });
    console.log('High-rated movies released in 2021:', highRated2021Movies);

    // Another one
    Movie.find({ rating: 'PG-13' })
    .then((movies) => {
        console.log('Movies with rating PG-13:', movies);
    })
    .catch((err) => {
        console.error('Error finding movies:', err);
    });
    ```

  - In this example,
    - **Movie** is the Mongoose model for the "movies" collection.
    - `find({ rating: 'PG-13' })` queries for documents where the rating field is 'PG-13'.
    - The `then` block is executed if the query is successful, and the `catch` block handles errors.

- **`findById()`** &mdash; The `findById()` method is a specific variation of `find()` used to retrieve a single document by its unique _id field.

    ```js
    const movieId = '5f72b4bdf6e96b14d0968dab'; // Replace with an actual document ID
    const movie = await Movie.findById(movieId);
    console.log('Movie found by ID:', movie);
    ```

- **`findOne()`** &mdash; The `findOne()` method retrieves a single document that matches the specified criteria. It is commonly used when you want to find a single document that satisfies certain conditions. Unlike `find()`, which returns an array of objects, `findOne()` returns only the first matching object.

    ```js
    const movie = await Movie.findOne({ title: 'Inception' });
    console.log('First movie found with title Inception:', movie);
    ```

### Updating with Mongoose &mdash; updateMany(), findOneAndUpdate()

- **`updateMany()`** &mdash; The `updateMany()` method is used to update multiple documents in a MongoDB collection that match a specified filter. It allows the modification of multiple documents in a single operation.

    ```js
    // Update the rating of all movies released before 2000 to 'PG'
    const result = await Movie.updateMany(
        { year: { $lt: 2000 } },
        { $set: { rating: 'PG' } }
    );

    console.log(`${result.nModified} movies updated successfully`);
    ```

> **Note**: Unlike some other update methods, `updateMany()` doesn't return the updated documents. Instead, it returns an object with information about the update operation, including the number of documents matched and the number of documents modified.
>
>> In this example, `result.nModified` indicates the number of documents that were modified by the update operation.

- **`findOneAndUpdate()`** &mdash; The `findOneAndUpdate()` method is used to update a single document that matches a specified filter. It returns the original document by default, but you can also request the updated document.

    ```js
    // Update the score of the movie with title 'Inception' and return the updated document
    const updatedMovie = await Movie.findOneAndUpdate(
        { title: 'Inception' },
        { $set: { score: 9.1 } },
        { new: true }   // returns the updated document
    );

    console.log('Updated movie:', updatedMovie);
    ```

- Both methods accept a filter as the first argument, specifying the documents to update.
- The second argument is the update operation using MongoDB update operators (`$set`, `$inc`, etc.).
- The third argument (`{ new: true }` for `findOneAndUpdate()`) controls whether the original or updated document is returned.

### Deleting Methods in Mongoose

- **`deleteOne()`** &mdash; Deletes a single document in a collection based on a specified filter.

     ```javascript
     const result = await Movie.deleteOne({ title: 'Inception' });
     console.log(`${result.deletedCount} movie deleted successfully`);
     ```

- **`deleteMany()`** &mdash; Deletes multiple documents in a collection based on a specified filter.

     ```javascript
     const result = await Movie.deleteMany({ year: { $lt: 2000 } });
     console.log(`${result.deletedCount} movies deleted successfully`);
     ```

- **`remove()`** &mdash; Removes the current document instance from the database.

     ```javascript
     const movie = await Movie.findOne({ title: 'Inception' });
     await movie.remove();
     console.log('Movie removed successfully');
     ```

- **`findByIdAndDelete()`** &mdash; Finds a document by its ID, deletes it, and returns either the original or the deleted document.

     ```javascript
     const deletedMovie = await Movie.findByIdAndDelete('5f72b4bdf6e96b14d0968dab');
     console.log('Deleted movie:', deletedMovie);
     ```

- **`findOneAndDelete()`** &mdash; Finds a single document in a collection based on a specified filter, deletes it, and returns either the original or the deleted document.

     ```javascript
     const deletedMovie = await Movie.findOneAndDelete({ title: 'Inception' });
     console.log('Deleted movie:', deletedMovie);
     ```

## Mongoose Schema Validation

Mongoose allows to define schema validations to ensure that data inserted into MongoDB adheres to certain rules. Here's an overview of Mongoose schema validation:

- **Built-in Validators** &mdash; Mongoose provides a variety of built-in validators that can be used while defining the schema. Examples include `required`, `min`, `max`, `enum`, and more

   ```javascript
   const userSchema = new mongoose.Schema({
       username: {
           type: String,
           required: true,
           unique: true,
           trim: true,
           minlength: 3,
           maxlength: 20
       },
       email: {
           type: String,
           required: true,
           unique: true,
           match: /^\S+@\S+\.\S+$/, // email regex
       },
       age: {
           type: Number,
           min: 18,
           max: 99
       },
       address: {
           street: {
            type: String,
            required: true
           },
           city{
            type: String,
            required: true
           },
           zipCode: {
            type: Number,
            required: true
           }
       }
   });
   ```

- **Middleware** &mdash; Mongoose also supports middleware, such as `pre` and `post`, where you can perform additional validation logic before or after certain operations

   ```javascript
   userSchema.pre('save', function(next) {
       // Custom validation logic before saving
       if (this.username === 'admin') {
           return next(new Error('Username cannot be "admin"'));
       }
       next();
   });
   ```

### Validation with Mongoose Update

Validating updates in Mongoose involves ensuring that the data being updated adheres to certain rules defined in the schema.

Assuming a Mongoose model for a user with the following schema:

```javascript
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
    username: {
        type: String,
        required: true,
        unique: true,
        trim: true,
    },
    email: {
        type: String,
        required: true,
        unique: true,
        match: /^\S+@\S+\.\S+$/,    // email regex
    },
    age: {
        type: Number,
        min: 18,
        max: 99,
    },
});

const User = mongoose.model('User', userSchema);
```

Now, let's say you want to update a user's email address and ensure that the new email adheres to the same validation rules as when creating a new user. It can be achieved using the `validate` option in Mongoose.

```javascript
const updatedUserData = {
    email: 'new-email@example.com', // This is an example, update it with the new email
};

// Assuming userId is the ID of the user you want to update
User.findByIdAndUpdate(userId, updatedUserData, { runValidators: true })
    .then(updatedUser => {
        console.log('User updated successfully:', updatedUser);
    })
    .catch(error => {
        console.error('Error updating user:', error);
    });
```

#### enums

- In this example, the `enum` validation ensures that the role field can only have values 'user' or 'admin'. If an attempt is made to save a user with a different role, Mongoose will throw a validation error. It can be used in other situations also such as whether the size is Small(S), Medium(M), XL(Extra Large), XS(Extra Small) etc.

    ```js
    const userSchema = new mongoose.Schema({
        role: {
            type: String,
            enum: ['user', 'admin'],
            required: true,
        },
        // Other fields...
    });
    
    const User = mongoose.model('User', userSchema);
    ```

### Custom Validation Messages

- When we define validations such as `minlength` and `enum` in a Mongoose schema, we can provide custom error messages to provide more meaningful information about the validation failure.

    ```js
    const userSchema = new mongoose.Schema({
        username: {
            type: String,
            required: true,
            unique: true,
            trim: true,
            minlength: [3, 'Username should be greater than 3 characters'],
            maxlength: [20, 'Username should not exceed more than 20 characters'],
        },
        // Other fields...
    });
    ```

#### Key points

1. Use `findByIdAndUpdate()` (or any other update method) to find and update the user.
2. Set the `runValidators` option to `true` in the update options. This ensures that Mongoose runs the validators defined in the schema during the update operation.

## Model Instance Methods

- In Mongoose, model instance methods are methods that are **defined on the model's schema** and can be called on instances of the model.
  - These methods operate on a specific instance of a document and can be useful for performing actions or computations related to that specific document.
  - Assuming a mongoose model of a user:

    ```js
    const mongoose = require('mongoose');

    const userSchema = new mongoose.Schema({
        username: String,
        email: String,
        age: Number,
    });

    // Define a model instance method
        // <schema_name>.methods.<instance_method>
    userSchema.methods.getInfo = function() {
        return `Username: ${this.username}, Email: ${this.email}, Age: ${this.age}`;
    };

    const User = mongoose.model('User', userSchema);
    ```

    >**Note**
    >
    > Model instance methods are written before a model is compiled by mongoose with `mongoose.model()`
    >> Inside the method, `this` refers to the specific document instance, giving access to its data, so its better not to use lambda or arrow function syntax while working with model instance methods.

    - In this example, `getInfo()` is a model instance method that can be called on a specific user instance. Here's a usage:

        ```js
        const myUser = new User({
            username: 'john_doe',
            email: 'john@example.com',
            age: 25,
        });

        // Call the model instance method
        const userInfo = myUser.getInfo();

        console.log(userInfo);
        // Output: "Username: john_doe, Email: john@example.com, Age: 25"
        ```

- More example usage of instance methods include toggling sale on products or adding categories to a specific product.

## Model Static Methods

- In Mongoose, model static methods are methods that are **defined on the model itself** and can be called directly on the model, not on instances of the model.
  - These methods operate at the model level and can be useful for performing actions or queries that involve the entire collection rather than a specific document instance.

    ```js
    const mongoose = require('mongoose');

    const userSchema = new mongoose.Schema({
        username: String,
        email: String,
        age: Number,
    });

    // Define a model static method
        // <schema_name>.statics.<static_method_name>
    userSchema.statics.findByUsername = function(username) {
        return this.findOne({ username });
    };

    const User = mongoose.model('User', userSchema);
    ```
  
  - In this example, `findByUsername()` is a model static method that you can call directly on the User model. Here's an example usage:

    ```js
    // Call the model static method
    User.findByUsername('john_doe')
        .then(user => {
            if (user) {
                console.log('User found:', user);
            } else {
                console.log('User not found');
            }
        })
        .catch(error => {
            console.error('Error finding user:', error);
        });
    ```

- More model static methods examples includes the fancier usage of existing model static methods such as creating, finding and updating with our custom logic.

## Mongoose Virtual

- In Mongoose, `virtuals` are additional fields or properties that can be defined on the schema but are not persisted to the database. Instead, they are computed properties based on the existing data in the document.
  - `Virtuals` are useful for deriving information or formatting data without actually adding more fields to the database.

    ```js
    const mongoose = require('mongoose');

    const userSchema = new mongoose.Schema({
        firstName: String,
        lastName: String,
    });

    // Define a virtual field 'fullName'
    userSchema.virtual('fullName').get(function() {
        return `${this.firstName} ${this.lastName}`;
    });

    const User = mongoose.model('User', userSchema);
    ```
  
  - In this example, the `fullName` virtual field is created by using the `virtual()` method on the schema. The virtual field is a **combination of the firstName and lastName** fields.
  - Now, when you retrieve a user from the database, the fullName virtual field will be available:

    ```js
    // Assuming user is retrieved from the database
    const user = new User({
        firstName: 'John',
        lastName: 'Doe',
    });

    console.log(user.fullName); // Output: "John Doe"
    ```

- **Key Points** &mdash; Virtuals are defined using the `virtual()` method on the schema.
  - **Usage** &mdash; Virtuals are accessed like regular fields but are not stored in the database.
  - **Getter and Setter Functions** &mdash; You can provide a getter function for the virtual field (as in the example) to compute its value. You can also provide a setter function if you want to handle setting the virtual field's value.

## Mongoose Middleware

- In the context of MongoDB and Mongoose, middleware refers to functions that are executed at specific points in the lifecycle of a Mongoose model or document. Middleware allows you to perform actions before or after certain events, such as saving a document, updating it, or validating it.

  - Mongoose supports several types of middleware, including document middleware, query middleware, and aggregate middleware. Here's a brief overview of each:

1. **Document Middleware**
   - Document middleware functions are executed before or after certain operations on individual documents.
   - Common events include `save`, `validate`, `remove`, and `init` (when a document is initialized).

    ```javascript
    const schema = new mongoose.Schema({ name: String });

    // Pre-save middleware
    schema.pre('save', function (next) {
      // Do something before saving
      // You can access the document using `this`
      console.log('Saving document...');
      next();
    });

    const Model = mongoose.model('Model', schema);
    const doc = new Model({ name: 'John' });

    doc.save(); // This will trigger the pre-save middleware
    ```

2. **Query Middleware**
   - Query middleware functions are executed before or after certain query operations.
   - Common events include `find`, `findOne`, `update`, and `remove`.

    ```javascript
    // Pre-find middleware
    schema.pre('find', function (next) {
      // Do something before a find operation
      console.log('About to execute find operation...');
      next();
    });

    // Post-find middleware
    schema.post('find', function (docs, next) {
      // Do something after a find operation
      console.log('Found documents:', docs);
      next();
    });
    ```
