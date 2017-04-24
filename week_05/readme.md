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

```java
public class Main {
    //additional Java code
}
```

This code declares a class named *Main* and the code associated with it is
written between the two braces. The minimal syntax required for declaring a
class is:

```java
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

```java
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

```java
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

```java
WeatherData(){}
```

Once we explicitly define a constructor, the default constructor is no longer
available.  Java allows us to specify multiple constructors, so we could add
the default constructor (and others) if we'd like.

```java
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
A class field stores an attribute that is associated with a class.  A class
field is shared by all objects created by the class (using the new operator).  
If one object modifies a class field, another object created from the same
class will "see" the new value.

Within a class declaration, a class field is declared using the following
syntax:

```
static type_name variable_name [ = expression ] ;
```

A class field is specified using the reserved word *static* followed by the
data type of the field, the name of the field, and an initial value, which is
optional.  If a value is specified, we call the value the class field
initializer. If no initial value is specified, the field is initialized to 0,
0.0, false, null, etc, depending on the. field's type.

In our WeatherData class, we can add class fields for values that we would want
to share with all objects created from the class.  

We can effectively make a class field a constant by using the reserved word
*final*.  This will prevent changes to the field.  A convention is to write
variable names with all capital letters when they represent constants.

```java
package com.myname.week_05;

class WeatherData {
    final static String TEMP_UNIT = "F";
    final static String HUMIDITY_UNIT = "%";
    final static String PRECIPITATION_UNIT = "%";
    final static int FREEZING_TEMP = 32;

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

In this example, we've added five fields *TEMP_UNIT*, *HUMIDITY_UNIT*,
*PRECIPITATION_UNIT*, *FREEZING_TEMP*, and *counter*.  The first four are
marked final so they will act like constants.  If you run this example, the
output should be:

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

An **instance field** stores an attribute associated with an object where each
instance of a class maintains a separate value. The syntax for the declaration
is:

```
type_name variable_name [ = expression ];
```

Here, *type_name* is the data type of the instance field, *variable_name* is
the name of the field, and *expression* is the optional value to set the field
to.  If an initial value is specified by *expression*, we call the value the
*instance field initializer*.  If no initial value is specified, the value
will be set to default value, depending on the type, when an instance is
created.  Notice that the primary difference between an instance field and a
class field is that a class field requires the use of the reserved word
*static*.

Let's add some instance fields to our WeatherData class.

```java
package com.myname.week_05;

class WeatherData {
    final static String TEMP_UNIT = "F";
    final static String HUMIDITY_UNIT = "%";
    final static String PRECIPITATION_UNIT = "%";
    final static int FREEZING_TEMP = 32;

    String cityName;
    double temperature;
    double humidity;
    double precipitation;

    WeatherData(String city, double temperature, double humidity,
                double precipitation) {
        cityName = city;
        this.temperature = temperature;
        this.humidity = humidity;
        this.precipitation = precipitation;
    }
}

public class Main {
    public static void main(String[] args) {
        WeatherData columbus = new WeatherData("Columbus", 50, 75, 30);
        System.out.println("City name: " + columbus.cityName);
        System.out.println("Temperature: " + columbus.temperature + WeatherData.TEMP_UNIT);
        System.out.println("Humidity: " + columbus.humidity + WeatherData.HUMIDITY_UNIT);
        System.out.println("Chance of precipitation: " + columbus.precipitation
                + WeatherData.PRECIPITATION_UNIT);
    }
}
```

If we run this program, the output is:

```
City name: Columbus
Temperature: 50.0F
Humidity: 75.0%
Chance of precipitation: 30.0%
```

After declaring and initializing the class fields, we declared four instance
methods: *cityName*, *temperature*, *humidity*, and *precipitation*.  In the
constructor, we assign values to these fields using the values passed to the
constructor.  Notice the use of *this* when the field name was the same
as the parameter name.  *this* is a reference to the current instance and
allows us to be explicit when accessing the instance field.  If we had written
`temperature = temperature` instead of `this.temperature = temperature`, Java
would assume that we were reassigning the value of the parameter *temperature*
to itself.  So, we have to be explicit and use *this*.

#### Methods
We've explored class methods in an earlier lecture.  Recall that methods can be
used to represent behavior.  Behavior associated with a class can be written
as class methods.  Class methods, like class fields, can be accessed using the
class's name and without creating an instance.  While class methods can
access and modify class fields, the cannot access or modify instance fields.  

Recall that class methods have the following syntax:

```
static return_type name(parameter_list) {
    //statements to execute
}
```
where *return_type* is the data type of any value returned by the method
(*void* if no value is returned), *name* is the method's name, and
*parameter_list* is a list of comma-separated types and names for values
passed to the method.  Class methods require the reserved word *static* be
a part of their method headers.  

Let's add some class methods to our WeatherData class.

```java
package com.myname.week_05;

