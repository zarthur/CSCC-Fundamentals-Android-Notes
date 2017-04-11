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

Here, the type appearing between the angle brackets (`<` and `>`) is known as
the *type parameter* and, in this case, it is used to specify the types of
elements stored in the list.  In general, we can use type parameters to create
both flexible and type-safe classes.  To understand what that means and why
it's a good thing, let's look at an example.

Prior to Java 5, the Collections framework didn't allow us to specify a type
parameter.  Suppose we had a *Contact* class that stored a contact's name and
email address and we wanted to store a collection of contacts in a list.

```java
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
A **generic type** is a class or interface that declares a family of types
using a **formal type parameter list**, a comma-separated list of type
parameters between angle brackets.  The syntax of a generic type is:

```java
class identifier<formal_type_parameter_list> {}
interface identifier<formal_type_parameter_list> {}
```

Examples of generic types are `List<E>` and `Map<K, V>`.  In the `List<E>`
generic type, *List* is an interface and *E* is the type parameter which, in
this case, specifies the list's elements' type.  Similarly in the `Map<K, V>`
generic type, *Map* is an interface and *K* and *V* are type parameters
specifying the map's key and value types.

By convention, a single uppercase letter is used for a type parameter name.
These names should be meaningful such as *E* for element, *T* for type, *K* for
key, and *V* for value.

Parameterized types are instances of generic types and are created by
specifying a type or types to replace the generic type's type parameter or
parameters.  For example, `List<Contact>` is a parameterized type based
on `List<E>`.  Similarly, `Map<String, Double>` is a parameterized type based
on `Map<K, V>`.

The type name that replaces the type parameter is known as the **actual type
argument**.  There are five kinds of actual type arguments supported by
generics.

1. **Concrete Type** The name of a class or interface is used as the type
   parameter.  `List<Contact>` is an example using concrete type.
2. **Concrete Parameterized Type** Another parameterized type is used as the
   type parameter.  `List<Map<String, Double>>` is an example using a concrete
   parameterized type, `Map<String, Double>` as the actual type argument.
3. **Array Type** An array type is passed to the type parameter.  
   `List<String []>` is an example using an array type; here, the elements of
   the list are arrays of strings.
4. **Type Parameter** Another type parameter is used as the type parameter.  
   Consider `class X<E> { List<E> internalList; }`.  The class *X* has a type
   parameter, *E*, that is used as a type parameter for `List<E>` when
   declaring an instance field.
5. **Wildcard** A wildcard, `?`, is used to indicate that the type is not
   known. For example, `List<?> aList;` indicates that the type of the list
   elements is unknown.

Every generic type also identifies a raw type.  A *raw type* is specified using 
a generic type without its type parameters.  Raw types are not generic types 
themselves and can be used as if `Object` were specified as the type parameter 
but without anytype safety checks.

### Declaring and Using Generic Types
In order to declare and use generic types, we must specify a formal type
parameter list when declaring a class or interface and make use of the type
parameters in the implementation.  Suppose we wanted to create a stack, a
collection of elements supporting two operations: *push* to add an element
and *pop* to remove the most recently added element.  

```java
class StackFullException extends Exception {}

class StackEmptyException extends Exception {}


class Stack<E> {
    private E[] elements;
    private int index = 0;
    private int size;

    Stack(int size) {
        elements = (E[]) new Object[size];
        this.size = size;
    }

    void push(E element ) throws StackFullException {
        if (index >= size) {
            throw new StackFullException();
        }

        elements[index] = element;
        index++;
    }

    E pop () throws StackEmptyException {
        if (index == 0) {
            throw new StackEmptyException();
        }

        E returnElement = elements[index - 1];
        index--;
        return returnElement;
    }
}


