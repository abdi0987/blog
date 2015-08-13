---
title:  "Building A Web App Part 1 REST API"
description: "Build A Todo Web App With The MEAN stack"
## date: add a date when publishing
---

In this series we will be building a to-do app using the MEAN Stack  We will build a rest api , design the page , and connect or frontend and backend

Today we’ll be looking at creating a RESTful API using Node, Express 4 and its Router, and Mongoose to interact with a MongoDB instance. We will also be testing our API using Postman in Chrome.

Let’s look at the API we want to build and what it can do.

##Setup

Install express-generator , scaffold a basic express app and install mongoose

Install express-generator with npm
```
npm install express-generator
```

Scaffold a web app

```
express todoApp
```

Install Mongoose
```
npm install mongoose
```

##Our application

* Handle CRUD for every todo item
* Return json data
* Make it RESTful (GET, POST, PUT, and DELETE)


## Getting started

This is a broad look at how our dir should look (it does not include all the file you should have)

```
  - todoApp/
    ----- bin/
    ----- models/ // Create a folder model
    ---------- todo.js  // and create a todo.js file
    ------ node_modules
    ------ public
    ------ routes
    ---------- todo.js // Create a todo.js file inside the routes 
    ------ views
    ------ app.js
```

After you created the models folder and todo.js file. Inside the routes folder remove the index.js and user.js file. Then create a folder called todo.js

Since we changed some file we need to change the app.js file.
At the top of the file we need to require mongoose and todo.js route , and add the mongoose uri by using creating your own database or you mongolab or any other service
Check the comments to see what we need to change

```javascript

var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');
var mongoose = require("mongoose"); // Require mongoose

/* Delete the index and user variable */
var todo = require('./routes/todo'); // Add this

var app = express();

mongoose.connect('mongodb://abdi0987:hassan22@ds059682.mongolab.com:59682/story') // Add mongoose uri


// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'hbs');

// uncomment after placing your favicon in /public
//app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/api', todo); // Define our api route

// catch 404 and forward to error handler
app.use(function(req, res, next) {
  var err = new Error('Not Found');
  err.status = 404;
  next(err);
});

// error handlers

// development error handler
// will print stacktrace
if (app.get('env') === 'development') {
  app.use(function(err, req, res, next) {
    res.status(err.status || 500);
    res.render('error', {
      message: err.message,
      error: err
    });
  });
}

// production error handler
// no stacktraces leaked to user
app.use(function(err, req, res, next) {
  res.status(err.status || 500);
  res.render('error', {
    message: err.message,
    error: {}
  });
});


module.exports = app;
```

## Database model

Open the todo.js file  and inside model folder Define a model

```javascript
var mongoose = require('mongoose');
var Schema = mongoose.Schema;

var TodoSchema = new Schema({
    text:String,
    completed:Boolean
})

module.exports = mongoose.model('Todo',TodoSchema);
```

We will use an instance of the Express Router to handle all of our routes. Here is an overview of the routes we will require, what they will do, and the HTTP Verb used to access it.

```
/* Do not type this any where!!!!!!!!!!!!! */

/api/todo : GET //Get all the to do items
/api/todo : POST //Add a new to do item
/api/todo/todo_id : GET // Get a specific to do item
/api/todo/todo_id : PUT // Update a to do item
/api/todo/todo_id : Delete // Delete a single to do item
```

## Routes

Lets define the post route to add a new to do item 

Open the todo.js file in the routes folder and add this

```javascript
    var express = require('express');
    var Todo = require('../models/todo');
    var router = express.Router();

    //Get post a new item
    router.route('/todo')
        .post(function(req, res) {
            var todo = new Todo(); // Create a new instance of todo

            todo.text = req.body.text; // Set the text 
            todo.completed = false; 

            //Save the todo item
            todo.save(function(err) {
                //error handeling 
                if (err)
                    res.send(err)
                    
                //Return a message
                res.json({
                    message: "Todo created"
                });
            })
        })
```
We will use Express’s router.route() to handle multiple routes for the same URI. This keeps our application clean and organized.


Test our route in post mate. This is how it should look.

![alt text](/assets/images/pos.PNG "Logo Title Text 1")

Remeber to check the x-www-form-urlencoded button so that is sends the data to node server as query strings




