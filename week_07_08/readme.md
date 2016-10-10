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


To specify implementation inheritance between classes, Java provides the
reserved word *extends*.  The syntax for using extends is as follows:

```java
class BaseClass {
  // member declarations
}

class DerivedClass extends BaseClass {
  // member declarations
}
```

Here, *DerivedClass* inherits methods and fields from *BaseClass*.  A base
class is also be called a parent class or super class.  A derived class is also
known as a child class or subclass.  Let's look at an example.

```Java
package com.myname.week_07_08;

enum Direction {NORTH, SOUTH, EAST, WEST};

class Storm {
    private double latitude;
    private double longitude;
    private double speed;
    private Direction direction;

    public double getLatitude() {
        return latitude;
    }

    public void setLatitude(double latitude) {
        this.latitude = latitude;
    }

    public double getLongitude() {
        return longitude;
    }

    public void setLongitude(double longitude) {
        this.longitude = longitude;
    }

    public double getSpeed() {
        return speed;
    }

    public void setSpeed(double speed) {
        this.speed = speed;
    }

    public Direction getDirection() {
        return direction;
    }

    public void setDirection(Direction direction) {
        this.direction = direction;
    }

    public void display() {
        System.out.println("The storm is currently located at ("
                + latitude + ", " + longitude + ") moving "
                + speed + "MPH " + direction + ".");
    }
}

class ThunderStorm extends Storm {

}

public class Main {
    public static void main(String[] args) {
        ThunderStorm thunderStorm = new ThunderStorm();
        thunderStorm.setLatitude(39.970456);
        thunderStorm.setLongitude(-82.988770);
        thunderStorm.setSpeed(5);
        thunderStorm.setDirection(Direction.EAST);

        thunderStorm.display();
    }
}
```

In this example, we have a class, *Storm*, with fields and methods.  We also
have another class, *ThunderStorm*, that extends, or inherits from, *Storm*.
In Main.main(), we create an instance of ThunderStorm and, because it inherits
from Storm, make use of methods defined in the Storm class.

We can add additional fields and methods to our subclass to represent state
or behaviors specific to the subclass but not appropriate for the base class.

```Java
package com.myname.week_07_08;

enum Direction {NORTH, SOUTH, EAST, WEST};

class Storm {
    private double latitude;
    private double longitude;
    private double speed;
    private Direction direction;

    public double getLatitude() {
        return latitude;
    }

    public void setLatitude(double latitude) {
        this.latitude = latitude;
    }

    public double getLongitude() {
        return longitude;
    }

    public void setLongitude(double longitude) {
        this.longitude = longitude;
    }

    public double getSpeed() {
        return speed;
    }

    public void setSpeed(double speed) {
        this.speed = speed;
    }

    public Direction getDirection() {
        return direction;
    }

    public void setDirection(Direction direction) {
        this.direction = direction;
    }

    public void display() {
        System.out.println("The storm is currently located at ("
                + latitude + ", " + longitude + ") moving "
                + speed + "MPH " + direction + ".");
    }
}

class ThunderStorm extends Storm {
    private int numberOfLightningStrikes;

    public int getNumberOfLightningStrikes() {
        return numberOfLightningStrikes;
    }

    public void setNumberOfLightningStrikes(int numberOfLightningStrikes) {
        this.numberOfLightningStrikes = numberOfLightningStrikes;
    }

    public void displayLightningStrikes() {
        String message;
        if (numberOfLightningStrikes == 1) {
            message = "There has been 1 lightning strike.";
        }
        else {
            message = "There have been " + numberOfLightningStrikes + " lightning strikes.";
        }
        System.out.println(message);
    }
}


public class Main {
    public static void main(String[] args) {
        ThunderStorm thunderStorm = new ThunderStorm();
        thunderStorm.setLatitude(39.970456);
        thunderStorm.setLongitude(-82.988770);
        thunderStorm.setSpeed(5);
        thunderStorm.setDirection(Direction.EAST);
        thunderStorm.setNumberOfLightningStrikes(10);

        thunderStorm.display();
        thunderStorm.displayLightningStrikes();
    }
}
```

We've added a field to track the number of lightning strikes and some methods
that make use of that field to the ThunderStorm class.  If we tried to set the
number of lightning strikes on an instance of the Storm class, our program
would not compile because the corresponding method and field only exists for
the ThunderStorm class.

