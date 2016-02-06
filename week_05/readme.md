# Week 5 - Classes and Objects

## Corresponding Text
*Learn Java for Android Development*, pp. 89-107, 124-137

## Classes and Objects
Last week, we discussed working with object in Java by creating instances of
classes in the standard library.  For example, we created an ArrayList using
code like `new ArrayList<String>()`.  We described classes as defining the
structure of the objects created from them, describing attributes representing
state and methods representing behaviors related to that state.  In Java, like
other object-oriented languages, we can create our own classes.  

### Declaring Classes
In all the examples we've worked with so far, we've relied on a class named
*Main* specified by code that looks like this:

```Java
public class Main {
    //additional Java code
}
```

This code declares a class named *Main* and the code associated with it is
written between the two braces. The minimal syntax required for declaring a
class is:

```Java
class Name {
    //class member declarations
}
```

A class is declared by first using the reserved word *class* followed by the
name of the class.  The name of the class is often written with the first
letter of every word capitalized (compare this to methods where the first
letter of the first word is lower case and the first letter of subsequent words
is capitalized).  After the name, we specify the code associated with the
class, defining its attributes and methods, between a set of braces.  Again,
this is the minimally required syntax and we'll build upon it as we learn about
additional language features.

Continuing with the weather theme of previous weeks, let's create a class
that represents weather data at a specific time for a specific location.

```Java
package com.myname.week_05;

class WeatherData {
}

public class Main {
    public static void main(String[] args) {
        WeatherData weather = new WeatherData();
    }    
}
```

Here we have two classes: *weatherData* and *Main*.  Recall that we write our
*main()* method in the *Main* class and configure IntelliJ to use the *Main*
class to run our programs.  We call the *Main* class the program's entry point.
We'll continue to use a class named *Main* as an entry point in our programs.
In addition to *Main*, we've also declared a class named *WeatherData*.  In
the *main()* method, we create a new *WeatherData* instance using the *new*
operator.  Notice that our class name is used to specify the data type of the
variable storing an instance of the class.  There's not much to our WeatherData
class right now besides its name.

### Constructing Objects
Often when we create an instance of a class, there is some code that we'd like
to execute to do things like set an instance's state.  One way we can do this
is by using a constructor.  A class's constructor is a method used to allocate
memory for an object when the *new* operator is used to create an instance.  
Once memory is allocated, the constructor is called (or invoked) to initialize
the object.  Once execution of the constructor is complete, the new operator
returns the memory location of the newly created object (classes specify
reference types).  The constructor doesn't have a name itself but uses the
name of the class that declares the constructor.  When declaring a constructor,
the class's name is followed by parentheses containing a comma-separated
parameter list that can be used when the constructor is called.  Let's look
at an example.

```Java
package com.myname.week_05;

class WeatherData {
    WeatherData(String city, double temperature, double humidity) {
        System.out.println("WeatherData city: " + city);
        System.out.println("WeatherData temperature: " + temperature);
        System.out.println("WeatherData humidity: " + humidity);
    }
}

public class Main {
    public static void main(String[] args) {
        WeatherData weather = new WeatherData("Columbus", 50, 72);
    }
}
```

If you run this program, the output should be:

```
WeatherData city: Columbus
WeatherData temperature: 50.0
WeatherData humidity: 72.0
```

We've added a constructor to our WeatherData class.  After indicating that
the method is a constructor by using the class's name, we specified that
the constructor took three parameters: a string and two doubles.  The
constructor then displayed some information using the parameters.  When we
ran the program, execution began in in the *Main* class with the *main()*
method.  In this method, we constructed a new instance of the *WeatherData*
class using the *new* operator.  The *new* operator first allocated sufficient
memory for a *WeatherData* object, then called the constructor with the values
we specified, and finally returned the memory location of the new object which
was stored in the *weather* variable.  

