---
title: mongo_database
updated: 2023-10-28 12:49:29
---

## MongoDB

- Mongo is a **document database**(refer the table below), which we can use to store and retrieve complex data from.

| Aspect                 | Description                                                                  |
|------------------------|------------------------------------------------------------------------------|
| **Data Structure**     | Documents stored in a flexible, JSON-like format                            |
| **Schema Flexibility** | Schemaless or schema-flexible, allowing for dynamic and varied data structures |
| **Query Language**     | MongoDB query language based on JavaScript and JSON-like syntax              |
| **Data Retrieval**     | Queries using methods such as find, aggregate, and indexing                  |

### Why use a Database?

- Database can handle large amounts of data efficiently and store it compactly.
- They provide tools for easy insertion, querying and updating of data.
- They generally offer security features and control over access to data.
- They (generally) scale well.

| Aspect                  | SQL Databases                   | NoSQL Databases                                             |
|-------------------------|---------------------------------|-------------------------------------------------------------|
| **Structure**           | Relational and table-based      | Non-relational, various types (document-based, key-value, graph stores etc.) |
| **Query Language**      | SQL                             | Various query languages (depends on the database type)       |
| **Schema**              | Predefined schema               | Flexible or no schema required                               |
| **Scalability**         | Primarily vertically scalable   | Primarily horizontally scalable                               |
| **ACID Properties**     | Emphasizes ACID compliance      | Emphasizes partition tolerance and availability               |
| **Consistency Model**   | Strong consistency               | Can sacrifice some consistency for availability (based on CAP theorem) |
| **Use Cases**           | Well-structured data, complex queries, transactions | Unstructured or rapidly changing data, high scalability, quick iterations |
| **Example Databases**           | MySQL, Postgress, SQLite, Oracle, Microsoft SQL Server | MongoDB, Neo4j, Couch DB, Cassandra, Redis |

## Installing MongoDB

- On arch based system, install it from AUR using your preferred aur helper `yay mongodb-bin` or you can build one for yourself from source.
- After installing mongo, run `sudo systemctl start mongodb`
- run `mongosh` from your shell to start using its REPL(Read-Evaluate-Print-Loop).

### BSON - Binary JSON

| Aspect                         | JSON                                      | BSON                                                  |
|--------------------------------|-------------------------------------------|-------------------------------------------------------|
| **Format**                     | Lightweight, human-readable data interchange format | Binary-encoded serialization of JSON-like documents  |
| **Structure**                  | Key-value pairs and ordered lists         | An extension of JSON supporting additional data types and binary encoding |
| **Usage**                      | Transmitting data between a server and a web application, configuration files, APIs | Used in MongoDB for data storage and retrieval |
|**Key Points**                 | - Simple and readable. <br> - Represents data using attribute-value pairs. <br> - No schema enforcement | - Binary representation of JSON-like documents. <br> - Supports more data types (e.g., Date, Binary). <br> - Efficient parsing due to its binary nature |

### Using MongoDB

- We can pass `json` or Javascript objects or array of objects.
- If we not set an `id` property, `mongo` will create a unique and randomly generated ids for our documents.

#### Show Databases

- `show dbs` or `show databases`

#### Create / Use Database

- `use <db-name>` - use if the database already exists(switching to the mentioned one and performing queries) or create brand new if doesn't.

#### Creating Collection

- Collection ins no-sql Behaves like tables in sql.
- **Inserting a single document** - Use `db.<collection_name>.insertOne()`
- **Inserting Multiple documents** - Use `db.<collection_name>.insertMany()` with an array of JS objects.
- **Best Option** - Use `db.<collection_name>.insert()` when we don't know whether the insertion will be of a single or multiple documents.

#### Searching Documents of Collections

- **Return all documents of collection** - use `db.<collection_name>.find()` similar to `db.<collection_name>.find({})`
- **Return all matching document having catFriendly to true in dogs collection** - `db.dogs.find({catFriendly: true})`
  - **Return only single or first matching document** - `db.dogs.findOne({catFriendly: true})`