What if we wanted to make use of the private fields declared in the Storm class
in code that we will add to the ThunderStorm class?  As the code is written in
the previous example, we cannot directly access any of the fields in Storm
because they are private.  When we called display() from our ThunderStorm
object, we were able to access the private fields because the display() method
was defined in the Storm class, the same class as the private fields.
In order to grant subclasses access to fields but to prevent other classes from
directly accessing them, we can use the *protected* access control modifier.

```java
package com.myname.week_07_08;

enum Direction {NORTH, SOUTH, EAST, WEST};

class Storm {
    protected double latitude;
    protected double longitude;
    protected double speed;
    protected Direction direction;

    public double getLatitude() {
        return latitude;
    }

    public void setLatitude(double latitude) {
        this.latitude = latitude;
    }

    public double getLongitude() {
        return longitude;
    }

    public void setLongitude(double longitude) {
        this.longitude = longitude;
    }

    public double getSpeed() {
        return speed;
    }

    public void setSpeed(double speed) {
        this.speed = speed;
    }

    public Direction getDirection() {
        return direction;
    }

    public void setDirection(Direction direction) {
        this.direction = direction;
    }

    public void display() {
        System.out.println("The storm is currently located at ("
                + latitude + ", " + longitude + ") moving "
                + speed + "MPH " + direction + ".");
    }
}

class ThunderStorm extends Storm {
    private int numberOfLightningStrikes;

    public int getNumberOfLightningStrikes() {
        return numberOfLightningStrikes;
    }

    public void setNumberOfLightningStrikes(int numberOfLightningStrikes) {
        this.numberOfLightningStrikes = numberOfLightningStrikes;
    }

    public void displayLightningStrikes() {
        String message;
        if (numberOfLightningStrikes == 1) {
            message = "There has been 1 lightning strike";
        }
        else {
            message = "There have been " + numberOfLightningStrikes + " lightning strikes";
        }
        message += " for the storm at (" + latitude + ", " + longitude + ").";
        System.out.println(message);
    }
}


public class Main {
    public static void main(String[] args) {
        ThunderStorm thunderStorm = new ThunderStorm();
        thunderStorm.setLatitude(39.970456);
        thunderStorm.setLongitude(-82.988770);
        thunderStorm.setSpeed(5);
        thunderStorm.setDirection(Direction.EAST);
        thunderStorm.setNumberOfLightningStrikes(10);

        thunderStorm.display();
        thunderStorm.displayLightningStrikes();
    }
}
```

By setting the latitude and longitude fields to protected in the base class,
we can directly access them from the subclass.  Alternatively, we could have
used the getters and setters provided by the base class since those are set to
be publicly accessible.

Let's add a constructor to our base class.

```java
package com.myname.week_07_08;

enum Direction {NORTH, SOUTH, EAST, WEST};

class Storm {
    private double latitude;
    private double longitude;
    private double speed;
    private Direction direction;

    Storm(){
    }

    Storm(double latitude, double longitude, double speed, Direction direction) {
        this.latitude = latitude;
        this.longitude = longitude;
        this.speed = speed;
        this.direction = direction;
    }


    //region getters and setters - this allows us to use IntelliJ code folding to hide a region of code
    public double getLatitude() {
        return latitude;
    }

    public void setLatitude(double latitude) {
        this.latitude = latitude;
    }

    public double getLongitude() {
        return longitude;
    }

    public void setLongitude(double longitude) {
        this.longitude = longitude;
    }

    public double getSpeed() {
        return speed;
    }

    public void setSpeed(double speed) {
        this.speed = speed;
    }

    public Direction getDirection() {
        return direction;
    }

    public void setDirection(Direction direction) {
        this.direction = direction;
    }
    //endregion

    public void display() {
        System.out.println("The storm is currently located at ("
                + latitude + ", " + longitude + ") moving "
                + speed + "MPH " + direction + ".");
    }
}

class ThunderStorm extends Storm {
    private int numberOfLightningStrikes;

    public int getNumberOfLightningStrikes() {
        return numberOfLightningStrikes;
    }

    public void setNumberOfLightningStrikes(int numberOfLightningStrikes) {
        this.numberOfLightningStrikes = numberOfLightningStrikes;
    }

    public void displayLightningStrikes() {
        String message;
        if (numberOfLightningStrikes == 1) {
            message = "There has been 1 lightning strike";
        }
        else {
            message = "There have been " + numberOfLightningStrikes + " lightning strikes";
        }
        message += " for the storm at (" + getLatitude() + ", " + getLongitude() + ").";
        System.out.println(message);
    }
}


public class Main {
    public static void main(String[] args) {
        ThunderStorm thunderStorm = new ThunderStorm();
        thunderStorm.setLatitude(39.970456);
        thunderStorm.setLongitude(-82.988770);
        thunderStorm.setSpeed(5);
        thunderStorm.setDirection(Direction.EAST);
        thunderStorm.setNumberOfLightningStrikes(10);

        thunderStorm.display();
        thunderStorm.displayLightningStrikes();
    }
}
```

