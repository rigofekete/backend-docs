## Object Oriented Programming


## Clean Code

### What is Clean Code?

Clean Code is a set of principles established with the purpose of presenting code in the most clear and unambiguous way possible. 

We can say that concepts such as `OOP` and `Functional Programming` are forms of writing `Clean Code`, as these paradigms introduce different ideas and constraints that attempt to encourage programmers to write readable and maintainable code that is easier to debug and extend. 

### What are the main concepts of Clean Code

The first concept I can think of on the top of my head is the idea of `DRY` code. This "Don't Repeat Yourself" constraint, informs us that any code base that repeats the same logic pattern in multiple areas, should be refactored into a more modular version, where the repeated block can be simply moved into a dedicated function or Class, so it can get called or instantiated whenever this same logic is needed. 

This way, besides reducing the size of the code and making the specified logic more modular, we avoid introducing unnecessary bugs as well as reducing the time spent debugging errors. 

For instance, if a logic pattern happens in a dedicated function only, if we ever need to change or refactor any of its code, we would only need to do it once, inside the function, as opposed to changing all of the places where this logic pattern would be present, most likely spread across the code base. We can confidently predict that the latter would be a complete chaos to handle, with a high risk of introducing errors. 

Another aspect of Clean Code I can talk about are variable names. To adhere to Clean Code, one most try to choose variable names that are explicit (but simple enough) and representative of their purpose, with as little ambiguity as possible. 

For example, having a variable assignment without any obvious context, such as:

```
int x = 2;
```
would be a complete nightmare not only for who wrote it, as he will need to come back to it at some point, but also to any other person who'll bump into it. That example also has another aspect that is not `clean` at all, which is the actual assignment of a `magic number`, `2` to `x`. 

In a few months time (or weeks), and if the context of the expression is not completely obvious, surely not even whoever wrote this will have any idea of what is going on and what this number is, neither what the variable stands for. 

There are other constraints that go hand in hand with these ideas, which I will summarize below for now:

- Code comments should be short and relevant, only present if strictly needed to help maintainability  
- Functions should be as small as they can be.
- Functions should be responsible for a narrow scope of action (do 1 thing).
- Use of Polymorphism instead of conditionals. 

The last 3 bullet points are examples of `SOLID` principles, namely, `Single Responsibility` and `Open for Extension Closed for Modification` constraints. 

## What is OOP

Object Oriented Programming is a programming paradigm that was thought and developed to model "real world" systems and relationships of entities as well as their behavior. This modeling is achieved through the use of `classes` that `instantiate objects`, which are the `entities` it represents in code.

This concept is used in development due to its capacity to separate different responsibilities under custom defined types which can have multiple relationships depending of the level of closeness and similarity with each other.

A simple example is an attempt to model a graphical user interface, which has separate types of instances (buttons, windows, edit boxes, list boxes) all of them instantiating from a base class or deriving additional subclasses to override initial behavior if needed.    

## Classes

Classes are user defined types that group data and methods internally, defining attributes and custom behavior for instantiated/constructed objects. They are templates or blueprints for object instantiation. 

These classes can be extended with derived subclasses, each one inheriting/overriding behavior and data from the parent ones. 

## Abstraction and Encapsulation

I have decided to place these 2 together in the same topic because they are like two sides of the same coin. 

Encapsulation is the concept of organizing or hiding data in one place, in this case, suppose a class, which contains attributes needed for its instance behavior. This data, when made `private`, will not be accessible (directly) from outside of the class due to its opaque nature. 

Another way of thinking of encapsulation is a function. When called, it does whatever it needs to do using internal logic and complexity that the caller does not need to know about. 

Now, looking at this same example from a different angle, we touch `Abstraction`, which is the concept of exposing what is `encapsulated` internally, in a clean and simple interface/surface, created with the purpose of doing a specific job.

Again, we can use the exact same example of a function. The function signature is an abstraction of its opaque internal complexity, exposed to the user through a surface that shows how it is meant to be used through its identifier, parameter list and return value/type (and also through additional specifications available in API documentations). 


## Inheritance

Inheritance is the concept of sharing code between different classes. This is used in `OOP` when the relation between parent classes and child classes have proximity. A simple example is a class `Animal`, which derives another class like `Dog`. Since dog is always an animal (`Is-a` relationship), it is perfectly logic for this child class to inherit everything from the base class, plus extending additional attributes and actions (traits) that satisfy the nature of its own condition. 

By making the child class inherit all of the attributes and implementation of its parent, we are also adhering to the `SOLID`,  `Open-Close Principle`, which encourages code from a class to be `extended` through this inheritance method, and not `modified` directly in the main class to suit incoming needs. Avoiding this direct modifications is an effective way to avoid bugs and code verbosity.

## Polymorphism 

The concept of having a type that can have multiple forms under itself. This is achieved by having child classes that have the same interface in common, in other words, having derived classes that implement the exact same function signature from the base class (or `abstract` class). Any derived class that overrides/implements this function signature will become that interface (with its own extended behavior), allowing itself to be used interchangeably at run-time. This adheres to another `SOLID` constraint which is the `Liskov Substitution` principle.   