public class Main {
    public static void main(String[] agrs) {
        Stack<String> strings = new Stack<>(2);
        try {
            strings.push("Hello");
            strings.push("World");
            System.out.println(strings.pop());
            System.out.println(strings.pop());
        } catch (StackFullException | StackEmptyException e) {
            e.printStackTrace();
        }

    }
}
```

In this example, we create a generic class *Stack\<E\>* that can store elements
of type *E* on a stack.  When we create an instance of the class, we'll specify
an actual type argument.  The actual type argument will be used to create an
array that will be used to store elements. Notice that we can't write
`new E[size]` in the constructor. This is due to how arrays and generics are
handled by the Java compiler.  See the *Arrays and Generics* section of the
assigned reading for a detailed explanation. We can, however, cast the array at
runtime.  We can suppress the warning by adding a
`@SuppressWarnings("unchecked")` annotation to the constructor but it's not
really necessary since *Object* instances can always be downcast to *E* in this
case. The class has two methods: *push()* and *pop()*.  The *push()* method
takes a single parameter of type *E* and add it to the array. The *pop()*
method returns the most recently added *E* instance.  Both methods throw
checked exceptions (since they inherit from *Exception* and not
*RuntimeException*) to indicate that elements cannot be added or there are not
elements to pop from the stack.

In *Main.main()*, we specify and actual type argument of *String* when creating
an instance of the generic class.

Beyond type safety, this example demonstrates another reason to use generic
types.  Rather than creating a class to represent a stack of strings, another
to represent a stack of doubles, etc., we can define a generic class once that
can be use to store any type.

### Type Parameter Bounds
*Stack\<E\>* from the previous example, *List\<E\>*, and *Map\<K, V\>* are examples
where the type parameter is an **unbounded type parameter** - any type can be
used as the actual type argument.  It is sometimes necessary to restrict kinds
of actual type arguments that can be passed to a type parameter - we might
require that the type parameter implement an interface so we can make use of a
specific method or we might only want to store certain kinds of things.  To
restrict the actual type arguments, we can specify an upper bound.  An **upper
bound** is a type that serves as an upper limit (in terms of subtyping) on the
types that can be chosen as actual type arguments.  The upper bound is
specified using the reserved word `extends` followed by a type name. We can
specify multiple upper bounds by separating the type names with an `&`
character.

Suppose we were writing a program that could display video on screen and play
audio through the computer's speakers.  We'd like to create a stack of objects
that are both displayable on screen and playable through the speakers so we
can create a playlist and then play all the items.

```Java
package com.myname.week_12;

import java.util.ArrayList;
import java.util.List;

class StackFullException extends Exception {}

class StackEmptyException extends Exception {}


interface Video {
    void display();
}

interface Audio {
    void playAudio();
}

class Movie implements Video, Audio {
    private String location;

    Movie(String location) {
        this.location = location;
    }

    public void display() {
        System.out.println("Displaying video from " + location);
    }

    public void playAudio() {
        System.out.println("Playing audio from " + location);
    }
}



class Stack<E extends Video & Audio > {
    private List<E> elements = new ArrayList<>();
    private int index = 0;
    private int size;

    Stack(int size) {
        this.size = size;
    }

    void push(E element ) throws StackFullException {
        if (index >= size) {
            throw new StackFullException();
        }

        elements.add(index, element); // increment first, then use as array index
        index++;
    }

    E pop () throws StackEmptyException {
        if (index == 0) {
            throw new StackEmptyException();
        }

        E returnElement = elements.get(index - 1);
        index--;
        return returnElement;
    }

    void playAllAudioAndVideo() {
        while (index > 0) {
            try {
                E element = pop();
                element.display();
                element.playAudio();
            } catch (StackEmptyException e) {
                // won't get here because of while condition
            }
        }
    }
}


public class Main {
    public static void main(String[] agrs) {
        Stack<Movie> movies = new Stack<>(2);
        try {
            movies.push(new Movie("C:\\AMovie.mp4"));
            movies.push(new Movie("C:\\AMovie2.mp4"));
        } catch (StackFullException e) {
            e.printStackTrace();
        }
        movies.playAllAudioAndVideo();


    }
}
```

In this example, we have two interfaces: one to represent object that represent
audio and one that represents video (without audio).  The *Movie* class
implements both these interfaces.  We've modified *Stack*'s type parameter list
to specify two upper bounds: *Audio* and *Video*.  Notice that the
implementation makes use of methods declared by these interfaces so it makes
sense that we don't want to allow our stack to include elements of types that
don't implement these interfaces.

### Type Parameter Scope
A type parameter's scope is the entirety of the corresponding generic type
unless the type parameter is masked/hidden. Consider this example:

```Java
class EnclosingClass<T> {
    static class EnclosedClass<T extends Comparable<T>> {
    }
}
```

Within *EnclosedClass*, *EnclosingClass*'s type parameter *T* is masked by
another type parameter (also named *T*).  The type parameter of *EnclosedClass*
differs from the type parameter of *EnclosingClass*.  *EnclosedClass*'s type
parameter is bounded by types that implement *Comparable*.  From within
*EnclosedClass* the type *T* refers only to the bounded type parameter and not
to the unbounded type parameter associated with *EnclosingClass*.  To avoid
confusion, a different name should be given to *EnclosedClass*'s type
parameter.

```Java
class EnclosingClass<T> {
    static class EnclosedClass<U extends Comparable<U>> {
    }
}
```

While *U* isn't as meaningful as *T*, the situation justifies its use in order
to avoid confusion.

### Wildcards
Suppose we were writing a program that kept a list of strings, maybe a list of
names, and we wanted to write a method to display the elements of the list. We
might write something like this:

```java
package com.myname.week_12;


