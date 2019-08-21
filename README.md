# NodeJS
# Index
This notebook is a reference guide for backend development using Node. If you are learning from scratch, you can follow this sequentally: read introduction first, then fundamentals topics, and intermediate. If you are here just trying to learn something new or looking for an specific topic, feel free to use the index and go to the section that better suits your needs, as each module is intended to be isolated and self-explained, the order is just an arbitrary recomendation.

The `book` tag you'll find in some sections, is for sections that serve as a 'index' for the main topics covered in the correspondent book. Those sections will be listed first. 

Book references
* [Book: NodeJs Complete Reference Guide](#book--nodejs-complete-reference-guide)

Projects Reference
* [Fundamentals: Reference projects](#fundamentals--reference-projects)

Introduction
* [Introduction: Node](#introduction--node)
* [Introduction: Node Internals](#introduction--node-internals)
* [Introduction: Nodemon](#introduction--nodemon)
* [Introduction: Debugging](#introduction--debugging)
* [Introduction: Express](#introduction--express)
* [Introduction: RESTful frameworks](#introduction--restful-frameworks)
* [Introduction: REST](#introduction--rest)
* [Introduction: Boilerplates setup](#introduction--boilerplates-setup)
* [Introduction: Why and how to use Linter](#introduction--why-and-how-to-use-linter)
* [Introduction: MVC](#introduction--mvc)
* [Introduction: Backend Architecture Introduction](#introduction--backend-architecture-introduction)

Fundamental level
* [Fundamentals: Testing](#fundamentals--testing)
* [Fundamentals: Testing Node](#fundamentals--testing-node)
* [Fundamentals: Middlewares](#fundamentals--middlewares)
* [Fundamentals: Upload files](#fundamentals--upload-files)
* [Fundamentals: Authentication](#fundamentals--authentication)
* [Fundamentals: Common HTTP Req and Res](#fundamentals--common-http-req-and-res)
* [Fundamentals: Sending Emails](#fundamentals--sending-emails)
* [Fundamentals: Environment Variables](#fundamentals--environment-variables)
* [Fundamentals: Deploying Apps](#fundamentals--deploying-apps)
* [Fundamentals: Heroku Deployment](#fundamentals--heroku-deployment)

Architectural Patterns
* [Intermediate: Architectural Patterns](#intermediate--architectural-patterns)

Intermediate level
* [Intermediate - Socket.IO and WebSockets](#intermediate---socket-io-and-websockets)
* [Intermediate: Chat libraries](#intermediate--chat-libraries)
* [Intermediate: WebSocket Protocol](#intermediate--websocket-protocol)
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
* Asynchronous event-based system 
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
# Introduction: Node Internals
    The two most important Node dependencies:

* V8 Engine Project
    * C++ and Javascript
    * Run Javascript outside of browser
* libuv Project
    * 100% C++ Open-source
    * Access to file-system
    * Access to networking
    * Concurrency

Node is written in C++ and Javascript, that provides a series of wrappers (Standard Library) like `http`, `fs`, `crypto`, and `path`, as APIs for the V8 and libuv implementations. 

 #### Node execution hierarchy

* Javascript code we write
* Node's Javascript Side (lib folder in Node repo)
    * `process.binding()`: Connects JS and C++ functions
    * __V8__: Converts values between JS and C++ world
* Node's C++ Side (src folder in Node Repo)
* __libuv__: Gives Node easy access to underyling OS

 ## Threads

Process: instance of a computer program that is being executed. Between a single process we can have multiple threads, that work as simple to-do lists that are run one by one.

`OS Scheduler` decides which thread should be processed.

# Introduction: Event Loop
Pseudocode idea

```javascript
// Node starts

const pendingTimers = [];
const pendingOSTasks = [];
const pendingOperations = [];

// New timers, tasks, operations are recorded from myFile running
myFile.runContents();

function shouldContinue() {
    // Check one: Any pending setTimeout, setInterval, setImmediate
    // Check two: Any pending OS tasks (like server listening to port)
    // Check three: Any pending long running operations (like fs module)

    return pendingTimers.length || pendingOSTasks.length || pendingOperations.length
}

// Entire body executes in one 'tick'
while(shouldContinue()) {
    // 1) Node looks at pendingTimers and sees if any functions
    // are ready to be called (setTimeout, setInterval)

    // 2) Node looks at pendingOSTasks and pendingOperations (threadpool)
    // and calls relevant callbacks

    // 3) Pause execution. Continue when...
    // - a new pendingOSTask is done
    // - a new pendingOperation is done
    // - a timer is about to complete

    // 4) Look at pendingTimers (setImmediate)

    // 5) Handle any 'close' events
}

// Exit back to terminal
```

---

 ### Single Threaded?

Node Event Loop is __Single Threaded__, but some of Node Framework/Std Lib is not and is not executed inside that single thread. We can do other things insde the event loop while those calculations happen.

 ### Libuv Thread Pool

For some of the standard library function calls, the Node C++ side and Libuv __do expensive and heavy calculations tasks totally outside the event loop__. There are 4 other threads on the Libuv __Thread pool__ that can be used.

All `fs` module functions, and some of crypto use it. Depends on OS (Windows vs UNIX based).

_Change the pool size_:
```javascript
process.env.UV_THREADPOOL_SIZE = 2;
```

Libuv can delegate operations to our OS that doesn't use the Thread Pool. (OS Async Helpers)

For example, with the `https` module, request are made by the OS, and there's no blocking of the event loop. OS decides how to use threads. Almost everything around networking for all OS's use the OS's async features.

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
# Introduction: REST
We will cover the following topics:

* __Representational State Transfer__ fundamentals
* REST with HTTP
* Essential differences compared to classical __SOAP__ based services (Simple Object Access Protocol)
* Taking advange of existing infrastructure 

 ### REST Fundamentals

Back in 1999, Roy Fielding defined a set of principles built around the HTTP and URI standards that give birth to REST. 

Let's look at the key principles around the HTTP and URI standards, sticking to which will make your HTTP application a RESTful service-enabled application:

1. Everything is a resource
2. Each resource is identifiable by a __unique identifier (URI)__
3. Resources are manipulated via standard HTTP methods
4. Resources can have multiple representations
5. Communicate with resources in a stateless manner

The native HTTP protocol (RFC 2616) defines eight actions, also known as HTTP verbs
* GET
* POST
* PUT
* DELETE
* HEAD
* OPTIONS
* TRACE
* CONNECT

See [Fundamentals: Common HTTP Codes](#fundamentals--common-http-req-and-res).

Resource manipulation operations through HTTP requests should always be considered atomic. All modifications of a resource should be carried out within an HTTP request in an isolated manner. After the request execution, the resource is left in a final state, this implicitly means that partial resource updates are not support. You should always send the complete state of the resoure.

Another requirement for your RESTful application to be stateless is that once the service gets deployed on a production enviornment, it is likely that incoming requests are serverd by a load balancer, ensuring scalability and high availability. Once exposed via a load balancer, the idea of keeping your application state at server side gets compromised. You should keep it in a RESTful way, for example, keep a part of the state within the URI, or use HTTP headers to provide additional state-related data.

The statelessness of your RESTful API isolates the caller against changes at the server side. Thus, the caller is not expected to communicate with the same server in consecutive requests. This allows easy application of changes within the server infrastructure, such as adding or removing nodes.

 ### REST Goals

* Separation of the representation and the resource
    * Multiple representations available. It's up to the caller to specify the desired media type and then it's up the server application to handle the representation accordingly
* Visibility
    * Every aspect of it should be self-descriptive and follow the natural HTTP language
* Reliability
    * Which HTTP method are safe and indepmpotent in the REST context
* Scalability
    * Stateless is crucial, as scalign your application would require you to put another piece of hardware for a load balancer, or bring another instance in your cloud environment
    * Is all about serving all your clients in an acceptable amount of time and keep your application running, preventing Denial of Service (DoS) caused by a huge amount of incoming requests
* Performance
    * Time needed for a single request to be processed

 #### Safe HTTP Methods

Considered to be safe provided that, when requested, it does not modify or cause any side effects on the state of the resource

* GET

 #### Idempotent HTTP Methods

Consdired to be idempotent if its response stays the same, regardless of the number of times it is requested. 

* GET
* PUT
* DELETE

 ### Working with WADL

Description language called __Web Application Definition Language__. This is an optional XML description of the interface of the service and defines an endpoint URL for invocation. Similar to WSDL for SOAP web services.

 ### Documenting with Swagger

Public APIs exposed on the web should be well documented. The [Swagger project](https://swagger.io/) addresses the ned for neat documentation of RESTful APIs. It defines a meta description of an API from an almost human-readable JSON format within a `swagger.json` file.
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

# Fundamentals: Testing
__Unit testing__: each unit is tested separately, isolating the unit under test as much as possible from other parts of the applications. A common technique is to use mock objects or mock data to isolate individual parts of the applications from one another. Usually __performed by developers__.

__Functional testing__: doesn't try to test individual components, but instead it tests the whole system. Usually __performed by QA or QE team__ (QUality Assurance and Quality Engineering). 

 #### Assert - the basis

Node.js has a useful built-in testing tool, the `assert` module.

 ### Mocha and Chai

[Mocha](https://mochajs.org/) is one o many test frameworks available for Node.js. It help us write test cases and test suites, and it provides a test results reporting mechanism. It was chosen over the alternatives vecause it supports Promises. It fits very well with the Chai assertion library.

Mocha requires that tests be CommonJS modules, a module `esm` exists to load an ES6 module into a CommonJS module.

In `test` directory create a file named `test-model.js` containing this as the outher shell of the test suite:

```javascript
'use strict';

require = require('esm')(module, {'esm':'js'});
const assert = require('chai').assert;

descrie('Model test', function () {
    // ...
});
```
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
# Fundamentals: Reference projects
The following projects will serve you as reference for directory structure and tests.

* [Task-manager](https://github.com/herrera-ignacio/task-manager)
* [Auth-server](https://github.com/herrera-ignacio/auth-server)
* [Chat-app](https://github.com/herrera-ignacio/chat-app)
# Fundamentals: Deploying Apps
We'll cover the following:

* Traditional LSB-compilant Node.js deployment
* Using PM2 to improve reliability
* Deployment to __Virtual Private Server (VPS)__
* Microservice deployment with Docker
* Deployment to a Docker hosting provider

 ### Traditional Linux Service Deployment

* __init script__ to manage background processes using shell scripts in the `/etc/init.d` directory.

Web services have to be:

* _Reliable_: auto-restart if the server process crashes
* _Manageable_: integrates well with system managment practices
* _Observable_: administrator must be able to get status and activity information

The Linux package managment system doens't allow two MySQL instances. Instead, we implement separation in the same MySQL instance by using separate databases with different usernames and access privileges for each database.

The MySQL server must support TCP connections from `localhost`. You can edit the configuration file `/etc/mysql/my.cnf`, to have the following line:

```
bind-address = 127.0.0.1
```
This limits MySQL server connections to the processes on the server.

To send your files to your server, use the following:

```
rsync --archive --verbose ./ root@<server_ip>:/opt/
```

 ### PM2 to manage processes

There are many ways to manage server processes, to ensure restarts if the process crashes, and so on. We'll use [PM2](http://pm2.keymetrics.io/) because it's optimized for Node.js processes. It bundles process management and monitoring into one application.

Please read the documentation to properly install it in your production server.

 ### Microservice deployment with Docker

Docker automates the application deployment within software containers. The Docker implementation creates a layer of software isolation and virtualization based on Linux cgroups, kernel namespaaces, and union-capable filesystems, which blend together to make Docker what it is.

A _Docker Container_ is a running instantiation of a _Docker Image_. An image is a given Linux OS and application configuration designed by developers for whatever purpose they gave in mind. Developers describe an image using a __Dockerfile__, that is a fairly simple-to-write script showing Docker how to build and image. Docker images are designed to be copied to any server, where the image is instatiated as a Docker container.

Docker containerization is ver different from a virtual machine system such as VirtualBox. The processes running inside the container are actually runnin on the host OS. The containerization technology creates the illusion.

Docker ecosystem contains may tools, we'll focus on the following:

* __Docker engine__: Core execution system that orchestrates everything. It runs on a Linux host system, exposing network-based API that client applications ue to make Docker requests, such as building, deploying and running containers.
* __Docker machine__: Client application performing functions around provisioning Docker Engine instances on host computers.
* __Docker compose__: Helps you define, in a single file, a multi-cointainer application with all its dependencies defined.

 ### Installing Docker

We're looking for the Docker __Community Edition (CE)__ Because Docker runs on Linux, it does not run antively on macOS or Windows, and installation on either OS requires installing Linux inside a virtual machine and then running Docker tools within that virtual Linux machine.

 ### Deployment

Docker __Orchestrator__ services automatically deploy and manage Docker containers over a group of machines. Examples of this are Docker Swarm, Kubernetes, CoreOS Fleet and Apache MEsos.

The `docker-mahine` command comes with drivers supporting a lnog list of cloud-hosting providers.

For DigitalOcean, you would use something like thise:

```
docker-machine create --driver digitalocean --digitalocean-size 2gb \
--digitalocean-access-token TOKEN-FROM-PROVIDER sandbox
```
# Fundamentals: Heroku Deployment
Basic steps for heroku deployment

You must create an account in _heroku_ and download _Heroku CLI_ to use the following commands from your terminal.

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

# Intermediate: Architectural Patterns
For developing an ecosystem of microservices.

* Front Controller
* Layered
* Service Locator
* Observer

 ### Front Controller

The __Front Controller__ pattern is when all requests go for a single point in your architecture, called the __handler__, which then processes and dispatches the requests to other handlers. This is the pattern used by, for example, load balancers and reverses proxies.

It's usefull to scale horizontally and in helping other services not having to know where the controllers are and choosing the one with the lowest load that should handle the request faster. 

 ### Layered

The __Layered pattern__ is common in filesystems and operative system. Consists of creating different layers that go from the raw data through to the data seen by a user.

The idea is to separate the complexity of the different layers, each one not having to know how the others do their stasks.

 ### Service Locator

The __Service Locator__ pattern is actually an anti-pattern. Not considered a good practice because it adds much more complexity to an ecosystem. Consist of a central registry, called a _Service Locator_, where services register their abilities, and other servicies can consult the registry and know where the services they need are located.

Similar to the _Front Controller_, but with added complexity, as you need to contact the Service Locator and the service you need, instead of just making a simple request to a Front Controller. 

 ### Observer

The __Observer pattern__ is used every day in Node.js. It consist of a __Subject__, which mantains a list of dependats, called __Observers__, which get notified of any state change happening on the SUbject.

You can see this happening every time in your web browser when some code attaches an event listener to an object or interface elemen

 ### Publish/Subscribe

Similar pattern, __Pub-Sub__. You have _Subscribers_ that subscribe to a specific event, and then you have _Publishers_ that emit those events. 

The different to the previous pattern may look very thin but is actually important. This pattern involves __third-party service__ and unlike the Observer pattern, __Publishers have no knowledge of the Subscribers__. This removes the need to handle and directly notify the Subscribers.
# Intermediate: Socket.IO and WebSockets
[Socket.IO](https://socket.io/) is a JavaScript library for __realtime__ web applications. It enables realtime, __bi-directional communication__ between web clients and servers. It has two parts: a client-side library that runs in the browser, and a server-side library for Node.js. Both components have a nearly identical API. Like Node.js, it is event-driven.

Socket.IO primarily uses the WebSocket protocol with polling as a fallback option, while providing the same interface. Although it can be used as simply a wrapper for WebSocket, it provides many more features, including broadcasting to multiple sockets, storing data associated with each client, and asynchronous I/O.

# Intermediate: Chat libraries
* [Mustache](https://cdnjs.com/libraries/mustache.js/): Render templates

* [Moment](https://cdnjs.com/libraries/moment.js/): Manipulate time

* [QS](https://www.npmjs.com/package/qs): Query String - Set up room and user names

CDN
```
https://cdnjs.cloudflare.com/ajax/libs/mustache.js/3.0.1/mustache.min.js
https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.24.0/moment.min.js
https://cdnjs.cloudflare.com/ajax/libs/qs/6.7.0/qs.min.js
```

Please check the [reference project here!](https://github.com/herrera-ignacio/chat-app)
# Intermediate: WebSocket Protocol
* Websockets allow for __full-duplex communication__. Server can also iniciate communication (request) to clients, without a request from a client before. Client connects to the server and remains connected as long as needed.

* __Separate protocol__ from HTTP.

* __Persistent connection__ between server and client.

# Books to read
* Building Bots with Node.js
    * Stefan Buttigieg, Milorad Jevdjenic
* Advanced Node.js Development
    * Andrew Mead
# Book: NodeJs Complete Reference Guide

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