Remember that when we explicitly specify a constructor, Java no longer provides
a default constructor.  When adding a constructor to Storm, we have to also
specify a constructor that acts like the default constructor.  The reason is
that ThunderStorm currently uses the default constructor and when it is
executed, it executes the base classes default constructor.  Notice that even
though we have specified a custom constructor for Storm, we can't use it to
create ThunderStorm objects.  We'll have to specify a constructor for the
ThunderStorm class.

Let's add a constructor that take's parameters for the latitude, longitude,
speed, direction, and number of lightning strikes.  We'll want to make use
of Storm's constructor. In order to do that, we'll use *super*, which is
similar to *this*.  We use *this* to refer to an instance in code that
appears within a class; *super* refers to a class's superclass.  From within
ThunderStorm, *super* corresponds to the storm class.  Calling super() from
within ThunderStorm corresponds to calling Storm's default constructor.  In the
code below, we'll use super() with parameters to access the custom constructor
we wrote for Storm from within the ThunderStorm class.

```java
package com.myname.week_07_08;

enum Direction {NORTH, SOUTH, EAST, WEST};

class Storm {
    private double latitude;
    private double longitude;
    private double speed;
    private Direction direction;

    Storm(){
    }

    Storm(double latitude, double longitude, double speed, Direction direction) {
        this.latitude = latitude;
        this.longitude = longitude;
        this.speed = speed;
        this.direction = direction;
    }


    //region getters and setters - this allows us to use IntelliJ code folding to hide a region of code
    public double getLatitude() {
        return latitude;
    }

    public void setLatitude(double latitude) {
        this.latitude = latitude;
    }

    public double getLongitude() {
        return longitude;
    }

    public void setLongitude(double longitude) {
        this.longitude = longitude;
    }

    public double getSpeed() {
        return speed;
    }

    public void setSpeed(double speed) {
        this.speed = speed;
    }

    public Direction getDirection() {
        return direction;
    }

    public void setDirection(Direction direction) {
        this.direction = direction;
    }
    //endregion

    public void display() {
        System.out.println("The storm is currently located at ("
                + latitude + ", " + longitude + ") moving "
                + speed + "MPH " + direction + ".");
    }
}

class ThunderStorm extends Storm {
    private int numberOfLightningStrikes;

    ThunderStorm(double latitude, double longitude, double speed, Direction direction, int numberOfLightningStrikes) {
        super(latitude, longitude, speed, direction);
        this.numberOfLightningStrikes = numberOfLightningStrikes;
    }

    public int getNumberOfLightningStrikes() {
        return numberOfLightningStrikes;
    }

    public void setNumberOfLightningStrikes(int numberOfLightningStrikes) {
        this.numberOfLightningStrikes = numberOfLightningStrikes;
    }

    public void displayLightningStrikes() {
        String message;
        if (numberOfLightningStrikes == 1) {
            message = "There has been 1 lightning strike";
        }
        else {
            message = "There have been " + numberOfLightningStrikes + " lightning strikes";
        }
        message += " for the storm at (" + getLatitude() + ", " + getLongitude() + ").";
        System.out.println(message);
    }
}


public class Main {
    public static void main(String[] args) {
        ThunderStorm thunderStorm = new ThunderStorm(-39.970456, -82.988770, 5, Direction.EAST, 10);

        thunderStorm.display();
        thunderStorm.displayLightningStrikes();
    }
}
```

Notice that in the ThunderStorm constructor, we call the superclass's
constructor using super(); this allows us to reuse Storm's constructor to
set latitude, longitude, speed, and direction.

