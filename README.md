# NodeJS
# Docs Reference: Books read

These are references from the topics I've considered important from differents book I've read so far. 

 ### Node.js complete reference guide

* HTTP Server and Client objects
    * HTTP Client request
    * Calling a REST backend service for heavy computational tasks

* Express
    * Promises and Async functions
    * MVC Paradigm
    * CRUD
    * Scaling up (running multiple servers)

* Mobile-first paradigm
    * Twitter Boostrap
    * Flexbox (12 column grid)

* Logging and debugging
    * Logger (`morgan`)
    * `rotating-file-stream` package
    * `debug` package
    * `uncaughtExceptions`
    * handle `unhandledPromise` with `util`

* CommonJS Modules vs ES6 Modules
    * `app.js` vs `app.mjs`
    * `require` vs `import`
    * `__dirname` vs `import.meta.url`
    * `bin/www` as an ES6 Module

 ##### Data Storage and Retrieval

* Data Storage and Retrieval
    * filesystem
        * Storage in `JSON`
        * `fs-extra` moule, Promised-based function to the `fs` module
        * `get JSON()` and `JSON.stringify(object)`
        * `static fromJSON(json)` and `JSON.parse(json)`
    * Dynamic import of ES6 Modules
        * `require(process.env.NOTES_MODEL ? path.join('.., ...) : 'models'/notes-memory)`
        * `import()` function that returns a Promise that will resolve to the imported module
    * LevelUP
        * Node.js-friendly wrapper around the LevelDB engine by Google (local NoSQL data storage in web browsers)
    * SQLite3
        * Schemas
        * `"sqlite3-setup": "sqlite3 dbname.sqlite3 --init models/schema.sql"`
    * ORM - Sequelize

