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
Often in our programs, objects will depend on information from other objects.
Suppose we are writing an app for a news app.  Two classes we might create
are a *ConsoleViewer* that displays news items and a *NewsCollection* that
maintains a collection of news items.  One way we can update the the
*ConsoleViewer* is by having it regularly check with a *NewCollection* instance
for new items. Alternatively, we can write *NewsCollection* to update
*ConsoleViewer* instances when new items are added to the collection.  Let's
look at how we might do this.

```java
package com.myname.week_11;

import java.util.ArrayList;
import java.util.List;

class NewsItem {
    private String title;
    private String content;

    NewsItem(String title, String content) {
        this.title = title;
        this.content = content;
    }

    public String getTitle() {
        return title;
    }

    public String getContent() {
        return content;
    }
}

interface NewsViewer {
    public void update(NewsItem item);
}

class NewsCollection {
    private List<NewsItem> collection = new ArrayList<>();
    private List<NewsViewer> viewers = new ArrayList<>();

    // add a listener
    public void addViewer(NewsViewer viewer) {
        viewers.add(viewer);
    }

    // update listeners
    public void updateViewers(NewsItem item) {
        for (NewsViewer viewer: viewers) {
            viewer.update(item);
        }
    }

    public void addNewsItem(NewsItem item) {
        collection.add(item);
        updateViewers(item);
    }

    public void addNewsItem(String title, String content) {
        addNewsItem(new NewsItem(title, content));
    }
}

class ConsoleViewer implements NewsViewer{
    // method called by objects that update listeners
    public void update(NewsItem item){
        System.out.println("Breaking News (displayed by ConsoleViewer): " + item.getTitle());
    }
}

public class Main {
    public static void main(String[] args) {
        NewsViewer consoleNews = new ConsoleViewer();
        NewsCollection allTheNews = new NewsCollection();
        allTheNews.addViewer(consoleNews);

        allTheNews.addNewsItem("Lotto Winner", "Bob won the lottery!");
        allTheNews.addNewsItem("Forecast", "It's going to be nice outside.");
    }
}
```

In this code, we first created a *NewsItem* class to represent individual news
items with a title and content.  Next, we declared a *NewsViewer* interface
that declared a single method: *update()* which has a single *NewsItem*
parameter; this provides a standard way of updating listeners.  The
*NewsCollection* class is responsible for keeping a collection of news items,
providing functionality for adding to the collection, and notifying any
listeners of any updates to the collection.  The *ConsoleViewer* class displays
information about new news items on the console.  It also implements the
*NewsViewer* interface.  In *Main.main()*, we create instances of both
*NewsViewer* and *NewsCollection*. We then register the instance of
*NewsViewer* as a listener with the instance of *NewsCollection*.  When we add
news items to the instance of *NewsCollection*, the instance notifies the
instance of *ConsoleViewer* by calling the *ConsoleViewer.update()* method.

One advantage to writing code like this is that it is easy to add another
listener.  If we wrote a *GUIViewer* class that implemented the *NewsViewer*
interface, we could register it as a listener with the *NewsCollection* object
and the *GUIViewer* instance would automatically be updated with new news
items. The alternative is to have the individual viewers regularly check with
the *NewsCollection* instance for new items.  

Let's looks at another example.  This example is adapted from *Head First:
Design Patterns* by Eric Freeman and Elisabeth Robson.  Suppose we wanted to
write a weather monitoring application.  The application will depend on data
from temperature and humidity sensors. A coordinating weather station will
act as a listeners for updates from the sensors. The sensors will pass data to
the listener based on a weather data class.  Here's the code for such an
application:

