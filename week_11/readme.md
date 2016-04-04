# Week 11 - Listeners and Anonymous Classes

## Corresponding Text
*Learn Java for Android Development*, pp. 197-203

## Nested Classes
Classes that are declared outside of any other class are known as **top-level
classes**.  Java allows us to declare classes within other classes; classes
that are declared as members of other classes are known as **nested classes**.  
There are four kinds of nested classes:
1. static member classes,
2. nonstatic member classes,
3. anonymous classes, and
4. local classes.

While we're likely to see anonymous classes more often than the other kinds of
nested classes, we'll look at all four.

### Static Member Classes
A **static member class** is a static class enclosed in and a member of another
class.  As an enclosed in another class, it has access to the outer
class's methods and fields but because it is static, it only has access to
the static methods and fields.  Consider the following example.  Suppose we
wanted to create a class for rectangles that stored each rectangle's width and
height and provided a method for calculating area.  We want to have the option
of storing height and width as either *doubles* or *floats* depending on
whether we need a lot of precision or if we don't need to waste memory storing
a double.  We could use this code:

```java
package com.myname.week_11;

abstract class Rectangle {
    abstract public double getWidth();
    abstract public double getHeight();

    //static member class
    static class DoubleRectangle extends Rectangle {
        private double width, height;

        DoubleRectangle(double width, double height) {
            this.width = width;
            this.height = height;
        }

        @Override
        public double getWidth() {
            return width;
        }

        @Override
        public double getHeight() {
            return height;
        }
    }

    //static member class
    static class FloatRectangle extends Rectangle {
        private float width, height;

        FloatRectangle(float width, float height) {
            this.width = width;
            this.height = height;
        }

        @Override
        public double getWidth() {
            return width;
        }

        @Override
        public double getHeight() {
            return height;
        }
    }

    private Rectangle() {}

    double getArea() {
        return getHeight() * getWidth();
    }
}

public class Main {
    public static void main(String[] args) {
        Rectangle r1 = new Rectangle.DoubleRectangle(10.0, 20.0);
        Rectangle r2 = new Rectangle.FloatRectangle(5.0f, 8.0f);
        System.out.println("Area of first rectangle: " + r1.getWidth());
        System.out.println("Area of second rectangle: " + r2.getWidth());

    }
}
```

Here, the abstract class *Rectangle* encloses two static member classes:
*DoubleRectangle* and *FloatRectangle*.  *DoubleRectangle* stores width and
height as doubles and *FloatRectangle* stores them as floats.  Because they
are static member classes, we don't need an instance of *Rectangle* in order
to access them.

Note also that we made the *Rectangle* constructor private.  This replaces
the default constructor and, because it is private, *Rectangle* cannot be
subclassed by non-member classes.

### Nonstatic Member Classes
A **nonstatic member class** is a non-static class enclosed in and a member of
another class.  An instance of a nonstatic member class is implicitly
associated an instance of the enclosing class and is able to access the outer
class's instance methods and fields.  

```java
package com.myname.week_11;

import java.util.ArrayList;
import java.util.List;


class AddressBook {
    private List<Contact> addresses = new ArrayList<>();
    private String name;

    public AddressBook(String name) {
        this.name = name;
    }

    public void addAddress(String name, String address) {
        Contact newContact = new Contact(name, address);
        addresses.add(newContact);
    }

    public String[] getAddresses() {
        String[] allAddresses = new String[addresses.size()];
        for (int i=0; i < addresses.size(); i++) {
            allAddresses[i] = addresses.get(i).toString();
        }
        return allAddresses;
    }

    //nonstatic member class
    private class Contact {
        private String name;
        private String address;

        Contact(String name, String address) {
            this.name = name;
            this.address = address;
        }

        @Override
        public String toString() {
            return "Name: " + name + ", Address: " + address;
        }
    }
}


public class Main {
    public static void main(String[] args) {
        AddressBook personal = new AddressBook("Personal");
        personal.addAddress("Bob", "123 Main St");
        personal.addAddress("Sue", "456 High St");

        for (String address: personal.getAddresses()) {
            System.out.println(address);
        }
    }
}
```

In this example, we created a class to provide some address book functionality.
The address book keeps a collection of individual addresses with a name
associated with each.  The *AddressBook* class contains an *addAddress()*
method that creates an instance of the nonstatic member class, *Contact*, and
adds the new instance to a list.  

### Anonymous Classes
An **anonymous class** is a class without a name.  It is also not a member of
its enclosing class.  An anonymous class is simultaneously declared and
instantiated in one place.  An anonymous class can be an extension of a class
or an implementation of an interface.  

```java
package com.myname.week_11;

import java.util.Random;

interface WeatherReport {
    public void display(double value);
}

class WeatherApp {
    private double temperature;

    public void updateTemperature() {
        temperature = new Random().nextDouble() * 100;
    }

    //anonymous class implementing WeatherReport
    public void displayTemperatureReport() {
        new WeatherReport() {
            @Override
            public void display(double value) {
                System.out.println("The current temperature is " + value + ".");
            }
        }.display(temperature);
    }
}
public class Main {
    public static void main(String[] args) {
        WeatherApp weather = new WeatherApp();
        weather.updateTemperature();
        weather.displayTemperatureReport();
    }
}
```

