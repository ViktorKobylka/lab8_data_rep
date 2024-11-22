# Lab 7: Mongo/MERN --- Data Representation and Querying

The following exercises focus on utilizing MongoDB within a MERN stack.

### Instructions

1. **Commit and push each solution to GitHub after completing an exercise.**
  

2. **React Application Setup**
   - If you haven’t completed the previous lab, clone the React application from GitHub:
     ```bash
     git clone https://github.com/Data-Rep-MERN-Application/lab_six
     ```
   - Install project dependencies:
     ```bash
     npm install
     ```

3. **MongoDB Setup**

   **NoSQL** is a type of database that provides a flexible and scalable alternative to traditional relational databases. Unlike SQL databases, which rely on tables and structured schemas, NoSQL databases store data in various formats, such as documents, key-value pairs,       graphs, or wide-column stores. This flexibility makes NoSQL databases ideal for handling large volumes of unstructured or semi-structured data and allows for more dynamic scaling, especially for applications with rapidly changing data needs.

   **MongoDB** is a popular NoSQL database known for its document-based storage model. Rather than using rows and tables, MongoDB stores data in JSON-like documents, where each document is a self-contained object with key-value pairs. This model is both flexible and     
   powerful, allowing for complex, nested data structures and rapid data retrieval. MongoDB’s scalability and ease of use make it well-suited for modern web applications, especially those built with the MERN stack (MongoDB, Express, React, Node.js).

   - (a) Create a free MongoDB account and cluster at [MongoDB Atlas](https://www.mongodb.com/).
   - (b) Allow all IP addresses to connect.
   - (c) Create a simple user (e.g., `admin` with password `admin`) for database access.

4. **Database Connection with Mongoose**

   Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js. It provides a schema-based solution for modeling application data, allowing developers to enforce structure, handle data relationships, and perform CRUD operations (Create, Read, Update, Delete) more easily. By defining models for each data type, Mongoose ensures data consistency and helps simplify database interactions.
   - Install the Mongoose library:
     ```bash
     npm install mongoose
     ```
   - Connect to MongoDB in `server.js`:
     ```javascript
     const mongoose = require('mongoose');
     mongoose.connect('my_db_connection_string');
     ```

5. **Create a Data Model**

      In Mongoose, a **data model** is a blueprint for defining the structure of data within a MongoDB collection. Models are created from **schemas**, which specify the fields, data types, and constraints for each document in a collection. This schema-based approach 
      ensures consistency in the way data is stored and accessed, making it easier to validate and manage data across an application.

      For example, defining a schema for a "Movie" model allows you to enforce specific fields like title, year, and poster for each movie document. By setting up a model in Mongoose, you can use it to create, read, update, and delete documents in MongoDB, with Mongoose handling many details behind the scenes.
    - Define schema and data model:
     ```javascript
       const movieSchema = new mongoose.Schema({
         title: String,
         year: String,
         poster: String
       });
      
       const Movie = mongoose.model('Movie', movieSchema);
     ```

6. **Add Data to MongoDB**
   - Create a method to add new movie records:
     ```javascript
      app.post('/api/movies', async (req, res)=>{

      const { title, year, poster } = req.body;

      const newMovie = new Movie({ title, year, poster });
      await newMovie.save();
  
      res.status(201).json({ message: 'Movie created successfully', movie: newMovie });
      })
     ```
     
   **Explanation**:
   - **async** and **await** are used to handle asynchronous operations like saving data to a database.
   - async allows us to use await, which pauses the function until the operation completes. Here, await newMovie.save() ensures the movie is saved to the database before continuing, making the code easier to read and manage.  
   - `app.post('/api/movies', ...)`: This sets up a POST route at `/api/movies`, which will be used to add new movies.
   - We extract `title`, `year`, and `poster` from `req.body`, the data sent in the POST request.
   - A new `Movie` instance is created using `new Movie({ title, year, poster })`.
   - `newMovie.save()` saves the new movie to MongoDB, and a success response is sent with the movie data.


7. **Retrieve All Data**
   - Implement a method to fetch all movie records:
     ```javascript
     app.get('/api/movies', async (req, res) => {
       const movies = await Movie.find({});
       res.json(movies);
     });
     ```
     
   **Explanation**:
   
   - `app.get('/api/movies', ...)`: Sets up a GET route at `/api/movies`, which will return all movies.
   - `Movie.find({})` is called. The empty object `{}` as an argument means it fetches all documents in the `movies` collection.
   - `res.json(movies)` sends the retrieved movies in JSON format back to the client.


8. **Retrieve Data by ID**
   - Create a method to retrieve a specific movie by its ID:
     ```javascript
     app.get('/api/movie/:id', async (req, res) => {
       const movie = await Movie.findById(req.params.id);
       res.send(movie);
     });
     ```
     
    **Explanation**:
  
     - `app.get('/api/movie/:id', ...)`: Defines a GET route at `/api/movie/:id`, where `:id` is a parameter for the movie’s unique ID.
     - `Movie.findById(req.params.id)`: This method searches the `movies` collection for a document with the ID provided in the URL.
     - If a movie is found, it’s sent back in JSON format.
"# lab8_data_rep" 