We can also use super to access other methods in the base class from within the
derived class.  Suppose we want ThunderStorm to provide it's own display method
that includes code to display information about lightning strikes (like the
displayLightningStrikes() method) but still displays the same information as
display() currently does.

We can do this by *overriding* the base class's implementation and replacing it
it with an implementation tailored to the derived class.  We can use *super*
from within the derived class to access the base class's methods.

```java
package com.myname.week_07_08;

enum Direction {NORTH, SOUTH, EAST, WEST};

class Storm {
    private double latitude;
    private double longitude;
    private double speed;
    private Direction direction;

    Storm(){
    }

    Storm(double latitude, double longitude, double speed, Direction direction) {
        this.latitude = latitude;
        this.longitude = longitude;
        this.speed = speed;
        this.direction = direction;
    }


    //region getters and setters - this allows us to use IntelliJ code folding to hide a region of code
    public double getLatitude() {
        return latitude;
    }

    public void setLatitude(double latitude) {
        this.latitude = latitude;
    }

    public double getLongitude() {
        return longitude;
    }

    public void setLongitude(double longitude) {
        this.longitude = longitude;
    }

    public double getSpeed() {
        return speed;
    }

    public void setSpeed(double speed) {
        this.speed = speed;
    }

    public Direction getDirection() {
        return direction;
    }

    public void setDirection(Direction direction) {
        this.direction = direction;
    }
    //endregion

    public void display() {
        System.out.println("The storm is currently located at ("
                + latitude + ", " + longitude + ") moving "
                + speed + "MPH " + direction + ".");
    }
}

class ThunderStorm extends Storm {
    private int numberOfLightningStrikes;

    ThunderStorm(double latitude, double longitude, double speed, Direction direction, int numberOfLightningStrikes) {
        super(latitude, longitude, speed, direction);
        this.numberOfLightningStrikes = numberOfLightningStrikes;
    }

    public int getNumberOfLightningStrikes() {
        return numberOfLightningStrikes;
    }

    public void setNumberOfLightningStrikes(int numberOfLightningStrikes) {
        this.numberOfLightningStrikes = numberOfLightningStrikes;
    }

    public void display() {
        super.display();
        String message;
        if (numberOfLightningStrikes == 1) {
            message = "There has been 1 lightning strike";
        }
        else {
            message = "There have been " + numberOfLightningStrikes + " lightning strikes";
        }
        message += " for the storm at (" + getLatitude() + ", " + getLongitude() + ").";
        System.out.println(message);
    }
}


public class Main {
    public static void main(String[] args) {
        ThunderStorm thunderStorm = new ThunderStorm(-39.970456, -82.988770, 5, Direction.EAST, 10);

        thunderStorm.display();
    }
}
```

We've replaced displayLightningStrikes() in the ThunderStorm class with
display().  ThunderStorm.display() overrides Storm.display() but can still make
use of it by calling super.display().

It's important to note the difference between method overloading and method
overriding.  Overriding replaces functionality inherited by the base class and
overloading adds functionality.  To make it clear when we are overriding a
method as opposed to overloading a base class's method, we can prefix the
method with the `@Override` annotation.  Annotations are a form of metadata,
providing information about the program itself without affecting the program.
If we apply the `@Override` annotation to an overloaded method, the complier
will report an error.  Below is an example with both overriding and
overloading.