class WeatherData {
    final static String TEMP_UNIT = "F";
    final static String HUMIDITY_UNIT = "%";
    final static String PRECIPITATION_UNIT = "%";
    final static int FREEZING_TEMP = 32;

    String cityName;
    double temperature;
    double humidity;
    double precipitation;

    WeatherData(String city, double temperature, double humidity,
                double precipitation) {
        cityName = city;
        this.temperature = temperature;
        this.humidity = humidity;
        this.precipitation = precipitation;
    }

    static double tempFtoC(double fahrenheit) {
        return 5.0 / 9 * (fahrenheit - 32);
    }

    static double tempCtoF(double celsius) {
        return 9.0 / 5 * celsius + 32;
    }

    static void displayFreezeTemp() {
        System.out.println(FREEZING_TEMP + TEMP_UNIT);
    }
}

public class Main {
    public static void main(String[] args) {
        System.out.println(WeatherData.tempCtoF(100));
        System.out.println(WeatherData.tempFtoC(32));
       WeatherData.displayFreezeTemp();
    }
}
```

We've added three class methods: *tempFtoC*, *tempCtoF*, and
*displayFreezeTemp*.  Notice that none of these class methods access or modify
instance fields but *displayFreezeTemp* does access two class fields.  Also,
when we access class fields from within the class, we can drop the class name
as long as what we are trying to access is unambiguous; if it is ambiguous, we
have to use the class name like `WeatherData.FREEZING_TEMP`.

An **instance method** represents a behavior associated with an object and
can access/modify an object's instance fields.  

The syntax for declaring an instance method is:

```
return_type name(parameter_list) {
    //statements to execute
}
```

Like a class method, the method's signature is given by the method's name and
the number, order, and types of the the parameters in *parameter_list*.  Class
and instance methods must be unique to their classes based on their signatures.

Let's add some instance methods to our program:

```java
package com.myname.week_05;

class WeatherData {
    final static String TEMP_UNIT = "F";
    final static String HUMIDITY_UNIT = "%";
    final static String PRECIPITATION_UNIT = "%";
    final static int FREEZING_TEMP = 32;

    String cityName;
    double temperature;
    double humidity;
    double precipitation;

    WeatherData(String city, double temperature, double humidity,
                double precipitation) {
        cityName = city;
        this.temperature = temperature;
        this.humidity = humidity;
        this.precipitation = precipitation;
    }

    static double tempFtoC(double fahrenheit) {
        return 5.0 / 9 * (fahrenheit - 32);
    }

    static double tempCtoF(double celsius) {
        return 9.0 / 5 * celsius + 32;
    }

    static void displayFreezeTemp() {
        System.out.println(FREEZING_TEMP + TEMP_UNIT);
    }

    boolean willSnow() {
        return (temperature <= FREEZING_TEMP) && (precipitation >= 50);
    }

    void displayWeatherReport() {
        String temp = temperature + TEMP_UNIT;
        String humid = humidity + HUMIDITY_UNIT;
        String precip = precipitation + PRECIPITATION_UNIT;
        String snowLikely = willSnow() ? "likely" : "unlikely";

        System.out.println("The current temperature in " + cityName + " is " + temp
                + ". The current relative humidity is: " + humid
                + ". The current chance of precipitation is " + precip
                + ". It is " + snowLikely + " to snow.");
    }
}

