# How to connect reactJS with nodeJS 

In this session we are going to focus on how do we connect our reactJS application with the nodeJS platform 

Creating a demo which will have two parts : 
    
    the client application that we will first create as a react application 
    
    ExpressJS application with nodeJS 

and we will then try to connect both of those applications together 

ExpressJS is a rapid application development framework that allows us to create a web 
application very quickly on the nodeJS platform and will use it to create a custom API and then connect that endpoint to the reactJS client application 

Let's start with creating our client and node Express applications and see how to set them up and then get them connected to each other 

let's start with our demo and we will first go to the [nodeJS website]https://nodejs.org/en/ nodejs.org

Install node.js 

next we'll open our editor you can use any editor of your choice so I'm going to use an editor
called Visual Studio code and let's open up the terminal here and I also have the command prompt open where I have navigated to a folder that I've created called reactJSdemo (I change the folder name to ConnectReactWithNodeJS)

now we will invoke the NPM package manager which is invoked by the command npx 
we will use npx to go and create a react application called client 

let's go and open up this project 

so that's my client app 

let's fire up the terminal that has launched the terminal for us

and here all we need to do to fire up this application and get it running with 

    $ cd ConnectReactWithNodeJS/client
    $ npm start 
    
which will compile and start up this application that is created
we go to the browser url localhost:3000 so that gives us a startup page 

the sample default startup page for the first client application and this is going to work as a client 

We'll now go and try to create the back-end application which is
going to be another application that we will set up and to create that let's say 

    $ cd ConnectReactWithNode
    $ npx express-generator api 
    
ok so this will create an Express application on nodeJS called api 

let's go to the terminal now 

   change directory:
     $ cd api

   install dependencies:
     $ npm install

   run the app:
     $ npm start

so that's the standard Express application 

now let's go to the API application we'll go into the bin folder and here we'll have a file called www we will change the default port number from
3000 to 9000 

    (ConnectReactWithNodeJS/api/bin)

        /**
        * Get port from environment and store in Express.
        */

        var port = normalizePort(process.env.PORT || '9000');
        app.set('port', port);

we've just updated the port number 

now let's go to the routes and we'll create a new file in the routes folder called testAPI.js 

    (ConnectReactWithNodeJS/api/routes)

        $ touch testAPI.js

here we will say variable express equal to require express and here we will say variable router equal to express dot router and here we will say router dot get we'll open this so that's
the default route is going to be slash which we had a callback function which will take the request and the response parameter and next pointer in case there is another function to call 
post that and here we will just send a simple message saying API is working a simple message sent out for the route default route and we'll have to export this so we will say module dot exports equal to router 

    (testAPI.js)

    var express = require("express");
    var router = express.Router();

    router.get("/", function(req, res, next){
        res.send("API is working properly)
    });

    module.exports = router;

all right so that's done let's go to the app.js file and here let us import that s

    (ConnectReactWithNodeJS/api/app.js)

    ...

    app.use('/', indexRouter);
    app.use('/users', usersRouter);
    app.use("/testAPI", testAPIRouter);

    // catch 404 and forward to error handler
    app.use(function(req, res, next) {
      next(createError(404));
    });

    ...


so here is where we will import this by saying app dot use we want to use the test API file and that is test API router and this is exactly the same variable name that we will declare here which will be equal to require and we are going to say routes test API 

    (ConnectReactWithNodeJS/api/app.js)

    ...

    var indexRouter = require('./routes/index');
    var usersRouter = require('./routes/users');
    var testAPIRouter = require('./routes/testAPI');

    var app = express();
    ...

alright so let's try launching this now 

    (ConnectReactWithNodeJS/api)

    $ npm start



let's go 

    url localhost:9000/testAPI

And there we have a message which says that the API is working properly 

so this is tested now let's go to the client application 

in the client application let's go to the app.js file okay

    (ConnectReactWithNodeJS/client/src/app.js)
    
    ...
    class App extends React.Component{
        constructor(props) {
            super(props);
            this.state = { apiResponse: "" };
        }

        callAPI() {
            fetch("http://localhost:9000/testAPI")
            .then(res -> res.text())
            .then(res -> this.setState({ apiResponse: res }));
        }

        componentWillMount() {
            this.callAPI();
        }

        render() {
            return (
              <div className="App">
                <header className="App-header">
                  <img src={logo} className="App-logo" alt="logo" />

                </header>
                <p>{ this.state.apiResponse }</p>
              </div>
            );
        }
    }
    ...

here we we will declare a class App which extends so that creates a new module so it extends react dot component and here we have constructor we will accept the props here I would pass props to the superclass constructor and set the state to API response currently it just has an empty value 

next we light up a new function called call API which will generate a call to that API URL which was HTTP localhost 9000 was the port and test API was the URL 

once this is called we will then call the then() function declare response generate a call to response dot text and then we will again say this dot set state API response is going to be equal to the response that comes in from that URL which is nothing but a string which says API is working

another function called componentWillMount() so the moment the component is
initialized on the screen and loaded we will generate a call to this function we created called callAPI() 

then comes the render function in the render function we would go and return a div class name with App this would be a header the standard logo after which in the paragraph we wouldn't need the link here, right after the header we'll add a paragraph where we will print this dot state and the variable we created called API response and we will data bind to that so that largely completes the return 


so we simply create this application that's the react application that will talk to this API and this URL that is hosted on node communicate with it get a simple string as a response print that string inside this react application 

so once that's done we have the component will mount which is a lifecycle method here it's a new life cycle event that gets trigger at the moment the component loads and that's when we have actually gone and executed the call API custom function




let's go to the API application we'll terminate the execution here 

there's one more step that we'll have to perform which is cross-origin resource sharing which means that to allow the react application to communicate from another hosted domain to this particular domain server we will actually have to enable cross-origin scripting and sharing otherwise it will block the request coming from another application 

so for that we'll have to install globally cross-origin module 

    (ConnectReactWithNodeJS/api)

        $ npm install --save cors

and save indicates that it's going to do a global install for all applications that are on this particular system. 


    (ConnectReactWithNodeJS/api/app.js)

        ...

        app.use(logger('dev'));
        app.use(express.json());
        app.use(cors());
        app.use(express.urlencoded({ extended: false }));
        
        ...

let's go to the app.js and right here is where we would want it to go in use cors so we will say app.use cors so that it uses this particular API, and here is where we will declare the cors variable 
            
    (ConnectReactWithNodeJS/api/app.js)

        ...

        var logger = require('morgan');
        var cors = require("cors");

        var indexRouter = require('./routes/index');

        ...

and where input the new library package that we just installed, which is cors 

so looks good and now let's fire up this application with an 

    (ConnectReactWithNodeJS/api)

    $ npm start 

    (ConnectReactWithNodeJS/client)

    $ npm start

    Important ! As he use the class component I get an error message in the browser saying that React is undefined so I had to put the import in the App.js file

        import React, { Component } from 'react';

let's also fire up client with an npm start and now when I go and try looking for localhost 3000 which is the react application the react application should go and talk to the API that is hosted on nodeJS which returns to me that string message 

so if I go and ping that API separately which is localhost 9000 test API you can see this is a standard message so here I've directly ping the API that's hosted in nodejs 

    url localhost:9000/testAPI

now we are consuming it through the reactJS application and if you scroll down you can see the message returned from node.js into react seeing the API is working properly 

so the practical impact of this is to actually host restful services etc on node as a server and you can go and consume the data that is returned there could be in string text json format
directly into the reactJS acting as a client application 


