# MEAN
Mongo, Express, Angular and Node.

## Separation of Concerns
 * Data Storage: Mongo
 * HTTP Routing, Request, Response: Express
 * Browser Front End: Angular
 * Runtime: Node

## Node.js

JavaScript, event-driven, non-blocking I/O.

    const fs = require('fs');

    fs.readFile('/path/to/file.txt', (err, data) => {
      if (err) return console.error('Error reading file', err);
      console.log('File contents', data);
    });

### Non-blocking I/O

    console.log('Before');

    fs.readFile('/path/to/file.txt', (err, data) => {
      console.log('In callback');
    });

    console.log('After');

    // Output
    > Before
    > After
    > In callback

### CommonJS Module System

Node.js supports CommonJS modules through the builtin `require()` funciton.

    const fs = require('fs');

To create a module, assign a value to `module.exports`. E.g.

    // add.js
    module.exports = (x,y) => {
      return x+y;
    }

    // myapp.js
    const add = require('./add.js');
    console.log(add(2, 3)); // => 5

### NPM - Node Package Manager

  * Package manager for Node.js
  * Ships with Node
  * ~300k packages

## Express Quick Start

Assuming you already have Node.js and npm installed, install the `express-generator` package
and create a new application.

    $ npm install -g express-generator
    $ express bookstore

This will create a skeleton framework for the **bookstore** application. Change into the
application directory, install dependencies and start the app.

    $ cd bookstore
    $ npm install
    $ DEBUG=bookstore:* npm start

## Controller: Express Routing

The entry point to your application is `index.js`. Here the app is created, and routes are specified

    const app = express();
    app.get('/about', (req, res, next) => {
      res.send('<h1>Bookstore 1.0</h1>');
    });

Routes can be modularized. In index.js

    var routes = require('./routes/index');
    var users = require('./routes/users');
    // ...
    app.use('/', routes);
    app.use('/users', users);

Store your routes as modules in `./routes`. Here is `./routes/index.js`.

    var express = require('express');
    var router = express.Router();

    /* GET home page. */
    router.get('/', function(req, res, next) {
      res.render('index', { title: 'Express' });
    });

    module.exports = router;


## View: Server side templates

The `app.render` function used in our `index.js` looks in a static view directory that
has been configured on the app. Templating is specified with `'view engine'`.

    // view engine setup
    app.set('views', path.join(__dirname, 'views'));
    app.set('view engine', 'jade');

## Templating with Jade

Express provides a several templating engines. Jade is the default. Here is a
basic layout generated in `./views/layout.jade`.

    doctype html
    html
      head
        title= title
        link(rel='stylesheet', href='/stylesheets/style.css')
      body
        block content

## Angular Front End

  * Insert some content here - PH todo


## Model: MongoDB

  * Document based storage
  * JSON is lingua franca
  * Natural fit with JavaScript

Install the `mongodb` module and save to your `package.json`.

    $ npm install --save mongodb

## Mongo Example

Obtain a `MongoClient`, and connect to the mongo server. To get a listing of all books of
fiction in our hypothetical data store, let's create a function.

    function getFiction(cb) {
      var MongoClient = require('mongodb').MongoClient;
      MongoClient.connect('mongodb://localhost:27017/books', function(err, db) {
        if (err) {
          return cb(err);
        }
        db.collection('fiction').find().toArray(function(err, result) {
          if (err) {
            return cb(err);
          }
          cb(null, result);
        });
      });
    }

Now we can use this in a route.