public class Main {
    public static void main(String[] args) {
        WeatherData columbus = new WeatherData("Columbus", 30, 60, 75);
        columbus.displayWeatherReport();
    }
}
```

We've added two instance methods: *willSnow* and *displayWeatherReport*.  The
*willSnow* instance method returns a boolean value that depends on the value
of the instance fields *temperature* and *precipitation*.  The
*displayWeatherReport* makes use of class fields, instance fields, and another
instance method.  The output of the program is:

`The current temperature in Columbus is 30.0F. The current relative humidity is: 60.0%. The current chance of precipitation is 75.0%. It is likely to snow.`

### Access Control
A class exposes an *interface*, constructors, methods, and fields that can be
accessed from outside the class.  An interface acts like a contract between a
class and other objects that might communicate with it or instances of it.  The
contract is such that the class won't change the methods and fields that
other things depend on.  A class also provides an *implementation* that
consists of code that supports the interface including helper methods that
assist exposed methods but probably should be publicly accessible themselves.
Hiding the implementation details makes it easier to make changes to code - we
only have to make sure that the interface remains the same to avoid breaking
code that uses our classes but we are free to make modifications to the
implementation.

Java provides four levels of control for methods and fields: *public*,
*protected*, *private*, and *package-private*. We can indicate that a method
or field is *public*, *protected*, or *private* by prefixing the declaration
with `public`, `protected`, or `private`.

A field, method, or constructor that is declared **public** is accessible from
anywhere.  Classes can also be declared public and public classes must be
declared in files whose names match the classes' names.

A **protected** field or method is accessible from all classes in the same
package as the the field's or method's class as well as any subclasses of the
class.

A **private** field or method cannot be accessed from outside the class in
which it is declared.

If no access control is specified, the method, field, or class is
**package-private**, meaning that it is only accessible to classes within the
same package.

Often, instance fields are declared *private* and special methods known as
*getters* and *setters* are defined for accessing or modifying their values.  
Usually, class fields are declared *public* but can be made private to hide
unnecessary details.  

Let's declare the access control levels of our fields and methods in
WeatherData.

```java
package com.myname.week_05;

class WeatherData {
    // class fields
    private final static String TEMP_UNIT = "F";
    private final static String HUMIDITY_UNIT = "%";
    private final static String PRECIPITATION_UNIT = "%";
    final static int FREEZING_TEMP = 32;

    // instance fields
    private String cityName;
    private double temperature;
    private double humidity;
    private double precipitation;

    // constructor
    public WeatherData(String city, double temperature, double humidity,
                       double precipitation) {
        cityName = city;
        this.humidity = humidity;
        this.precipitation = precipitation;
        setTemperature(temperature);
    }

    // class methods
    public static double tempFtoC(double fahrenheit) {
        return 5.0 / 9 * (fahrenheit - 32);
    }

    public static double tempCtoF(double celsius) {
        return 9.0 / 5 * celsius + 32;
    }

    // instance methods
    public String getCityName() {
        return cityName;
    }

    public void setTemperature(double temperature) {
        this.temperature = temperature;
    }


    private boolean willSnow() {
        return (temperature <= FREEZING_TEMP) && (precipitation >= 50);
    }

    public void displayWeatherReport() {
        String temp = temperature + TEMP_UNIT;
        String humid = humidity + HUMIDITY_UNIT;
        String precip = precipitation + PRECIPITATION_UNIT;
        String snowLikely = willSnow() ? "likely" : "unlikely";

        System.out.println("The current temperature in " + cityName + " is " + temp
                + ". The current relative humidity is: " + humid
                + ". The current chance of precipitation is " + precip
                + ". It is " + snowLikely + " to snow.");
    }
}

