
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

# Design Patterns<a name="toc"></a>
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

# Constructor Pattern<a name="constructor"></a>

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

# Module Pattern<a name="module"></a>

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

# Singleton Pattern<a name="singleton"></a>

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

# Observer Pattern<a name="observer"></a>

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

# Mediator Pattern<a name="mediator"></a>

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