In this example, we have an interface for reporting weather conditions that
declares a *display()* method.  Our *WeatherApp* class makes use of the
*WeatherReport* interface in its *displayTemperatureReport()* method.  Rather
than creating a class that implements the interface, creating an instance of
the class, and storing it in a local variable, we can use a anonymous class
instead.  Notice that we can call the *display()* method of the anonymous
class.

Let's look at another example using an anonymous class.  Suppose we have a
class that stores people's names.  The class will make use of a list to store
the names.  We'd like to be able to add functionality to the class that allows
us to filter the list of names.  Rather than hard-coding a specific type of
filter - do the names begin with a certain letter, do the names have certain
length, etc - we can create an interface with a method used determine if a
name string is what we want or not.  When we want to use the filtering method,
we'll have to specify the filter using an anonymous class.

```java
package com.myname.week_11;

import java.util.ArrayList;
import java.util.List;

interface StringFilter {
    public boolean accept(String candidate);
}

class Names {
    private List<String> names = new ArrayList<>();

    public void add(String name) {
        names.add(name);
    }

    public List<String> filter(StringFilter filter) {
        List<String> filteredNames = new ArrayList<>();
        for (String name: names) {
            if (filter.accept(name)) {
                filteredNames.add(name);
            }
        }
        return filteredNames;
    }

}

public class Main {
    public static void main(String[] args ) {
        Names friends = new Names();
        friends.add("arthur");
        friends.add("bob");
        friends.add("sue");

        //anonymous class implementing StringFilter
        List<String> bNames = friends.filter(new StringFilter() {
            @Override
            public boolean accept(String candidate) {
                if (candidate.startsWith("b")) {
                    return true;
                }
                return false;
            }
        });

        for (String name: bNames) {
            System.out.println(name);
        }
    }
}
```

Here, the *Names* class has a *filter()* method that takes a single parameter,
a *StringFilter* object that can be used to determine if each string in the
list should be part of the new list or not.  In *Main.main()*, we use the
*filter()* method but have to provide an implementation of the *StringFilter*
instance.  We do this with an anonymous class in which we specify the details
of the filter's *accept()* method.  Specifically, the filter looks for strings
that begin with the letter 'b'.  

Note that instead of using an anonymous class, we could have created an
class that implemented the *StringFilter* interface, created an instance, and
used the instance with the *filter()* method.  

```java
package com.myname.week_11;

import java.util.ArrayList;
import java.util.List;

interface StringFilter {
    public boolean accept(String candidate);
}

class Names {
    private List<String> names = new ArrayList<>();

    public void add(String name) {
        names.add(name);
    }

    public List<String> filter(StringFilter filter) {
        List<String> filteredNames = new ArrayList<>();
        for (String name: names) {
            if (filter.accept(name)) {
                filteredNames.add(name);
            }
        }
        return filteredNames;
    }

}

//alternative to anonymous class implementing StringFilter
class BFilter implements StringFilter {
    @Override
    public boolean accept(String candidate) {
        if (candidate.startsWith("b")) {
            return true;
        }
        return false;
    }
}

public class Main {
    public static void main(String[] args ) {
        Names friends = new Names();
        friends.add("arthur");
        friends.add("bob");
        friends.add("sue");
        StringFilter bFilter = new BFilter();
        List<String> bNames = friends.filter(bFilter);

        for (String name: bNames) {
            System.out.println(name);
        }
    }
}
```

Notice that an anonymous class is much more convenient, especially if we never
need to use the *BFilter* class again.

### Local Classes
A **local class** is a class declared anywhere that a local variable can be
declared.  A local class has a name and can be reused.  A local class can
access the variables in the surrounding scope but they must be declared final.  

Continuing with the last example, suppose we want to create a class that we
could use to produce a variety of different filters - objects implementing
the *StringFilter* interface.

```java
package com.myname.week_11;

import java.util.ArrayList;
import java.util.List;

interface StringFilter {
    public boolean accept(String candidate);
}

class Names {
    private List<String> names = new ArrayList<>();

    public void add(String name) {
        names.add(name);
    }

    public List<String> filter(StringFilter filter) {
        List<String> filteredNames = new ArrayList<>();
        for (String name: names) {
            if (filter.accept(name)) {
                filteredNames.add(name);
            }
        }
        return filteredNames;
    }

}

class FilterCreator{
    public static StringFilter startsWtihFilter(String prefix) {
        //local class
        class StartsWithFilter implements StringFilter {
            @Override
            public boolean accept(String candidate) {
                if (candidate.startsWith("b")) {
                    return true;
                }
                return false;
            }
        }
        return new StartsWithFilter();
    }
}

public class Main {
    public static void main(String[] args ) {
        Names friends = new Names();
        friends.add("arthur");
        friends.add("bob");
        friends.add("sue");
        StringFilter bFilter = FilterCreator.startsWtihFilter("b");
        List<String> bNames = friends.filter(bFilter);

        for (String name: bNames) {
            System.out.println(name);
        }
    }
}
```

Here, the *FilterCreator.startsWtihFilter()* method declares a class that
implements *StringFilter* within it's body, creates an instance, and returns
the new instance.  Because the *StartsWithFilter* class is declared within
a method and not within the *FilterCreator* class body, it is not a member
class.

## Listeners

### A more complicated example
Examples adapted from *Head First: Design Patterns* by Eric Freeman and
Elisabeth Robson

See `java.util.Observer` and `java.util.Observable`.

### UI Event Handlers