```java
package com.myname.week_07_08;

enum Direction {NORTH, SOUTH, EAST, WEST};

class Storm {
    private double latitude;
    private double longitude;
    private double speed;
    private Direction direction;

    Storm(){
    }

    Storm(double latitude, double longitude, double speed, Direction direction) {
        this.latitude = latitude;
        this.longitude = longitude;
        this.speed = speed;
        this.direction = direction;
    }


    //region getters and setters - this allows us to use IntelliJ code folding to hide a region of code
    public double getLatitude() {
        return latitude;
    }

    public void setLatitude(double latitude) {
        this.latitude = latitude;
    }

    public double getLongitude() {
        return longitude;
    }

    public void setLongitude(double longitude) {
        this.longitude = longitude;
    }

    public double getSpeed() {
        return speed;
    }

    public void setSpeed(double speed) {
        this.speed = speed;
    }

    public Direction getDirection() {
        return direction;
    }

    public void setDirection(Direction direction) {
        this.direction = direction;
    }
    //endregion

    public void display() {
        System.out.println("The storm is currently located at ("
                + latitude + ", " + longitude + ") moving "
                + speed + "MPH " + direction + ".");
    }
}

class ThunderStorm extends Storm {
    private int numberOfLightningStrikes;

    ThunderStorm(double latitude, double longitude, double speed, Direction direction, int numberOfLightningStrikes) {
        super(latitude, longitude, speed, direction);
        this.numberOfLightningStrikes = numberOfLightningStrikes;
    }

    public int getNumberOfLightningStrikes() {
        return numberOfLightningStrikes;
    }

    public void setNumberOfLightningStrikes(int numberOfLightningStrikes) {
        this.numberOfLightningStrikes = numberOfLightningStrikes;
    }

    @Override
    public void display() {
        super.display();
        String message;
        if (numberOfLightningStrikes == 1) {
            message = "There has been 1 lightning strike";
        }
        else {
            message = "There have been " + numberOfLightningStrikes + " lightning strikes";
        }
        message += " for the storm at (" + getLatitude() + ", " + getLongitude() + ").";
        System.out.println(message);
    }

    public void display(String message){
        display();
        System.out.println(message);
    }
}


public class Main {
    public static void main(String[] args) {
        ThunderStorm thunderStorm = new ThunderStorm(-39.970456, -82.988770, 5, Direction.EAST, 10);

        thunderStorm.display("Hello");
    }
}
```

### The Object Class
In Java, a class that doesn't explicitly extend another class extends the
Object class.  Object is the ultimate superclass because it serves as the
ancestor of every other class and doesn't extend any other class itself.
Object provides a set of methods that all classes inherit.  Some of these
methods are listed below.

| Method                     | Description                                            |
|:---------------------------|:-------------------------------------------------------|
| Object clone()             | Create and return a shallow copy of the current object |
| boolean equals(Object obj) | Determine if the current object is equal to *obj*      |
| void finalize()            | Finalize the current object                            |
| String toString()          | Return a string representation of the current object   |

Let's look at the euals() method; the online Java documentation has more
information about the other methods. The `==` and `!=` operators compare two
primitive values for equality or inequality.  With reference types, these
operators check if objects refer to the same object.  Two objects are equal in
this sense if they occupy the same location in memory. This is known as an
**identity check** and two objects that are equal based on an identity check
are said to have reference equality. Often, this type of equality is not what
we mean when we say two things are equal.  For example, if we have a class that
represents contact information with name and email fields, we might say that
two contacts are "equal" if the name and email fields are the same.  The
equals() method in the Object class compares references, so we need to override
the method if we want to compare objects' fields.

Consider the following example:

```java
package com.myname.week_07_08;

class Contact {
    public String name;
    public String email;

    Contact(String name, String email){
        this.name = name;
        this.email = email;
    }

    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof Contact)) {
            return false;
        }
        Contact other = (Contact) obj;
        return this.name.equals(other.name) && this.email.equals(other.email);
    }

}


public class Main {
    public static void main(String[] args) {
        Contact bob = new Contact("bob", "bob@bob.com");
        Contact bob2 = new Contact("bob", "bob@bob.com");

        System.out.println("Reference equality: " + (bob == bob2));
        System.out.println("Equals method: " + bob.equals(bob2));
    }
}
```

In the *Object* class, the equals() method depends on reference equality.  We'd
like to say that two contacts are the same if their names and email addresses
are the same, so we have to override the method.  Because the equals() method
takes an *Object* as a parameter as its defined in the *Object* class, our
implementation has to take an *Object* as well.  The first thing we do in our
method is to check if *obj* is an instance of the *Contact* class; if it is
not, then we know it's not equal to the *Contact* instance we're comparing it
to.  If *obj* is a *Contact* instance, we have to convert it from a general
*Object* type to our class's type, *Contact*.  To do this, we **cast** *obj* to
the *Contact* type. Casting a variable means converting its type.  To cast a
variable, we specify the desired type in parentheses.  If the variable can't be
cast, we would encounter a runtime exception so it's important that we check
that *obj* is an instance of *Contact* before casting it.  In this example,
we see that *bob* and *bob2* do not have reference equality but are "equal"
based on how we defined the equals() method.