public class Main {
    public static void main(String[] args) {
        WeatherData columbus = new WeatherData("Columbus", 30, 60, 75);
        columbus.displayWeatherReport();
    }
}
```

We've made a number of changes but the output should be the same as before.  
First, we marked the class fields specifying units as *private*. The constant
representing the freezing temperature was marked *public*. All the instance
methods were marked *private* We made both the constructor and the class
methods that provide temperature conversion functionality public - any class
should be able to access them.  We created a `getter` for the city name and a
`setter` for the temperature and marked them public; this allows other classes
to indirectly access the city name and indirectly set the temperature - we'll
see why this could be important in the next example.  We decided that
`willSnow()` is a helper method and shouldn't be available to other classes, so
we marked it as *private*.  The `displayWeatherReport()` method should be
accessible to other classes, so we marked it *public*.  

What if we wanted to perform a check on the value the specified for the
temperature and make sure it was greater than a minimal value?  This is one
reason to use *setters* and to prevent access directly to the instance field.

```java
package com.myname.week_05;

class WeatherData {
    // class fields
    private final static String TEMP_UNIT = "F";
    private final static String HUMIDITY_UNIT = "%";
    private final static String PRECIPITATION_UNIT = "%";
    final static int FREEZING_TEMP = 32;

    // instance fields
    private String cityName;
    private double temperature;
    private double humidity;
    private double precipitation;

    // constructor
    public WeatherData(String city, double temperature, double humidity,
                       double precipitation) {
        cityName = city;
        this.humidity = humidity;
        this.precipitation = precipitation;
        setTemperature(temperature);
    }

    // class methods
    public static double tempFtoC(double fahrenheit) {
        return 5.0 / 9 * (fahrenheit - 32);
    }

    public static double tempCtoF(double celsius) {
        return 9.0 / 5 * celsius + 32;
    }

    // instance methods
    public String getCityName() {
        return cityName;
    }

    public void setTemperature(double temperature) {
        this.temperature = temperature < -461 ? -461 : temperature;
    }


    private boolean willSnow() {
        return (temperature <= FREEZING_TEMP) && (precipitation >= 50);
    }

    public void displayWeatherReport() {
        String temp = temperature + TEMP_UNIT;
        String humid = humidity + HUMIDITY_UNIT;
        String precip = precipitation + PRECIPITATION_UNIT;
        String snowLikely = willSnow() ? "likely" : "unlikely";

        System.out.println("The current temperature in " + cityName + " is " + temp
                + ". The current relative humidity is: " + humid
                + ". The current chance of precipitation is " + precip
                + ". It is " + snowLikely + " to snow.");
    }
}

public class Main {
    public static void main(String[] args) {
        WeatherData columbus = new WeatherData("Columbus", 30, 60, 75);
        columbus.displayWeatherReport();
    }
}
```

Here, we modified the `setTemperature()` *setter* method to check if the
temperature was less than -461.  If it is, the temperature field is set to -461
otherwise the field is set to whatever value was specified.

## Garbage Collection
When we create objects, we use the *new* operator.  As part of the process
of creating a new object, the *new* operator allocates space in memory to store
the object.  But how do we free memory when we no longer need the object?  If
memory weren't freed, we could eventually run out of available memory and the
program would stop.  While some languages require that objects be explicitly
removed from memory, Java takes care of this for us.  

Java provides a **garbage collector** that occasionally runs, checks for
unreferenced objects (or objects with references to each other but nothing else
referencing them), and frees the memory for any such objects it finds.  An
**unreferenced object** is an object that cannot be accessed from anywhere
else in the program. For example, `WeatherData columbus = new WeatherData();`
represents a referenced object because the new WeatherData object is accessible
using the `columbus` variable.  However, once `columbus` goes out of scope
or we assign a new value to it, the object might not be accessible from
anywhere else and is an unreferenced object.  Similarly, `new WeatherData()`
is an unreferenced object: the new object is not stored and not accessible.
While we don't have to worry about explicitly freeing memory, we should be
aware of references that exist to objects - especially if it appears that our
program is utilizing more memory than we expect.


## Exercise
Create a class that represents contact information for a person.  The class
should store the person's name and their email address.  

Create a second class that represents an address book (a collection of contact
information for many people) that includes methods for adding new contact
information and for searching the existing collection for a contacts email
address when the name is specified.

The program should create instances of the classes and demonstrate the
functionality.
