
# An All-In-One Markdown Fork of "TLDR-Learning-JS-Design-Patterns-by-Addy-Osmani"

A **too long; didn't read** summary of the book [Learning JavaScript Design Patterns](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#designpatternsjavascript) by [Addy Osmani](https://github.com/addyosmani)

[![img](http://akamaicovers.oreilly.com/images/0636920025832/cat.gif)](http://addyosmani.com/resources/essentialjsdesignpatterns/book/)

# Contributing

Don't be shy! Lets make the docs better to help out our other fellow devs to learn design patterns in JavaScript.

Please do review the [book](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#designpatternsjavascript) first before making any PR fixes/additions to the `master`. Thanks!

---

# Anti-Patterns
- Bad solution to a particular problem. 
- Knowledge of anti-patterns is critical for success. Once we are able to recognize such anti-patterns, we're able to refactor our code to negate them so that the overall quality of our solutions improves instantly.

# Categories Of Design Pattern
### Creational
Creational design patterns focus on handling object creation mechanisms where objects are created in a manner suitable for the situation we're working in.

Patterns: *Constructor, Factory, Abstract, Prototype, Singleton and Builder*

### Structural
Structural patterns are concerned with object composition and typically identify simple ways to realize relationships between different objects.

Patterns:  *Iterator, Mediator, Observer and Visitor*

### Behavioral
Behavioral patterns focus on improving or streamlining the communication between disparate objects in a system.

Patterns: *Iterator, Mediator, Observer and Visitor*

<a name="toc"></a>
# Design Patterns
- [Constructor Pattern](#constructor)
- [Module Pattern](#module)
- [Revealing Module Pattern](#revealing-module)
- [Singleton Pattern](#singleton)
- [Observer Pattern](#observer)
- [Mediator Pattern](#mediator)
- [Prototype Pattern](#prototype)
- [Command Pattern](#command)
- [Facade Pattern](#facade)
- [Factory Pattern](#factory)
- [Mixin Pattern](#mixin)
- [Decorator Pattern](#decorator)
- [Flyweight Pattern](#flyweight)

<a name="constructor"></a>
# Constructor Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#constructorpatternjavascript).

### TL;DR

Object constructors are used to create specific types of objects - both preparing the object for use and accepting arguments which a constructor can use to set the values of member properties and methods when the object is first created.

```javascript

    function Car(model, year, miles) {
        this.model = model;
        this.year = year;
        this.miles = miles;
    }

    Car.prototype.toString = function () {
        return this.model + " has done " + this.miles + " miles";
    };

    // We can create new instances of the car
    var civic = new Car("Honda Civic", 2009, 20000);
    var mondeo = new Car("Ford Mondeo", 2010, 5000);

    // and then open our browser console to view the
    // output of the toString() method being called on
    // these objects
    console.log(civic.toString());
    console.log(mondeo.toString());

```

Above, a single instance of toString() will now be shared between all of the Car objects.

[Return to Table of Contents](#toc)

<a name="module"></a>
# Module Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#modulepatternjavascript).

### TL;DR

The Module pattern encapsulates "privacy", state and organization using closures. It provides a way of wrapping a mix of public and private methods and variables, protecting pieces from leaking into the global scope and accidentally colliding with another developer's interface. With this pattern, only a public API is returned, keeping everything else within the closure private.

When working with the Module pattern, we may find it useful to define a simple template that we use for getting started with it. Here's one that covers namespacing, public and private variables:

```javascript

    var myNamespace = (function () {

        var myPrivateVar, myPrivateMethod;

        // A private counter variable
        myPrivateVar = 0;

        // A private function which logs any arguments
        myPrivateMethod = function (foo) {
            console.log(foo);
        };

        return {

            // A public variable
            myPublicVar: "foo",

            // A public function utilizing privates
            myPublicFunction: function (bar) {

                // Increment our private counter
                myPrivateVar++;

                // Call our private method using bar
                myPrivateMethod(bar);

            }
        };

    })();

    console.log(myNamespace)

```

### Advantages
- it supports encapsulation of data
- it's a lot cleaner for developers coming from an object-oriented background than the idea of true encapsulation, at least from a JavaScript perspective.

### Disadvantages
- inability to create automated unit tests for private members and additional complexity when bugs require hot fixes.

[Return to Table of Contents](#toc)

# Revealing Module Pattern<a name="revealing-module"></a>

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#revealingmodulepatternjavascript).

### TL;DR

This is a pattern where we would simply define all of our functions and variables in the private scope and return an anonymous object with pointers to the private functionality we wished to reveal as public.

```javascript

    var myRevealingModule = (function () {

        var privateVar = "Ben Cherry",
            publicVar = "Hey there!";

        function privateFunction() {
            console.log("Name:" + privateVar);
        }

        function publicSetName(strName) {
            privateVar = strName;
        }

        function publicGetName() {
            privateFunction();
        }


        // Reveal public pointers to
        // private functions and properties

        return {
            setName: publicSetName,
            greeting: publicVar,
            getName: publicGetName
        };

    })();

myRevealingModule.setName("Paul Kinlan");

```

### Advantages
- This pattern allows the syntax of our scripts to be more consistent. It also makes it more clear at the end of the module which of our functions and variables may be accessed publicly which eases readability.

### Disadvantages
- A disadvantage of this pattern is that if a private function refers to a public function, that public function can't be overridden if a patch is necessary. This is because the private function will continue to refer to the private implementation and the pattern doesn't apply to public members, only to functions.

[Return to Table of Contents](#toc)

<a name="singleton"></a>
# Singleton Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#singletonpatternjavascript).

### TL;DR

The Singleton pattern is thus known because it restricts instantiation of a class to a single object. Singleton can be implemented by creating a class with a method that creates a new instance of the class if one doesn't exist. In the event of an instance already existing, it simply returns a reference to that object.

```javascript

var mySingleton = (function () {

  // Instance stores a reference to the Singleton
  var instance;

  function init() {

    // Singleton

    // Private methods and variables
    function privateMethod(){
        console.log( "I am private" );
    }

    var privateVariable = "Im also private";

    var privateRandomNumber = Math.random();

    return {

      // Public methods and variables
      publicMethod: function () {
        console.log( "The public can see me!" );
      },

      publicProperty: "I am also public",

      getRandomNumber: function() {
        return privateRandomNumber;
      }

    };

  };

  return {

    // Get the Singleton instance if one exists
    // or create one if it doesn't
    getInstance: function () {

      if ( !instance ) {
        instance = init();
      }

      return instance;
    }

  };

})();

var myBadSingleton = (function () {

  // Instance stores a reference to the Singleton
  var instance;

  function init() {

    // Singleton

    var privateRandomNumber = Math.random();

    return {

      getRandomNumber: function() {
        return privateRandomNumber;
      }

    };

  };

  return {

    // Always create a new Singleton instance
    getInstance: function () {

      instance = init();

      return instance;
    }

  };

})();


// Usage:

var singleA = mySingleton.getInstance();
var singleB = mySingleton.getInstance();
console.log( singleA.getRandomNumber() === singleB.getRandomNumber() ); // true

var badSingleA = myBadSingleton.getInstance();
var badSingleB = myBadSingleton.getInstance();
console.log( badSingleA.getRandomNumber() !== badSingleB.getRandomNumber() ); // true

// Note: as we are working with random numbers, there is a
// mathematical possibility both numbers will be the same,
// however unlikely. The above example should otherwise still
// be valid.

```


### Advantages
- In practice, the Singleton pattern is useful when exactly one object is needed to coordinate others across a system
- There must be exactly one instance of a class, and it must be accessible to clients from a well-known access point.

### Disadvantages
- often an indication that modules in a system are either tightly coupled or that logic is overly spread across multiple parts of a codebase.
- Singletons can be more difficult to test due to issues ranging from hidden dependencies, the difficulty in creating multiple instances, difficulty in stubbing dependencies and so on.

[Return to Table of Contents](#toc)

<a name="observer"></a>
# Observer Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#observerpatternjavascript).

### TL;DR

The Observer is a design pattern where an object (known as a subject) maintains a list of objects depending on it (observers), automatically notifying them of any changes to state.

When a subject needs to notify observers about something interesting happening, it broadcasts a notification to the observers (which can include specific data related to the topic of the notification).

When we no longer wish for a particular observer to be notified of changes by the subject they are registered with, the subject can remove them from the list of observers.

### Differences Between The Observer And Publish/Subscribe Pattern

Whilst very similar, there are differences between these patterns worth noting.

- The Observer pattern requires that the observer (or object) wishing to receive topic notifications must subscribe this interest to the object firing the event (the subject).
- The Publish/Subscribe pattern however uses a topic/event channel which sits between the objects wishing to receive notifications (subscribers) and the object firing the event (the publisher).

### Examples

The minimalist version of Publish/Subscribe I released on GitHub under a project called [pubsubz](http://github.com/addyosmani/pubsubz). This demonstrates the core concepts of subscribe, publish as well as the concept of unsubscribing.

```javascript
var pubsub = {};
 
(function(myObject) {
 
    // Storage for topics that can be broadcast
    // or listened to
    var topics = {};
 
    // An topic identifier
    var subUid = -1;
 
    // Publish or broadcast events of interest
    // with a specific topic name and arguments
    // such as the data to pass along
    myObject.publish = function( topic, args ) {
 
        if ( !topics[topic] ) {
            return false;
        }
 
        var subscribers = topics[topic],
            len = subscribers ? subscribers.length : 0;
 
        while (len--) {
            subscribers[len].func( topic, args );
        }
 
        return this;
    };
 
    // Subscribe to events of interest
    // with a specific topic name and a
    // callback function, to be executed
    // when the topic/event is observed
    myObject.subscribe = function( topic, func ) {
 
        if (!topics[topic]) {
            topics[topic] = [];
        }
 
        var token = ( ++subUid ).toString();
        topics[topic].push({
            token: token,
            func: func
        });
        return token;
    };
 
    // Unsubscribe from a specific
    // topic, based on a tokenized reference
    // to the subscription
    myObject.unsubscribe = function( token ) {
        for ( var m in topics ) {
            if ( topics[m] ) {
                for ( var i = 0, j = topics[m].length; i < j; i++ ) {
                    if ( topics[m][i].token === token ) {
                        topics[m].splice( i, 1 );
                        return token;
                    }
                }
            }
        }
        return this;
    };
}( pubsub ));

```
*Example: Using Our Implementation*

``` javascript
// Another simple message handler
 
// A simple message logger that logs any topics and data received through our
// subscriber
var messageLogger = function ( topics, data ) {
    console.log( "Logging: " + topics + ": " + data );
};
 
// Subscribers listen for topics they have subscribed to and
// invoke a callback function (e.g messageLogger) once a new
// notification is broadcast on that topic
var subscription = pubsub.subscribe( "inbox/newMessage", messageLogger );
 
// Publishers are in charge of publishing topics or notifications of
// interest to the application. e.g:
 
pubsub.publish( "inbox/newMessage", "hello world!" );
 
// or
pubsub.publish( "inbox/newMessage", ["test", "a", "b", "c"] );
 
// or
pubsub.publish( "inbox/newMessage", {
  sender: "hello@google.com",
  body: "Hey again!"
});
 
// We can also unsubscribe if we no longer wish for our subscribers
// to be notified
pubsub.unsubscribe( subscription );
 
// Once unsubscribed, this for example won't result in our
// messageLogger being executed as the subscriber is
// no longer listening
pubsub.publish( "inbox/newMessage", "Hello! are you still there?" );
```

*Example: Decoupling applications using Ben Alman's Pub/Sub implementation*

```javascript
;(function( $ ) {
 
  // Pre-compile templates and "cache" them using closure
  var
    userTemplate = _.template($( "#userTemplate" ).html()),
    ratingsTemplate = _.template($( "#ratingsTemplate" ).html());
 
  // Subscribe to the new user topic, which adds a user
  // to a list of users who have submitted reviews
  $.subscribe( "/new/user", function( e, data ){
 
    if( data ){
 
      $('#users').append( userTemplate( data ));
 
    }
 
  });
 
  // Subscribe to the new rating topic. This is composed of a title and
  // rating. New ratings are appended to a running list of added user
  // ratings.
  $.subscribe( "/new/rating", function( e, data ){
 
    if( data ){
 
      $( "#ratings" ).append( ratingsTemplate( data ) );
 
    }
 
  });
 
  // Handler for adding a new user
  $("#add").on("click", function( e ) {
 
    e.preventDefault();
 
    var strUser = $("#twitter_handle").val(),
       strMovie = $("#movie_seen").val(),
       strRating = $("#movie_rating").val();
 
    // Inform the application a new user is available
    $.publish( "/new/user", { name: strUser } );
 
    // Inform the app a new rating is available
    $.publish( "/new/rating", { title: strMovie, rating: strRating} );
 
    });
 
})( jQuery );
```

It's one of the easier design patterns to get started with but also one of the most powerful.

### Advantages
- Encourage us to think hard about the relationships between different parts of our application.
- Effectively could be used to break down an application into smaller, more loosely coupled blocks to improve code management and potentials for re-use.
- one of the best tools for designing decoupled systems and should be considered an important tool in any JavaScript developer's utility belt.

### Disadvantages
- In Publish/Subscribe, by decoupling publishers from subscribers, it can sometimes become difficult to obtain guarantees that particular parts of our applications are functioning as we may expect.

[Return to Table of Contents](#toc)

<a name="mediator"></a>
# Mediator Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#mediatorpatternjavascript).

### TL;DR

A Mediator is an object that coordinates interactions (logic and behavior) between multiple objects. It makes decisions on when to call which objects, based on the actions (or inaction) of other objects and input.

A real-world analogy could be a typical airport traffic control system. A tower (Mediator) handles what planes can take off and land because all communications (notifications being listened out for or broadcast) are done from the planes to the control tower, rather than from plane-to-plane. A centralized controller is key to the success of this system and that's really the role a Mediator plays in software design.

### A Simple Mediator

```javascript
var orgChart = {

  addNewEmployee: function(){

    // getEmployeeDetail provides a view that users interact with
    var employeeDetail = this.getEmployeeDetail();

    // when the employee detail is complete, the mediator (the 'orgchart' object)
    // decides what should happen next
    employeeDetail.on("complete", function(employee){

      // set up additional objects that have additional events, which are used
      // by the mediator to do additional things
      var managerSelector = this.selectManager(employee);
      managerSelector.on("save", function(employee){
        employee.save();
      });

    });
  },

  // ...
}
```

A Mediator is an object that handles the workflow between many other objects, aggregating the responsibility of that workflow knowledge into a single object. The result is workflow that is easier to understand and maintain.

### Similarities And Differences (event aggregator and mediator)

The similarities boil down to two primary items: events and third-party objects

 - **Events**: Both the event aggregator and mediator use events. The event aggregator, as a pattern, is designed to deal with events. The mediator, though, only uses them because it’s convenient.
 - **Third-Party Objects**: Both use a third-party object to facilitate things. In the case of an event aggregator, the third party object is there only to facilitate the pass-through of events from an unknown number of sources to an unknown number of handlers. In the case of the mediator, though, the business logic and workflow is aggregated into the mediator itself.

An event aggregator facilitates a “fire and forget” model of communication. The object triggering the event doesn’t care if there are any subscribers. It just fires the event and moves on. A mediator, though, might use events to make decisions, but it is definitely not “fire and forget”. A mediator pays attention to a known set of input or activities so that it can facilitate and coordinate additional behavior with a known set of actors (objects).


### Advantages
- Reduces the communication channels needed between objects or components in a system from many to many to just many to one.
- Adding new publishers and subscribers is relatively easy due to the level of decoupling present.

### Disadvantages
- Biggest downside of using the pattern is that it can introduce a single point of failure. Placing a Mediator between modules can also cause a performance hit as they are always communicating indirectly. Because of the nature of loose coupling, it's difficult to establish how a system might react by only looking at the broadcasts.

At the end of the day, tight coupling causes all kinds of headaches and this is just another alternative solution, but one which can work very well if implemented correctly.

[Return to Table of Contents](#toc)

<a name="prototype"></a>
# Prototype Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#prototypepatternjavascript).

### TL;DR

Creates objects based on a template of an existing object through cloning. We can think of the prototype pattern as being based on prototypal inheritance where we create objects which act as prototypes for other objects.

```javascript
var myCar = {
 
  name: "Ford Escort",
 
  drive: function () {
    console.log( "Weeee. I'm driving!" );
  },
 
  panic: function () {
    console.log( "Wait. How do you stop this thing?" );
  }
 
};
 
// Use Object.create to instantiate a new car
var yourCar = Object.create( myCar );
 
// Now we can see that one is a prototype of the other
console.log( yourCar.name );
```
If we wish to implement the prototype pattern without directly using `Object.create`, we can simulate the pattern as per the above example as follows:
```javascript
var vehiclePrototype = {
 
  init: function ( carModel ) {
    this.model = carModel;
  },
 
  getModel: function () {
    console.log( "The model of this vehicle is.." + this.model);
  }
};
 
 
function vehicle( model ) {
 
  function F() {};
  F.prototype = vehiclePrototype;
 
  var f = new F();
 
  f.init( model );
  return f;
 
}
 
var car = vehicle( "Ford Escort" );
car.getModel();
```

**Note**: This alternative does not allow the user to define read-only properties in the same manner (as the `vehiclePrototype` may be altered if not careful).

### Advantages
- One of the benefits of using the prototype pattern is that we're working with the prototypal strengths JavaScript has to offer natively rather than attempting to imitate features of other languages.
- Easy way to implement inheritance
- Performance boost: when defining a function in an object, they're all created by reference (so all child objects point to the same function) instead of creating their own individual copies.

### Disadvantages
- It is worth noting that prototypal relationships can cause trouble when enumerating properties of objects and (as Crockford recommends) wrapping the contents of the loop in a `hasOwnProperty()` check. 

[Return to Table of Contents](#toc)

<a name="command"></a>
# Command Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#commandpatternjavascript).

### TL;DR

The Command pattern aims to encapsulate method invocation, requests or operations into a single object and gives us the ability to both parameterize and pass method calls around that can be executed at our discretion. 

It provides us a means to separate the responsibilities of issuing commands from anything executing commands, delegating this responsibility to different objects instead. They consistently include an execution operation (such as run() or execute()).

###Example

```javascript
(function(){
 
  var carManager = {
 
    // request information
    requestInfo: function( model, id ){
      return "The information for " + model + " with ID " + id + " is foobar";
    },
 
    // purchase the car
    buyVehicle: function( model, id ){
      return "You have successfully purchased Item " + id + ", a " + model;
    },
 
    // arrange a viewing
    arrangeViewing: function( model, id ){
      return "You have successfully booked a viewing of " + model + " ( " + id + " ) ";
    }
 
  };
 
})();
```

Here is what we would like to be able to achieve:

```javascript
carManager.execute( "buyVehicle", "Ford Escort", "453543" );
```

As per this structure we should now add a definition for the carManager.execute method as follows:

```javascript
carManager.execute = function ( name ) {
    return carManager[name] && carManager[name].apply( carManager, [].slice.call(arguments, 1) );
};
```

Our final sample calls would thus look as follows:

```javascript
carManager.execute( "arrangeViewing", "Ferrari", "14523" );
carManager.execute( "requestInfo", "Ford Mondeo", "54323" );
carManager.execute( "requestInfo", "Ford Escort", "34232" );
carManager.execute( "buyVehicle", "Ford Escort", "34232" );
```

### Advantages
- it enables us to decouple objects invoking the action from the objects which implement them, giving us a greater degree of overall flexibility in swapping out concrete classes (objects).

[Return to Table of Contents](#toc)

<a name="facade"></a>
# Facade Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#facadepatternjavascript).

### TL;DR

This pattern provides a convenient higher-level interface to a larger body of code, hiding its true underlying complexity. Think of it as simplifying the API being presented to other developers, something which almost always improves usability.

Whenever we use jQuery's `$(el).css()` or `$(el).animate()` methods, we're actually using a Facade - the simpler public interface that avoids us having to manually call the many internal methods in jQuery core required to get some behavior working. This also avoids the need to manually interact with DOM APIs and maintain state variables.

We're all familiar with jQuery's `$(document).ready(..)`. Internally, this is actually being powered by a method called bindReady(), which is doing this:

```javascript
bindReady: function() {
    ...
    if ( document.addEventListener ) {
      // Use the handy event callback
      document.addEventListener( "DOMContentLoaded", DOMContentLoaded, false );
 
      // A fallback to window.onload, that will always work
      window.addEventListener( "load", jQuery.ready, false );
 
    // If IE event model is used
    } else if ( document.attachEvent ) {
 
      document.attachEvent( "onreadystatechange", DOMContentLoaded );
 
      // A fallback to window.onload, that will always work
      window.attachEvent( "onload", jQuery.ready );
               ...
```

This is another example of a Facade, where the rest of the world simply uses the limited interface exposed by `$(document).ready(..)` and the more complex implementation powering it is kept hidden from sight.

### Disadvantages
Facades generally have few disadvantages, but one concern worth noting is performance. Namely, one must determine whether there is an implicit cost to the abstraction a Facade offers to our implementation and if so, whether this cost is justifiable.

[Return to Table of Contents](#toc)

<a name="factory"></a>
# Factory Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#factorypatternjavascript).

### TL;DR

The Factory pattern is another creational pattern concerned with the notion of creating objects. Where it differs from the other patterns in its category is that it doesn't explicitly require us use a constructor. Instead, a Factory can provide a generic interface for creating objects, where we can specify the type of factory object we wish to be created.

This pattern is particularly useful if the object creation process is relatively complex, e.g. if it strongly depends on dynamic factors or application configuration.

###Example

```javascript
// Types.js - Constructors used behind the scenes
 
// A constructor for defining new cars
function Car( options ) {
 
  // some defaults
  this.doors = options.doors || 4;
  this.state = options.state || "brand new";
  this.color = options.color || "silver";
 
}
 
// A constructor for defining new trucks
function Truck( options){
 
  this.state = options.state || "used";
  this.wheelSize = options.wheelSize || "large";
  this.color = options.color || "blue";
}
 
 
// FactoryExample.js
 
// Define a skeleton vehicle factory
function VehicleFactory() {}
 
// Define the prototypes and utilities for this factory
 
// Our default vehicleClass is Car
VehicleFactory.prototype.vehicleClass = Car;
 
// Our Factory method for creating new Vehicle instances
VehicleFactory.prototype.createVehicle = function ( options ) {
 
  switch(options.vehicleType){
    case "car":
      this.vehicleClass = Car;
      break;
    case "truck":
      this.vehicleClass = Truck;
      break;
    //defaults to VehicleFactory.prototype.vehicleClass (Car)
  }
 
  return new this.vehicleClass( options );
 
};
 
// Create an instance of our factory that makes cars
var carFactory = new VehicleFactory();
var car = carFactory.createVehicle( {
            vehicleType: "car",
            color: "yellow",
            doors: 6 } );
 
// Test to confirm our car was created using the vehicleClass/prototype Car
 
// Outputs: true
console.log( car instanceof Car );
 
// Outputs: Car object of color "yellow", doors: 6 in a "brand new" state
console.log( car );
```
***Approach #1: Modify a VehicleFactory instance to use the Truck class***

```javascript
var movingTruck = carFactory.createVehicle( {
                      vehicleType: "truck",
                      state: "like new",
                      color: "red",
                      wheelSize: "small" } );
 
// Test to confirm our truck was created with the vehicleClass/prototype Truck
 
// Outputs: true
console.log( movingTruck instanceof Truck );
 
// Outputs: Truck object of color "red", a "like new" state
// and a "small" wheelSize
console.log( movingTruck );
```

***Approach #2: Subclass VehicleFactory to create a factory class that builds Trucks***

```javascript
function TruckFactory () {}
TruckFactory.prototype = new VehicleFactory();
TruckFactory.prototype.vehicleClass = Truck;
 
var truckFactory = new TruckFactory();
var myBigTruck = truckFactory.createVehicle( {
                    state: "omg..so bad.",
                    color: "pink",
                    wheelSize: "so big" } );
 
// Confirms that myBigTruck was created with the prototype Truck
// Outputs: true
console.log( myBigTruck instanceof Truck );
 
// Outputs: Truck object with the color "pink", wheelSize "so big"
// and state "omg. so bad"
console.log( myBigTruck );
```

### Disadvantages
- Due to the fact that the process of object creation is effectively abstracted behind an interface, this can also introduce problems with unit testing depending on just how complex this process might be.

[Return to Table of Contents](#toc)

<a name="mixin"></a>
# Mixin Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#mixinpatternjavascript).

### TL;DR

In JavaScript, we can look at inheriting from Mixins as a means of collecting functionality through extension. Mixins allow objects to borrow (or inherit) functionality from them with a minimal amount of complexity. As the pattern works well with JavaScripts object prototypes, it gives us a fairly flexible way to share functionality from not just one Mixin, but effectively many through multiple inheritance.

They can be viewed as objects with attributes and methods that can be easily shared across a number of other object prototypes.

### Example 
```javascript
var myMixins = {
 
  moveUp: function(){
    console.log( "move up" );
  },
 
  moveDown: function(){
    console.log( "move down" );
  },
 
  stop: function(){
    console.log( "stop! in the name of love!" );
  }
 
};
```
We can then easily extend the prototype of existing constructor functions to include this behavior using a helper such as the Underscore.js `_.extend()` method:

```javascript
// A skeleton carAnimator constructor
function CarAnimator(){
  this.moveLeft = function(){
    console.log( "move left" );
  };
}
 
// A skeleton personAnimator constructor
function PersonAnimator(){
  this.moveRandomly = function(){ /*..*/ };
}
 
// Extend both constructors with our Mixin
_.extend( CarAnimator.prototype, myMixins );
_.extend( PersonAnimator.prototype, myMixins );
 
// Create a new instance of carAnimator
var myAnimator = new CarAnimator();
myAnimator.moveLeft();
myAnimator.moveDown();
myAnimator.stop();
 
// Outputs:
// move left
// move down
// stop! in the name of love!
```

### Advantages
- Mixins assist in decreasing functional repetition and increasing function re-use in a system.

### Disadvantages
- Some developers feel that injecting functionality into an object prototype is a bad idea as it leads to both prototype pollution and a level of uncertainty regarding the origin of our functions. In large systems this may well be the case.

[Return to Table of Contents](#toc)

<a name="decorator"></a>
# Decorator Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#decoratorpatternjavascript).

### TL;DR

Similar to Mixins, they can be considered another viable alternative to object sub-classing. They can be used to modify existing systems where we wish to add additional features to objects without the need to heavily modify the underlying code using them.

The Decorator pattern isn't heavily tied to how objects are created but instead focuses on the problem of extending their functionality. Rather than just relying on prototypal inheritance, we work with a single base object and progressively add decorator objects which provide the additional capabilities. The idea is that rather than sub-classing, we add (decorate) properties or methods to a base object so it's a little more streamlined.

```javascript
// The constructor to decorate
function MacBook() {
 
  this.cost = function () { return 997; };
  this.screenSize = function () { return 11.6; };
 
}
 
// Decorator 1
function memory( macbook ) {
 
  var v = macbook.cost();
  macbook.cost = function() {
    return v + 75;
  };
 
}
 
// Decorator 2
function engraving( macbook ){
 
  var v = macbook.cost();
  macbook.cost = function(){
    return v + 200;
  };
 
}
 
// Decorator 3
function insurance( macbook ){
 
  var v = macbook.cost();
  macbook.cost = function(){
     return v + 250;
  };
 
}
 
var mb = new MacBook();
memory( mb );
engraving( mb );
insurance( mb );
 
// Outputs: 1522
console.log( mb.cost() );
 
// Outputs: 11.6
console.log( mb.screenSize() );
```

In the above example, our Decorators are overriding the `MacBook()` super-class objects `.cost()` function to return the current price of the `Macbook` plus the cost of the upgrade being specified.

It's considered a decoration as the original `Macbook` objects constructor methods which are not overridden (e.g. `screenSize()`) as well as any other properties which we may define as a part of the `Macbook` remain unchanged and intact.

### Advantages
- Fairly flexible. As we've seen, objects can be wrapped or "decorated" with new behavior and then continue to be used without needing to worry about the base object being modified.
- In a broader context, this pattern also avoids us needing to rely on large numbers of subclasses to get the same benefits.

### Disadvantages
-  If poorly managed, it can significantly complicate our application architecture as it introduces many small, but similar objects into our namespace.

[Return to Table of Contents](#toc)

<a name="flyweight"></a>
# Flyweight Pattern

For complete reference, click [here](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#detailflyweight).

### TL;DR

The Flyweight pattern is a classical structural solution for optimizing code that is repetitive, slow and inefficiently shares data. It aims to minimize the use of memory in an application by sharing as much data as possible with related objects (e.g application configuration, state and so on).

### Flyweight's State Concepts
- **Intrinsic** - Intrinsic information may be required by internal methods in our objects which they absolutely cannot function without
- **Extrinsic** - Extrinsic information can however be removed and stored externally.

Next, let's continue our look at Flyweights by implementing a system to manage all of the books in a library.

```javascript
var Book = function( id, title, author, genre, pageCount,publisherID, ISBN, checkoutDate, checkoutMember, dueReturnDate,availability ){

   this.id = id;
   this.title = title;
   this.author = author;
   this.genre = genre;
   this.pageCount = pageCount;
   this.publisherID = publisherID;
   this.ISBN = ISBN;
   this.checkoutDate = checkoutDate;
   this.checkoutMember = checkoutMember;
   this.dueReturnDate = dueReturnDate;
   this.availability = availability;

};

Book.prototype = {

  getTitle: function () {
     return this.title;
  },

  getAuthor: function () {
     return this.author;
  },

  getISBN: function (){
     return this.ISBN;
  },

  // For brevity, other getters are not shown
  updateCheckoutStatus: function( bookID, newStatus, checkoutDate, checkoutMember, newReturnDate ){

     this.id = bookID;
     this.availability = newStatus;
     this.checkoutDate = checkoutDate;
     this.checkoutMember = checkoutMember;
     this.dueReturnDate = newReturnDate;

  },

  extendCheckoutPeriod: function( bookID, newReturnDate ){

      this.id = bookID;
      this.dueReturnDate = newReturnDate;

  },

  isPastDue: function(bookID){

     var currentDate = new Date();
     return currentDate.getTime() > Date.parse( this.dueReturnDate );

   }
};
```

This probably works fine initially for small collections of books, however as the library expands to include a larger inventory with multiple versions and copies of each book available, we may find the management system running slower and slower over time.

We can now separate our data into intrinsic and extrinsic states as follows: data relevant to the book object (`title`, `author`, etc) is intrinsic whilst the checkout data (`checkoutMember`, `dueReturnDate`, etc) is considered extrinsic.

```javascript
// Flyweight optimized version
var Book = function ( title, author, genre, pageCount, publisherID, ISBN ) {

    this.title = title;
    this.author = author;
    this.genre = genre;
    this.pageCount = pageCount;
    this.publisherID = publisherID;
    this.ISBN = ISBN;

};
```

As we can see, the extrinsic states have been removed. Everything to do with library check-outs will be moved to a manager and as the object data is now segmented, a factory can be used for instantiation.

*A basic Factory*
```javascript
// Book Factory singleton
var BookFactory = (function () {
  var existingBooks = {}, existingBook;

  return {
    createBook: function ( title, author, genre, pageCount, publisherID, ISBN ) {

      // Find out if a particular book meta-data combination has been created before
      // !! or (bang bang) forces a boolean to be returned
      existingBook = existingBooks[ISBN];
      if ( !!existingBook ) {
        return existingBook;
      } else {

        // if not, let's create a new instance of the book and store it
        var book = new Book( title, author, genre, pageCount, publisherID, ISBN );
        existingBooks[ISBN] = book;
        return book;

      }
    }
  };

})();
```

*Managing the Extrinsic States*
```javascript
// BookRecordManager singleton
var BookRecordManager = (function () {

  var bookRecordDatabase = {};

  return {
    // add a new book into the library system
    addBookRecord: function ( id, title, author, genre, pageCount, publisherID, ISBN, checkoutDate, checkoutMember, dueReturnDate, availability ) {

      var book = bookFactory.createBook( title, author, genre, pageCount, publisherID, ISBN );

      bookRecordDatabase[id] = {
        checkoutMember: checkoutMember,
        checkoutDate: checkoutDate,
        dueReturnDate: dueReturnDate,
        availability: availability,
        book: book
      };
    },
    updateCheckoutStatus: function ( bookID, newStatus, checkoutDate, checkoutMember, newReturnDate ) {

      var record = bookRecordDatabase[bookID];
      record.availability = newStatus;
      record.checkoutDate = checkoutDate;
      record.checkoutMember = checkoutMember;
      record.dueReturnDate = newReturnDate;
    },

    extendCheckoutPeriod: function ( bookID, newReturnDate ) {
      bookRecordDatabase[bookID].dueReturnDate = newReturnDate;
    },

    isPastDue: function ( bookID ) {
      var currentDate = new Date();
      return currentDate.getTime() > Date.parse( bookRecordDatabase[bookID].dueReturnDate );
    }
  };

})();
```

The result of these changes is that all of the data that's been extracted from the Book class is now being stored in an attribute of the BookManager singleton (BookDatabase) - something considerably more efficient than the large number of objects we were previously using. Methods related to book checkouts are also now based here as they deal with data that's extrinsic rather than intrinsic.

[Return to Table of Contents](#toc)