### Composition
Implementation inheritance and the ability to reuse code can be very convenient
but there can be disadvantages to it.  Implementation inheritance breaks
encapsulation: a subclass relies on implementation details in the base class
and if the base implementation changes, the subclass could stop functioning as
expected.  While this isn't likely to be a problem when we control the code for
both the base class and derived class, we'll often find it useful to inherit
from code that we don't control such as a class in a third party library.
Changes to the base class can have unexpected effects on the derived class.

As an example, suppose a third-party library has a contact class similar to
this one:

```java
package com.myname.week_07_08;

public class Contact {
    public String name;
    public String email;

    Contact(String name, String email){
        this.name = name;
        this.email = email;
    }
}
```

We'd like to use this class but we'd also like to implement some additional
functionality - the ability to send an email, for example.  We can extend the
Contact class like this:

```java
package com.myname.week_07_08;

public class BetterContact extends Contact {
    BetterContact(String name, String email) {
        super(name, email);
    }

    public void sendEmail(String message) {
        // code to send an email containing a message
    }

}
```

And use the new BetterContact class:

package com.myname.week_07_08;

```java
public class Main {
    public static void main(String[] args) throws CloneNotSupportedException {
        BetterContact bob = new BetterContact("Bob", "bob@bob.com");
        bob.sendEmail("Hi");
    }
}
```

Suppose, that the maintainer of the Contact class releases an updated version
of their library and adds email functionality.  The new contact class might
look like this:

```
package com.myname.week_07_08;

public class Contact {
    public String name;
    public String email;

    Contact(String name, String email){
        this.name = name;
        this.email = email;
    }

    public boolean sendEmail(String message) {
        // code to send an email
        // will return false if unable to send
        return true;
    }

}
```

Our program no longer works because the return types of the sendEmail() method
differ (so it cannot act as an overriding method).

Composition offers an alternative to implementation inheritance.  Rather
than extending the Contact class, BetterContact can *wrap* the class by
keeping a private field representing a Contact object.

```java
package com.myname.week_07_08;

public class BetterContact {
    private Contact contact;

    BetterContact(String name, String email) {
        contact = new Contact(name, email);
    }

    public void sendEmail(String message) {
        // code to send an email containing a message
        // that can make use of the contact object
    }

}
```

In this example, we don't have to worry about conflicting method return
types and signatures.  When the Contact class is updated to include a
sendEmail() method, we can either leave the code in BetterContact unchanged or
call the new method - our program will run in either case.

Composition can also provide functionality similar to multiple implementation
inheritance.  A class can wrap many other classes.  For example, a we might
have classes representing wheels, engines, doors, and other car components.
A car class might wrap all of these classes.

```java
package com.myname.week_07_08;

class Wheel {
    //Wheel implementation
}

class Door {
    // Door implementation
}

class Engine {
    // Engine implementation
}

public class Car {
    private Wheel[] wheels;
    private Door[] doors;
    private Engine engine;

    Car() {
        wheels = new Wheel[] {new Wheel(), new Wheel(), new Wheel(), new Wheel()};
        doors = new Door[] {new Door(), new Door(), new Door(), new Door()};
        engine = new Engine();

    }
}
```

Car's implementation can now access the wheels, doors, and engine using the
values of the corresponding private fields.

Inheritance is often described as an "is-a" relationship: a car is a vehicle.
Composition is often described as a "has-a" relationship: a car has an engine,
a car has a wheel, etc.

## Polymorphism
Suppose we had a Shape class with a draw() method.  Suppose we also had several
kinds of shapes, Circle, Rectangle, Triangle, that all inherited from Shape but
provided their own implementations of the draw method.  If we were to create an
array of Shapes, `Shape[]`, that contained Circles, Rectangles, and Triangles
in addition to Shapes, we could call the draw() method associated with each.
The method would do different things depending on the instance.  Because
of this, we say that the Shape.draw() method has many forms or that it is
**polymorphic**.

Here is code based on the shape example:

```java
package com.myname.week_07_08;

class Shape {
    public void draw() {
        System.out.println("Drawing a shape.");
    }
}

class Triangle extends Shape {
    @Override
    public void draw() {
        System.out.println("Drawing a triangle.");
    }
}

class Rectangle extends Shape {
    @Override
    public void draw() {
        System.out.println("Drawing a rectangle.");
    }
}

class Circle extends Shape {
    @Override
    public void draw() {
        System.out.println("Drawing a circle.");
    }
}


public class Main {
   public static void main(String[] args) {
       Shape[] shapes = new Shape[] { new Triangle(), new Rectangle(), new Circle()};
       for (Shape shape: shapes) {
           shape.draw();
       }
   }
}
```

