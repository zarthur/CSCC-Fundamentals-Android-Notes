# Week 12 - Generics

## Corresponding Text
*Learn Java for Android Development*, pp. 255-273

## Collections Example
Recall that when we started working with Lists, Sets, and Maps, we specified
the types of elements contained in instances of ArrayList, HashMap, and so on.  
For example, if we were planning on storing Strings in an array list, we might
have written code like this

```java
List<Sting> StringList = new ArrayList<>();
```

Here, the type appearing between the angle brackets (`<` and '>') is known as
the *type parameter* and, in this case, it is used to specify the types of
elements stored in the list.  In general, we can use type parameters to create
both flexible and type-safe classes.  To understand what that means and why
it's a good thing, let's look at an example.

Prior to Java 5, the Collections framework didn't allow us to specify a type
parameter.  Suppose we had a *Contact* class that stored a contact's name and
email address and we wanted to store a collection of contacts in a list.

```java
package com.myname.week_12;

import java.util.ArrayList;
import java.util.List;

class Contact {
    private String name;
    private String email;

    Contact(String name, String email) {
        this.name = name;
        this.email = email;
    }

    public void displayInfo() {
        System.out.println("Name: " + name + ", Email: " + email);
    }
}


public class Main {
    public static void main(String[] agrs) {
        List contacts = new ArrayList();
        contacts.add(new Contact("bob", "bob@bob.com"));
        contacts.add("sue");

        for (Object o: contacts) {
            Contact c = (Contact) o;
            c.displayInfo();
        }

    }
}
```

Here, we've created a *Contact* class and, in *Main.main()*, created an
instance of *ArrayList* without specifying the types of elements stored in the
instance.  Without specifying a type for the elements, Java has no information
about their type when we iterate through the list.  In the for-each loop, we
have to declare each element as an *Object*.  If we want to access a method
unique to *Contact*, we have to cast the *Object* instance to *Contact* then
access the method.  If we've only added instances of the *Contact* class to our
list, this wouldn't be a problem.  But if we add an instance of another class
that isn't a subclass of *Contact*, like *String*, we'll encounter a
*ClassCastException*.  One solution is to check if each *Object* instance is an
instance of *Contact* before casting but it would be nice to restrict our list
to only allow instances of *Contact* as elements - then Java would know the
list only contains *Contact* instances when iterating through the list. We can
accomplish this using generic types.  

Notice also that the program above will actually compile and start running.  We
can see that by the fact that there is output associated with the contact we
initially add to the list.  It would be nice if Java could indicate that there
is a problem at compile time, before the program is ever run.  The ability
to detect type safety violations like this at compile time rather than at
runtime is the primary benefit of using generics.

## Generic Types
### Declaring and Using Generic Types
### Type Parameter Bounds
### Type Parameter scope
### Wildcards
### Generic Methods
### Arrays and Generics
## Exercise
