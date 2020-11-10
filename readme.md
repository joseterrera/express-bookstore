### Express Bookstore

This is an express.js application that validates a resource and then add tests to the application.

### Why do we need validation?
A server lacking adequate validation can result in corrupt or incomplete data, extra server or database operations due to unsuccussfuly trying to process bad data, or displaying unhelpful errors to the user.

To avoid this, we use a JSON Schema, which provides an easy to maintain validation system and allows user data to fail fast before entering the database.

Instead of just checking if req.body is null or undefined, we dig deeper and define if the data field is a required one, or if needs to be a string.

Once the schemas are defined [here](https://jsonschema.net/login), we validate inside our routes:

```js
const jsonschema = require("jsonschema");
const bookSchema = require("../schemas/bookSchema.json");

router.post("/with-validation", function(req, res, next) {
  const result = jsonschema.validate(req.body, bookSchema);

  if (!result.valid) {
    // pass validation errors to error handler
    //  (the "stack" key is generally the most useful)
    let listOfErrors = result.errors.map(error => error.stack);
    let error = new ExpressError(listOfErrors, 400);
    return next(error);
  }

  // at this point in code, we know we have a valid payload
  const { book } = req.body;
  return res.json(book);
});

```


### Setting up:
1. On the terminal type psql and then `CREATE DATABASE books;`
2. Exit and then type `psql books < data.sql` to import the tables
4. Run `npm install` 
5. Run `nodemon server.js`
6. To run tests `npm run tests` after creating an importing a test database.