The output is:

```
Drawing a triangle.
Drawing a rectangle.
Drawing a circle.
```

Notice that though we were working with the Shape type in the for loop, the
subclasses' methods were used when we called Shape.draw().

Java supports four kinds of polymorphism.
- *coercion*: an operation serves multiple types through implicit conversion.
  for example, while division is defined for two integers or two doubles,
  division of an integer by a double is supported because the compiler coerces
  or implicitly converts the integer to a double.  Using the instance of a
  subclass for a method parameter that expects an instance of the superclass
  is another example of coercion.

- *overloading*: the same operator or method can take different parameters

- *parametric*: within a class declaration, a field name can associate with
  different types and a method can associate with different parameter and
  return types. We'll look at this further when we discuss generics.

- *subtype*: When a subtype/subclass is used in a supertype/superclass context,
  a call to the supertype's method results in execution of the subtype's
  method.  An example of this is calling Shape.draw() on elements in a Shape
  array when the elements are instances of Circle, Rectangle, and Triangle as
  we saw earlier.


### Upcasting and Late Binding
Consider the following example:

```java
package com.myname.week_07_08;

class Shape {
    private int centerX;
    private int centerY;

    Shape(int centerX, int centerY) {
        this.centerX = centerX;
        this.centerY = centerY;

    }

    public int getCenterX() {
        return centerX;
    }

    public int getCenterY() {
        return centerY;
    }

    public void draw() {
        System.out.println("Drawing a shape with center (" + centerX + ", " + centerY + ")");
    }
}


class Rectangle extends Shape {
    int length;
    int width;

    Rectangle(int centerX, int centerY, int length, int width) {
        super(centerX, centerY);
        this.length = length;
        this.width = width;
    }

    public int getLength() {
        return length;
    }

    public  int getWidth() {
        return width;
    }

    @Override
    public void draw() {
        System.out.println("Drawing a rectangle with length: " + length + " and width: " + width);
    }
}


public class Main {
   public static void main(String[] args) {
       Rectangle r = new Rectangle(1, 2, 5, 10);
       Shape s = r;
   }
}
```

Here, we don't have to explicitly cast the Rectangle object to a Shape object
with code like this:

```java
Shape s = (Shape) r;
```

because the Rectangle class provides the same interface as the Shape class (and
more) since Rectangle extends Shape; this is known as **upcasting** since we
are moving up the type hierarchy.  Notice that we relied on this feature when
we constructed an array of shapes in a previous example.  Building on that
functionality, let's look at this code:


```java
package com.myname.week_07_08;

class Shape {
    private int centerX;
    private int centerY;

    Shape(int centerX, int centerY) {
        this.centerX = centerX;
        this.centerY = centerY;

    }

    public int getCenterX() {
        return centerX;
    }

    public int getCenterY() {
        return centerY;
    }

    public void draw() {
        System.out.println("Drawing a shape with center (" + centerX + ", " + centerY + ")");
    }
}


class Rectangle extends Shape {
    int length;
    int width;

    Rectangle(int centerX, int centerY, int length, int width) {
        super(centerX, centerY);
        this.length = length;
        this.width = width;
    }

    public int getLength() {
        return length;
    }

    public  int getWidth() {
        return width;
    }

    @Override
    public void draw() {
        System.out.println("Drawing a rectangle with length: " + length + " and width: " + width);
    }
}


public class Main {
   public static void main(String[] args) {
       Shape s = new Shape(5, 6);
       Rectangle r = new Rectangle(1, 2, 5, 10);
       Shape[] shapes = new Shape[] {s, r};
       for (Shape shape: shapes) {
           shape.draw();
       }
   }
}
```

Though the Rectangle object, *r*, is upcast to a Shape so it can be stored in
the Shape array, *shapes*, when its draw method is called, we see that the
draw method defined for the Rectangle class is used.  When the code is
compiled, the compiler doesn't know which method to use - only that a method
with the appropriate name and parameters exists on the superclass.  The
compiler inserts an instruction in the compiled code that examines the object
at runtime to call the correct method.  This is known as **late binding**.
If a method is declared using the reserved word `final`, it cannot be
overridden and the compiler knows what method to call; this is known as
**early binding**.

