
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