What did the *new* operator do when we didn't declare a constructor?  Java
created a default constructor.  A *default constructor* is used to when no
other constructor is specified.  It takes no arguments.  The default
constructor for the *WeatherData* class would be equivalent to

```Java
WeatherData(){}
```

Once we explicitly define a constructor, the default constructor is no longer
available.  Java allows us to specify multiple constructors, so we could add
the default constructor (and others) if we'd like.

```Java
package com.myname.week_05;

class WeatherData {
    WeatherData(){}

    WeatherData(String city) {
        System.out.println("WeatherData city: " + city);
    }

    WeatherData(String city, double temperature, double humidity) {
        System.out.println("WeatherData city: " + city);
        System.out.println("WeatherData temperature: " + temperature);
        System.out.println("WeatherData humidity: " + humidity);
    }
}

public class Main {
    public static void main(String[] args) {
        WeatherData weather1 = new WeatherData();
        WeatherData weather2 = new WeatherData("Cleveland");
        WeatherData weather3 = new WeatherData("Columbus", 50, 72);
    }
}
```

In this example, we have three different constructors for the WeatherData
class: one that behaves like the default constructor, one that takes only
a string parameter, and one that takes a string and two doubles.  Constructors,
like other methods, must have unique signatures.

### Encapsulation
Previously, we described classes as combining related state information
(attributes) with behaviors that make use of or modify that state information.
Combining state information with related behaviors in one data structure is
known as **encapsulation**.  Encapsulation is also sometimes used to describe
restrictions on access to behaviors and state information to avoid misuse or
interference.  We'll begin by looking at how we define behaviors and state
information and then look at restricting access to these items.  

#### Fields
A class field stores an attributed that associated with a class.  A class field
is shared by all objects created by the class (using the new operator).  If one
object modifies a class field, another object created from the same class will
"see" the new value.

Within a class declaration, a class field is declared using the following
syntax:

```
static type_name variable_name [ = expression ] ;
```

A class field is specified using the reserved word *static* followed by the
data type of the field, the name of the field, and an initial value, which is
optional.  If no initial value is specified, the field is initialized to 0,
0.0, false, null, etc, depending on the. field's type.

In our WeatherData class, we can add class fields for values that we would want
to share with all objects created from the class.  

We can effectively make a class field a constant by using the reserved word
*final*.  This will prevent changes to the field.  A convention is to write
variable names with all capital letters when they represent constants.

```Java
package com.myname.week_05;

class WeatherData {
    final static String TEMP_UNIT = "F";
    final static String HUMIDITY_UNIT = "%";
    final static String PRECIPITATION_UNIT = "%";

    static int counter = 0;

    WeatherData(String city, double temperature, double humidity,
                double precipitation) {
        counter++;
    }
}

public class Main {
    public static void main(String[] args) {
        System.out.println("Counter: " + WeatherData.counter);

        WeatherData columbus = new WeatherData("Columbus", 50, 75, 30);
        System.out.println("Counter: " + WeatherData.counter);

        WeatherData cleveland = new WeatherData("Cleveland", 45, 70, 30);
        System.out.println("Counter: " + WeatherData.counter);

        System.out.println("Counter: " + columbus.counter);
        System.out.println("Counter: " + cleveland.counter);
    }
}
```

In this example, we've added four fields *TEMP_UNIT*, *HUMIDITY_UNIT*,
*PRECIPITATION_UNIT*, and *counter*.  The first three are marked final so
they will act like constants.  If you run this example, the output should be:

```
Counter: 0
Counter: 1
Counter: 2
Counter: 2
Counter: 2
```

Notice that the value of constructor increases each time a WeatherData object
is created.  This is consistent with the code in the constructor.  Each
WeatherData object also has access to the updated value.  While we were able
to use the objects we created in main() to access the counter class field
(`cleveland.counter` and `columbus.counter`), we typically use the class
name to access the field (`WeatherData.counter`).

Instance or object fields...
#### Methods

### Access Control

### Initializers

## Garbage Collection