```java
package com.myname.week_11;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Random;

interface WeatherDataSource {
    public void addListener(WeatherDataListener listener);
    public void removeListener(WeatherDataListener listener);
    public void update();
}

interface WeatherDataListener {
    public void updateData(WeatherData newData);
}

abstract class WeatherData {
    private String dataType;
    private Double measuredValue;

    WeatherData(String dataType, Double measuredValue) {
        this.dataType = dataType;
        this.measuredValue = measuredValue;
    }

    public String getDataType() {
        return dataType;
    }

    public Double getMeasuredValue() {
        return measuredValue;
    }

    abstract public String getUpdateMessage();
}

class TemperatureSensor implements WeatherDataSource {
    private double currentTemperature;
    private List<WeatherDataListener> listeners = new ArrayList<>();

    TemperatureSensor() {
        updateTemperature();
    }

    public double getCurrentTemperature() {
        return currentTemperature;
    }

    private void updateTemperature() {
        // read from humidity sensor
        currentTemperature = new Random().nextDouble() * 100;
    }

    @Override
    public void addListener(WeatherDataListener listener) {
        listeners.add(listener);
    }

    @Override
    public void removeListener(WeatherDataListener listener) {
        listeners.remove(listener);
    }

    @Override
    public void update() {
        System.out.println("TemperatureSensor: getting new data.");
        updateTemperature();
        for (WeatherDataListener listener : listeners) {
            listener.updateData(new WeatherData("temperature", currentTemperature) {
                @Override
                public String getUpdateMessage() {
                    return "The new temperature is " + currentTemperature;
                }
            });
        }

    }
}

class HumiditySensor implements WeatherDataSource {
    private double currentHumidity;
    private List<WeatherDataListener> listeners = new ArrayList<>();

    HumiditySensor() {
        updateHumidity();
    }

    public double getCurrentHumidity() {
        return currentHumidity;
    }

    private void updateHumidity() {
        // read from humidity sensor
        currentHumidity = new Random().nextDouble();
    }

    @Override
    public void addListener(WeatherDataListener listener) {
        listeners.add(listener);
    }

    @Override
    public void removeListener(WeatherDataListener listener) {
        listeners.remove(listener);
    }

    @Override
    public void update() {
        System.out.println("HumiditySensor: getting new data.");
        updateHumidity();
        for (WeatherDataListener listener: listeners) {
            listener.updateData(new WeatherData("humidity", currentHumidity) {
                @Override
                public String getUpdateMessage() {
                    return "Humidity updated to " + currentHumidity;
                }
            });
        }

    }

}

class WeatherStation implements WeatherDataListener {
    private Map<String, Double> allWeatherData = new HashMap<>();
    private List<String> log = new ArrayList<>();

    @Override
    public void updateData(WeatherData newData) {
        System.out.println("WeatherStation: Updating the weather station data with new "
                + newData.getDataType() + " data.");
        allWeatherData.put(newData.getDataType(), newData.getMeasuredValue());
        log.add(newData.getUpdateMessage());
    }

    public void displayCurrentWeather() {
        System.out.println("Weather Report");
        for (String dataType: allWeatherData.keySet()) {
            System.out.println(dataType + ": " + allWeatherData.get(dataType));
        }
    }

    public void displayLog() {
        for (int i=0; i < log.size(); i++) {
            int currentLine = i + 1;
            System.out.println(currentLine + ": " + log.get(i));
        }
    }
}

public class Main {
    public static void main(String[] args) {
        System.out.println("Main: creating objects");
        WeatherStation localWeatherStation = new WeatherStation();

        TemperatureSensor temperatureSensor = new TemperatureSensor();
        HumiditySensor humiditySensor = new HumiditySensor();

        temperatureSensor.addListener(localWeatherStation);
        humiditySensor.addListener(localWeatherStation);

        System.out.println("Main: simulating updates from sensors");
        temperatureSensor.update();
        humiditySensor.update();
        temperatureSensor.update();

        System.out.println("Main: displaying report and logs");
        localWeatherStation.displayCurrentWeather();
        localWeatherStation.displayLog();

    }
}
```

We start by defining an interface for a data source and a data listener.  The
source must provide implementations for adding listeners, removing listeners,
and updating listeners as declared in the *WeatherDataSource* interface.  Any
listeners should implement the *WeatherDataListener* interface and provide an
implementation for a method that can be called to update the listener.  

Next, we define an abstract class, *WeatherData*, to represent individual
weather measurements.  In addition to storing the type of weather data and the
value, the *WeatherData* abstract class declares an abstract method that can be
used to get a message associated with the update - possibly for logging
purposes.

We create two classes to represent sensors: *TemperatureSensor* and
*HumiditySensor*.  In a real application, these classes would be responsible
for reading data from a sensor but in our example, they rely on random numbers.
Because they implement the *WeatherDataSource* interface, they have to
implement the methods associated with managing and updating listeners.  

The sensors make use of the *WeatherData* abstract class for sending
information to any listeners. Because *WeatherData* is abstract, we have to
provide an implementation for any of its abstract methods if we want to use it.
In our code, we do this through the use of an anonymous class when we need it.

We also create a class, *WeatherStation* that collects all weather data and
keeps a log of new weather items. We want an instance of the *WeatherStation*
class to be able to act as a listener to instances of the sensor classes so
*WeatherStation* implements the *WeatherDataListener* interface; its
*updateData()* method appends the new data to its list of weather data and
updates its log.  

In *Main.main()* we create instance of the sensors and *WeatherStation* and
register the *WeatherStation* instance as a listener with the the sensor
instances.  We then simulate updates to the sensors.  Notice that we don't need
to have the *WeatherStation* instance check with the sensors for new values -
the sensors notify the *WeatherStation* instance of new data.  We then use
the instance of *WeatherStation* to display the current weather conditions and
its log.

##Exercise
Modify the WeatherStation example from the lecture notes to include a
*PressureSensor* class that implements *WeatherDataSource* and reports
atmospheric pressure data; it should behave similar to *TemperatureSensor* and
*HumiditySensor*.  Update *Main.main()* to register the *WeatherStation*
instance as a listener for an instance of *PressureSensor* and demonstrate an
update of pressure data from the instance. 