- When dealing with documents where objects are nested inside a parent document, altering the `find` query becomes necessary to navigate to the specific nested object. In this context, the search for 'burger' within the nested structure is performed using the query: `db.orders.find({'1.name': 'burger'})`.

   ```json
    {
        '0': { name: 'Meggie', qty: 3 },
        '1': { name: 'burger', qty: 5 },
        '2': { name: 'mango-shake', qty: 3 },
        '_id': ObjectId("653de89f7bb234f0a203e403")
    }
    ```

- This query searches for 'burger' within the nested structure by targeting the field named '1' and its nested attribute name, enabling the retrieval of the desired object from the document.

### Updating Documents of Collection

- **Updating a single document** - `db.<collection_name>.updateOne({<matching_criteria>}, {$set: {<new_value>}})`
  - For instance, let's consider a product with the following structure:

    ```json
    {
      "_id": ObjectId("61d1b867b5f4c1331530515e"),
      "name": "Smartphone XYZ",
      "price": 799.99,
      "availability": true,
      "description": "A high-end smartphone with advanced features."
    }
    ```

    Let's say there's an update for this product's price and availability. We can do something like this:

    ```json
    db.products.updateOne(
      { _id: ObjectId("61d1b867b5f4c1331530515e") }, // Specify the product by its _id
      {
        $set: {
          price: 899.99,      // Update the price
          availability: false // Update the availability
        }
      }
    )
    ```

- The **`$set`** operator is used to update existing fields or add new fields to a document.
- **`$currentDate`** operator is used within update operations to set a field to the current date or time.

    ```json
    db.collection.updateOne(
      { _id: ObjectId("document_id") },
      { $currentDate: { lastModified: true } }
    )
    ```

- `db.<collection_name>.updateMany()` - works in similar way as `db.<collection_name>.updateOne()` but it updates all the documents matching the specified filter.
- `db.<collection_name>.replaceOne()` or `db.<collection_name>.replaceMultiple()` - replaces or modify all of the fields of the matching documents except the unique identifier which is in most of the cases in `_id`

### Deleting with Mongo

- `db.<collection_name>.deleteOne()` - deletes a single document based on the provided filter.
  - `db.collection.deleteOne({ _id: ObjectId("document_id") })`
- `db.<collection_name>.deleteMany()` - deletes multiple documents matching the specified filter.
  - `db.collection.deleteMany({ status: "Inactive" })`
- **Deleting all documents of a collection** - `db.<collection_name>.deleteMany({})`

### Additional Mongo Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `$eq`    | Matches values that are equal to a specified value. | `{ age: { $eq: 30 } }` |
| `$ne`    | Matches all values that are not equal to a specified value. | `{ age: { $ne: 25 } }` |
| `$gt`    | Matches values that are greater than a specified value. | `{ age: { $gt: 20 } }` |
| `$gte`   | Matches values that are greater than or equal to a specified value. | `{ age: { $gte: 18 } }` |
| `$lt`    | Matches values that are less than a specified value. | `{ age: { $lt: 40 } }` |
| `$lte`   | Matches values that are less than or equal to a specified value. | `{ age: { $lte: 35 } }` |
| `$in`    | Matches any of the values specified in an array. | `{ status: { $in: ['active', 'pending'] } }` |
| `$nin`   | Matches none of the values specified in an array. | `{ status: { $nin: ['archived', 'deleted'] } }` |
| `$or`    | Performs a logical OR operation on an array of two or more query expressions. | `{ $or: [ { qty: { $lt: 20 } }, { item: "lamp" } ] }` |
| `$and`   | Joins query clauses with a logical AND. | `{ $and: [ { price: { $ne: null } }, { qty: { $gt: 5 } } ] }` |
| `$not`   | Inverts the effect of a query expression and returns documents that do not match the query expression. | `{ price: { $not: { $gt: 10 } } }` |
| `$exists`| Matches documents that have the specified field. | `{ name: { $exists: true } }` |
| `$type`  | Selects documents if a field is of the specified type. | `{ age: { $type: "number" } }` |
| `$regex` | Provides regular expression capabilities for pattern matching strings in queries. | `{ name: { $regex: '^J' } }` |