* SQLite3
    * PROS
        * Lightweight (doesn't require a server)
        * Self-contained
        * No-set-up-required, just `npm install sqlite3`

* MongoDB
    * JSON Documents
    * Mongoose ORM

 ##### Authentication

* `express-session` middleware
* PassportJS
* Restify: `restify-server` `restify-client` (doesn't support a Promise-oriented API)
* Superagent: `superagent` (support for Promise-orientd API)
* Supertestmodels

---
# Introduction: Node
Node.js is a JavaScript runtime built on [Chrome's V8 Javascript engine](https://v8.dev/).

* Asynchronous promised-based style
* Event-driven (Call Stack, Callback Queue, and Event Loop)
* Non-Blocking I/O


 ### Under the hood

 #### V8 Engine

* JavaScirpt is a high-level language 
* Compiler turns high level code into machine code
* Why V8 Engine instead of compiler?
    * __Just-in-Time__ Compiler
    * Pros of both interpreted an compiled languages
    * Written in C++

---

 ### What makes Node so great?

 #### Event-driven, non-blocking I/O model

What is __event-driven programming__? it's a computer programming paradigm in which control flow of the program is determined by the occurrence of events. These events are monitored by code known as an event listener that, if it detects that its assigned event has occurred, runs an event “handler”, typically a callback function or method. This handler deals with the event by responding to it with program code. The responses only run once we get a request — __that’s__ event driven programming. For this, Node provides the __event loop__. 

Blocking methods execute synchronously and __non-blocking methods execute asynchronously__.

---

 #### The Event Loop

The event loops result in callback type programming. In simple terms it’s where you end up splitting your program in to smaller & smaller chunks until each chunk is mapped to operation with data.

 ##### Promises style programming

When a function will return a promise object that will return something in the future. It promises that it will do something for you. You can also chain promises together and it really helps simplify your code.

 ##### Event emitters 

An Event emitter, as it sounds, is just something that triggers an event to which anyone can listen. Different libraries offer different implementations and for different purposes, but the basic idea is to provide a framework for issuing events and subscribing to them.

In node.js an event can be described simply as a string with a corresponding callback. An event can be “emitted” (or in other words, the corresponding callback be called) multiple times or you can choose to only listen for the first time it is emitted.

---

 ##### Call Stack 

* Javascript goes through a program sequentially
* Flow of execution is synchronous
    * May delay execution of other functions 

 ##### Node API 

* Node.js does __support asynchronous behavior__.
* Asynchornous event-based system 
* Call stack is used for the debugging stack trace 

---

 ### Templating 

 #### Middlewares

Middleware is any number of functions that are invoked by the Express.js routing layer before your final request handler is, and thus sits in the middle between a raw request and the final intended route. It basically lets you configure how your express application works.

--- 


 ### PG (Postgres)

```
$ npm install pg --save
```

Configuring Database Pool
# Introduction: Nodemon
[Nodemon](https://nodemon.io/) is a utility that will monitor for any changes in your source and automatically restart your server. Perfect for development. Install it using npm.

Just use nodemon instead of node to run your code, and now your process will automatically restart when your code changes. To install, get node.js, then from your terminal run:

```bash
$ npm install -g nodemon
```

 #### Basic setup

Install
``` 
npm i nodemon --save-dev
```

Add to package.json file 

```
"scripts": {
    "start": "node .",
    "dev": "nodemon ."
}

"DevDependecies": {
    "nodemon": <VERSION>
}
```

Run dev enviroment
```
npm run dev
```
# Introduction: Debugging
Use the `debbuger` statement as breakpoints and go to `chrome://inspect`.
# Introduction: Express
Express is a minimal and flexible Node.js web application framework that provides a robust set of features for Web Applications and APIs.
# Introduction: RESTful frameworks
[RESTIFY](http://restify.com/):

A Node.js web service framework optimized for building semantically correct RESTful web services ready for production use at scale. restify optimizes for introspection and performance, and is used in some of the largest Node.js deployments on Earth.

[LOOPBACK](https://loopback.io/)

Offering from StrongLoop, the current sponsor of the Express project. It offers a lot of features and is, of course, built on top of Express.
# Introduction: Boilerplates setup
Go ahead and start coding with these templates

* [MERN](https://medium.com/javascript-in-plain-english/full-stack-mongodb-react-node-js-express-js-in-one-simple-app-6cc8ed6de274)

* [Express + React](https://github.com/bradtraversy/react_express_starter)

* [Express + React + Redux](https://github.com/bradtraversy/react_redux_express_starter)
# Introduction: Why and how to use Linter
This [note in Medium](https://medium.com/the-node-js-collection/why-and-how-to-use-eslint-in-your-project-742d0bc61ed7) will probably explain it better than me!
# Introduction: MVC
__Model__ hold the application data, changing it as instructed by the __Controller__ that coordinates (routing) the response for each URL, and supplying data requested by the __View__.

The API of the modules in the models direcotry will provide functions to Create, Read, Update and Delete/Destroy data items (__CRUD__), the four basic operations of persistent data storage, and other functions necessary for the View code to do its thing. 
# Introduction: Backend Architecture Introduction
The front-end is the code that is executed on the client side. This code (typically HTML, CSS, and JavaScript) runs in the user’s browser and creates the user interface.

The back-end is the code that runs on the server, that receives requests from the clients, and contains the logic to send the appropriate data back to the client. The back-end also includes the database, which will persistently store all of the data for the application. This article focuses on the hardware and software on the server-side that make this possible.

Review [REST and HTTP](Software-Theory.md#rest) if you want to refresh your memory on these topics. These are the main conventions that provide structure to the request-response cycle between clients and servers.

Let’s start by reviewing the __client-server relationship__.

 ### What are the clients?
The clients are anything that send requests to the back-end. They are often browsers that make requests for the HTML and JavaScript code that they will execute to display websites to the end user. However, there many different kinds of clients: they might be a mobile application, an application running on another server, or even a web enabled smart appliance.

 ## What is a back-end?
The back-end is all of the __technology required to process the incoming request and generate and send the response to the client__. This typically includes three major parts:

 ### The server 

This is the computer that receives requests. Though there are machines made and optimized for this particular purpose, any computer that is connected to a network can act as a server.


 ### The app 
This is the application running on the server that listens for requests, retrieves information from the database, and sends a response. It contains logic about how to respond to various requests based on the __HTTP verb__ and the __URI__ (Uniform Resource Identifier). The pair of both of them is called a __route__ and matching them based on a request is called __routing__.

Some of these handler functions will be __middleware__. In this context, middleware is any __code that executes between the server receiving a request and sending a response__. These middleware functions might modify the request object, query the database, or otherwise process the incoming request. Middleware functions typically end by passing control to the next middleware function, rather than by sending a response.

Eventually, a middleware function will be called that ends the request-response cycle by sending an HTTP response back to the client.

Often, programmers will use a framework like Express or Ruby on Rails to simplify the logic of routing. For now, just think that each route can have one or many handler functions that are executed whenever a request to that route (HTTP verb and URI) is matched.

 ###The database

Databases provide an interface to save data in a persistent way to memory. Storing the data in a database both reduces the load on the main memory of the server CPU and allows the data to be retrieved if the server crashes or loses power

Many requests sent to the server might require a database query. A client might request information that is stored in the database, or a client might submit data with their request to be added to the database.

---

 ## What kinds of responses can a server send?
The data that the server sends back can come in different forms. For example, a server might serve up an HTML file, send data as JSON, or it might send back only an HTTP status code. You’ve probably seen the status code “404 - Not Found” whenever you’ve tried navigating to a URI that doesn’t exist, but there are many more status codes that indicate what happened when the server received the request.


 ## What is a Web API, really?
An API is a collection of clearly __defined methods of communication between different software components__.

More specifically, a Web API is the interface created by the back-end: the __collection of endpoints and the resources these endpoints expose__.

A Web API is defined by the types of requests that it can handle, which is determined by the routes that it defines, and the the types of responses that the clients can expect to receive after hitting those routes.

One Web API can be used to provide data for different front-ends. Since a Web API can provide data without really specifying how the data is viewed, multiple different HTML pages or mobile applications can be created to view the data from the Web API.

---

 ## Other principles of the request-response cycle

* The server typically cannot initiate responses without requests!
* Every request needs a response, even if it’s just a 404 status code indicating that the content was not found. Otherwise your client will be left hanging (indefinitely waiting).
* The server should not send more than one response per request. This will throw errors in your code.


 ###  Example: Mapping out a request
Let’s make all of this a bit more concrete, by following an example of the main steps that happen when a client makes a request to the server.

1. Alice is shopping on SuperCoolShop.com. She clicks on a picture of a cover for her smartphone, and that click event makes a GET request to http://www.SuperCoolShop.com/products/66432.

Remember, GET describes the kind of request (the client is just asking for data, not changing anything). The URI (uniform resource identifier) /products/66432 specifies that the client is looking for more information about a product, and that product, has an id of 66432.

SuperCoolShop has an huge number of products, and many different categories for filtering through them, so the actual URI would be more complicated than this. But this is the general principle for how requests and resource identifiers work.

2. Alice’s request travels across the internet to one of SuperCoolShop’s servers. This is one of the slower steps in the process, because the request cannot go faster than the speed of light, and it might have a long distance to travel. For this reason, major websites with users all over the world will have many different servers, and they will direct users to the server that is closest to them!

3. The server, which is actively listening for requests from all users, receives Alice’s request!

4. Event listeners that match this request (the HTTP verb: GET, and the URI: /products/66432) are triggered. The code that runs on the server between the request and the response is called middleware.

5. In processing the request, the server code makes a database query to get more information about this smartphone case. The database contains all of the other information that Alice wants to know about this smartphone case: the name of the product, the price of the product, a few product reviews, and a string that will provide a path to the image of the product.

6. The database query is executed, and the database sends the requested data back to the server. It’s worth noting that database queries are one of the slower steps in this process. Reading and writing from static memory is fairly slow, and the database might be on a different machine than the original server. This query itself might have to go across the internet!

7. The server receives the data that it needs from the database, and it is now ready to construct and send its response back to the client. This response body has all of the information needed by the browser to show Alice more details (price, reviews, size, etc) about the phone case she’s interested in. The response header will contain an HTTP status code 200 to indicate that the request has succeeded.

8. The response travels across the internet, back to Alice’s computer.

9. Alice’s browser receives the response and uses that information to create and render the view that Alice ultimately sees!

# Fundamentals: Testing Node
We will use [JEST Framework](https://jestjs.io/) to build our Test Suites and [SuperTest package](https://www.npmjs.com/package/supertest) to work with http requests.

Run script:
```
"test": "env-cmd -f ./config/dev.env jest --watch --runInBand"
```

Flag `--watch` to listen for changes in tests, and `--runInBand` to run test suites isolated and avoid conflicts.

 ### Test cases 

Blueprint test case
```
test('Should convert 0C to 32F', () => {
    const temp = celciusToFahrenheit(0);
    expect(temp).toBe(32);
})
```
Blueprint ASYNC code
```
test('Async test demo', (done) => {
  setTimeout(() => {
    expect(1).toBe(2);
    done()
  }, 2000)
});
```

```
test('Async test demo2', (done) => {
  add(2, 3).then((sum) => {
    expect(sum).toBe(5);
    done()
    })
});
```
# Fundamentals: Middlewares
We get access to use middlewares with `app.use(<middleware>)`

[CHECK EXPRESS API DOCS FOR BUILT-IN MIDDLEWARES!](https://expressjs.com/en/api.html)

Example: 

```
app.use((req, res, next) => {
  if (req.method === 'GET') {
    res.send('GET Requests are disabled')
  } else {
    next()
  }
});
```

---

 ### express.json

```
app.use(express.json());
```
Parse the request body in JSON format as an object, so you can access it using `console.log(req.body)`.
# Fundamentals: Upload files
We will use [MULTER library](https://www.npmjs.com/package/multer) to actually upload files. There's two different approachs for this:

* Save files to your working directory
* Save files as binary data (Buffer) and manually serve them as images

The second approach works better as we can just save the binary raw string in our database, and data will be safer. 

We will also use [SHARP library](https://www.npmjs.com/package/sharp) to convert large images in common formats to smaller, web-friendly JPEG, PNG and WebP images of varying dimensions.
# Fundamentals: Authentication
Tools we use:

* `express-session` middleware: Cache sessions with broser's cookies
* PassportJS: third-party services like Twitter or Facebook
* Restify `restify-server` `restify-client`: for authentication with a RESTful api, doesn't support a Promise-oriented API
* Superagent: `superagent`: to allow asynchronous, promise-based authentication (support for Promise-orientd API)
* Supertestmodels: Superagent companion for unit-testing

You should check documentation for further details, we will only serve particular examples to get a common authentication service up and running but you might need special configurations depending on business logic.

 ### Bcrypt - Password Storage

We should never store passwords as plain text. We'll use [BCRPT Library](https://www.npmjs.com/package/bcrypt) to encode passwords using hashes.

```
npm i bcrypt
```

 ### JSON Web Tokens 

[JWT Library](https://www.npmjs.com/package/jsonwebtoken): manage authenication tokens for active sessions.

```
npm i jsonwebtoken
```

 ### Express Middleware

* Without middleware: new request -> run route handler
* With middleware: new request -> do something -> run route handler
# Fundamentals: Common HTTP Req and Res
Full list [HERE!](https://httpstatuses.com/)

 #### Most common status code 

* __OK__ - 200 
* __Bad Response__ - 404

 #### CRUD Status Codes

* __GET__
    * __OK__ - 200
    * __Bad Response__ - 404

* __POST__
    * OK - 201 + New Object
    * Error - 400 (object not valid)

* __PUT__
    * Ok - 201 + Updated Object
    * __Bad Response__ - 404

* __ DELETE __
    * Ok - 204 + Deleted object id
    * __Bad Response__ - 404 (Request with invalid id)
# Fundamentals: Sending Emails
When a user registers, we'd like to be able to send emails confirming that they did register for example, or maybe just send them institutional news. 

For this we will be using [SendGrid API](https://sendgrid.com/).
# Fundamentals: Environment Variables
Keep production and development environment variables separeted and secured using `.env` files in a `config` folder. 

We will be using [env-cmd Library](https://www.npmjs.com/package/env-cmd) for executing command using an environment from an env file.
# Fundamentals: Heroku Deployment
Basic steps for heroku deployment

Create app
```
heroku create <unique_name>
```

Environment variables
```
heroku config:set key=value
heroku config:unset key
heroku config
```

Take in mind that `PORT` environment variable is handled by heroku and you don't need to specify it.

# Intermediate - Socket.IO and WebSockets
[Socket.IO](https://socket.io/) is a JavaScript library for __realtime__ web applications. It enables realtime, __bi-directional communication__ between web clients and servers. It has two parts: a client-side library that runs in the browser, and a server-side library for Node.js. Both components have a nearly identical API. Like Node.js, it is event-driven.

Socket.IO primarily uses the WebSocket protocol with polling as a fallback option, while providing the same interface. Although it can be used as simply a wrapper for WebSocket, it provides many more features, including broadcasting to multiple sockets, storing data associated with each client, and asynchronous I/O.

# Intermediate: Chat libraries
* [Mustache](https://cdnjs.com/libraries/mustache.js/): Render templates

* [Moment](https://cdnjs.com/libraries/moment.js/): Manipulate time

* [QS](): Query String - Set up room and user names

CDN
```
https://cdnjs.cloudflare.com/ajax/libs/mustache.js/3.0.1/mustache.min.js
https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.24.0/moment.min.js
https://cdnjs.cloudflare.com/ajax/libs/qs/6.7.0/qs.min.js
```

# Intermediate: WebSocket Protocol
* Websockets allow for __full-duplex communication__. Server can also iniciate communication (request) to clients, without a request from a client before. Client connects to the server and remains connected as long as needed.

* __Separate protocol__ from HTTP.

* __Persistent connection__ between server and client.