import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void displayList(List<Object> list) {
        for (Object o: list) {
            System.out.println(o);
        }
    }

    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        names.add("bob");
        names.add("sue");
        displayList(names);
    }

}
```

While this seems reasonable, if you type this code in IntelliJ or try to
compile it with the Java compiler, you'll encounter an error with a message
about the inability of converting a List\<String\> to a List\<Object\>.  In
general, for a given subtype *x* of type *y* and a given raw type *G*, *G\<x\>*
is not a subtype of *G\<y\>*.

![Generic Inheritance](images/generic-inheritance.png)

While *String* is a subtype of *Object*, the polymorphic behavior doesn't apply
to parameterized types so *List\<String\>* is not a subtype of *List\<Object\>*.
However, *List\<String\>* is a subtype of *Collection\<String\>* since the raw
type *List* is a subtype of *Collection*.

Despite this, we would still like our *displayList* method to work.  We can
achieve this using the `?` wildcard which represents any type.

```java
package com.myname.week_12;

import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void displayList(List<?> list) {
        for (Object o: list) {
            System.out.println(o);
        }
    }

    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        names.add("bob");
        names.add("sue");
        displayList(names);
    }

}
```

### Generic Methods
Suppose we wanted to write a method to copy the elements of one list to another
list.  We could write a method similar to the following but it would be very
limited.

```java
public static void copyList(List<Object> source, List<Object> destination) {
    for (Object o: source) {
        destination.add(o);
    }
}
```

Its limitation is that it can only be used to copy lists of objects and not
lists of other element types.  The reason it is limited is because of the
inheritance issue we encountered when discussing wildcards: if *x* is a subtype
of *y*, *G\<x\>* is not necessarily a subtype of *G\<y\>*.  We might consider 
using wildcards to address the problem.

```java
public static void copyList(List<? extends String> source, List<? super String> destination) {
    for (String s: source) {
        destination.add(s);
    }
}
```

Here, *source* is any type that is a subtype of *String* or *String* itself
(*String* is an upper bound) and *destination* is any type which is a supertype 
of *String* is or *String* itself (*String* is a lower bound).  Note that we
cannot specify lower bounds as formal type parameters when creating generic
classes.  While this code will work to copy lists of strings, it is limited
to lists of strings and lists of *Object* elements.  We'd like to create a copy
method that is more robust. To do this, we can create a generic method.  Note
that the methods above are not generic methods but simply methods that take
parameters of generic types.  A *generic method* is a class or instance method
with a type-generalized implementation.  Generic methods are declared using the
following syntax:

```java
<formal_type_parameter_list> return_type identifier(parameter_list)
```

The formal type parameter list is the same as when specifying a generic type.
The type parameter can appear as both the method's return type as well as the
type of parameters in the parameter list.  Java infers the actual type
arguments from the context in which the method is called.  Here is the list
copying method as a generic method as well as code to demonstrate it's use.

```java
package com.myname.week_12;

import java.util.ArrayList;
import java.util.List;


public class Main {
    public static <T> void copyList(List<T> source, List<T> destination) {
        for (T object: source) {
            destination.add(object);
        }
    }

    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        names.add("bob");
        names.add("sue");
        List<String> namesCopy = new ArrayList<>();
        copyList(names, namesCopy);
        for (String name: namesCopy) {
            System.out.println(name);
        }

    }

}
```

The biggest advantage here is that we are no longer limited by the type of the
elements in the list like we were when we tried to use a method relying on
generic types with wildcards.

## Exercise
Implement a *Queue\<E\>* generic type similar to the *Stack\<E\>* type but with
*enqueue()* and *dequeue()* methods.  The *enqueue()* method adds an element to
the queue and the *dequeue()* method removes the first/oldest element from the
queue.  Stacks are often described as being "last-in, first-out" whereas queues
represent a "first-in, first-out" behavior.
