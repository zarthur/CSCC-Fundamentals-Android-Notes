# Weeks 7 and 8 - Inheritance and Polymorphism

## Corresponding Text
*Learn Java for Android Development*, pp. 141-167, 169-174

## Extending Classes
Often, we can classify things with statements like "cars are a kind of
vehicle" or "squares are a kind of rectangle and a rectangle is a kind of
shape". From a software development point of view, what we might mean by these
kinds of statements are cars have some state information that is common to
vehicles in general (speed, color) and have behaviors common to all vehicles
(move, park).  We can say that cars inherit state and behaviors from vehicles.
**Inheritance** is a hierarchical relationship between similar categories where
one category inherits state and behaviors from at least one other category.  

There are two types of inheritance: implementation inheritance and interface
inheritance.  **Implementation inheritance** refers to one class being able to
reuse another class's states and behaviors through extension.  **Interface
inheritance** refers to one class inheriting another class's behavior
templates, guides for what behavior should be supported but without code to
provide an implementation.  Java only supports single inheritance for
implementation inheritance meaning that one class can extend or reuse code
from only one other class.  Java support multiple inheritance with interface
inheritance meaning that a class can inherit behavior/method templates from
many classes.  For now, we'll explore implementation inheritance.  We'll
look at interface inheritance later.


super()
overriding
override annotation

### The Object Class

### Equality

### Finalization

### Composition

## Polymorphism
### Upcasting and Late Binding

### Downcasting  

### Covariant Return Types
