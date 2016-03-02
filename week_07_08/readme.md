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