### Downcasting
Upcasting allows us to move up the inheritance type hierarchy.  We an also move
down the hierarchy by explicitly casting an instance of a superclass to an
instance of a subclass.  This is known as **down casting**.  Consider this
example:

```java
package com.myname.week_07_08;

class Shape {
    private int centerX;
    private int centerY;

    Shape(int centerX, int centerY) {
        setCenter(centerX, centerY);

    }

    public int getCenterX() {
        return centerX;
    }

    public int getCenterY() {
        return centerY;
    }

    public void setCenter(int centerX, int centerY) {
        this.centerX = centerX;
        this.centerY = centerY;
    }

    public void draw() {
        System.out.println("Drawing a shape with center (" + centerX + ", " + centerY + ")");
    }
}


class Rectangle extends Shape {
    int length;
    int width;

    Rectangle(int centerX, int centerY, int length, int width) {
        super(centerX, centerY);
        this.length = length;
        this.width = width;
    }

    public int getLength() {
        return length;
    }

    public  int getWidth() {
        return width;
    }

    @Override
    public void draw() {
        System.out.println("Drawing a rectangle with length: " + length + " and width: " + width);
    }
}


public class Main {
    public static Shape moveRight(Shape s) {
        int newCenterX = s.getCenterX() + 1;
        s.setCenter(newCenterX, s.getCenterY());
        return s;
    }

    public static void main(String[] args) {
        Rectangle r = new Rectangle(1, 2, 5, 10);
        Shape s = moveRight(r);
        r = (Rectangle) s;
        System.out.println(r.getLength() + ", " + r.getWidth());
    }
}
```

In the Main.main() method, we create a Rectangle object then use the
Main.moveRight() method.  The moveRight() method takes a Shape parameter but
we are able to specify a Rectangle thanks to upcasting.  However, the method
returns a Shape object.  We know that the method modifies the object we provide
rather than creating a new and we'd like to continue to use our object as a
Rectangle rather than the more generic Shape.  In order to access Rectangle's
unique methods, we have to downcast the shape returned by the moveRight()
method back to a Rectangle.

While upcasting is always safe, downcasting is not.  Consider this example:

```Java
package com.myname.week_07_08;

class Shape {
    private int centerX;
    private int centerY;

    Shape(int centerX, int centerY) {
        setCenter(centerX, centerY);

    }

    public int getCenterX() {
        return centerX;
    }

    public int getCenterY() {
        return centerY;
    }

    public void setCenter(int centerX, int centerY) {
        this.centerX = centerX;
        this.centerY = centerY;
    }

    public void draw() {
        System.out.println("Drawing a shape with center (" + centerX + ", " + centerY + ")");
    }
}


class Rectangle extends Shape {
    int length;
    int width;

    Rectangle(int centerX, int centerY, int length, int width) {
        super(centerX, centerY);
        this.length = length;
        this.width = width;
    }

    public int getLength() {
        return length;
    }

    public  int getWidth() {
        return width;
    }

    @Override
    public void draw() {
        System.out.println("Drawing a rectangle with length: " + length + " and width: " + width);
    }
}


public class Main {
    public static Shape moveRight(Shape s) {
        int newCenterX = s.getCenterX() + 1;
        s.setCenter(newCenterX, s.getCenterY());
        return s;
    }

    public static void main(String[] args) {
        Shape s = new Shape(1, 2);
        s = moveRight(s);
        Rectangle r = (Rectangle) s;
        System.out.println(r.getLength() + ", " + r.getWidth());
    }
}
```

Running this code will result in a *ClassCastException* because we are trying
to cast an object to the Rectangle class when the object wasn't a Rectangle to
begin with.

## Exercises
1. Write a program that includes a class representing contact information for
a person including their name and email address.  This class should include
a method for displaying the contact's name and email address.  The program
should also include a class for business contacts that extends the contact
class and stores the contact's phone number.  The business contact class
should override the base class's method that displays the name and email
address so that it displays the phone number in addition to the name and email
address. Create instances of both classes to demonstrate functionality.

2. Add a class to the previous example that represents a collection of
contacts, both of the base class and the derived class.  The collection class
should include a single method to add a contact, regardless of class, to the
collection - this demonstrates upcasting.  The collection class should also
include a method to iterate through the contacts and call their display
methods - this demonstrates late binding.
