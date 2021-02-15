# Week 11 - Anonymous Classes and Listeners

## Corresponding Text

*Learn Java for Android Development*, pp. 197-203

## Nested Classes

Classes that are declared outside of any other class are known as **top-level
classes**.  Java allows us to declare classes within other classes; classes
that are declared within other classes are known as **nested classes**.
There are four kinds of nested classes:

1. static member classes,
2. nonstatic member classes,
3. anonymous classes, and
4. local classes.

While we're likely to see anonymous classes more often than the other kinds of
nested classes, we'll look at all four.

### Static Member Classes

A **static member class** is a static class enclosed in and a member of another
class.  As it is enclosed in another class, it has access to the outer
class's methods and fields but because it is static, it only has access to
the static methods and fields.  Consider the following example.  Suppose we
wanted to create a class for rectangles that stored each rectangle's width and
height and provided a method for calculating area.  We want to have the option
of storing height and width as either *doubles* or *floats* depending on
whether we need a lot of precision or if we don't need to waste memory storing
a double.  We could use this code:

---

Rectangle.java

``` java
package com.myname.myapplication;;

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
```

---

MainActivity.java

``` java
package com.myname.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    public void addText(StringBuilder builder, String text){
        builder.append(text);
        builder.append(System.lineSeparator());
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final TextView output = (TextView) findViewById((R.id.output));
        final EditText input = (EditText) findViewById(R.id.input);
        final Button button = (Button) findViewById(R.id.button);
        final StringBuilder builder = new StringBuilder();


        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Rectangle r1 = new Rectangle.DoubleRectangle(10.0, 20.0);
                Rectangle r2 = new Rectangle.FloatRectangle(5.0f, 8.0f);

                addText(builder, "Area of the first rectangle: " + r1.getArea());
                addText(builder, "Area of the second rectangle: " + r2.getArea());

                output.setText(builder);
            }
        });
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
associated with an instance of the enclosing class and is able to access the
outer class's instance methods and fields.

---

AddressBook.java

``` java
package com.myname.myapplication;

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
```

---

MainActivity.java

``` java
package com.myname.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    public void addText(StringBuilder builder, String text){
        builder.append(text);
        builder.append(System.lineSeparator());
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final TextView output = (TextView) findViewById((R.id.output));
        final EditText input = (EditText) findViewById(R.id.input);
        final Button button = (Button) findViewById(R.id.button);
        final StringBuilder builder = new StringBuilder();


        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                AddressBook personal = new AddressBook("Personal");
                personal.addAddress("Bob", "123 Main St");
                personal.addAddress("Sue", "456 High St");

                for (String address: personal.getAddresses()) {
                   addText(builder, address);
                }

                output.setText(builder);
            }
        });
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

---

WeatherApp.java

``` java
package com.myname.myapplication;

import java.util.Random;

interface WeatherReport {
    public String report(double value);
}

class WeatherApp {
    private double temperature;

    public void updateTemperature() {
        temperature = new Random().nextDouble() * 100;
    }

    //anonymous class implementing WeatherReport
    public String getTemperatureReport() {
        return new WeatherReport() {
            @Override
            public String report(double value) {
                return "The current temperature is " + value + ".";
            }
        }.report(temperature);
    }
}

```

---

MainActivity.java

``` java
package com.myname.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    public void addText(StringBuilder builder, String text){
        builder.append(text);
        builder.append(System.lineSeparator());
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final TextView output = (TextView) findViewById((R.id.output));
        final EditText input = (EditText) findViewById(R.id.input);
        final Button button = (Button) findViewById(R.id.button);
        final StringBuilder builder = new StringBuilder();


        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                WeatherApp weather = new WeatherApp();
                weather.updateTemperature();
                String report = weather.getTemperatureReport();
                addText(builder, report);
                output.setText(builder);
            }
        });
    }
}
```

In this example, we have an interface for reporting weather conditions that
declares a *report()* method.  Our *WeatherApp* class makes use of the
*WeatherReport* interface in its *getTemperatureReport()* method.  Rather
than creating a class that implements the interface, creating an instance of
the class, and storing it in a local variable, we can use a anonymous class
instead.  Notice that we can call the *report()* method of the anonymous
class and return its value in *getTemperatureReport()*.

Look carefully at the *MainActivity* class, specifically in the *onCreate()*
method. When we call *button.setOnClickListener()*, we use an anonymous class
based on *View.OnClickListener* interface.  Rather than defining a stand-alone
class that implements the *onClick()* method and then creating an instance of
that class, we've been using an anonymous class.  Generally, anonymous classes
are used when only once instance of the class will be created.

### Local Classes

A **local class** is a class declared anywhere that a local variable can be
declared.  A local class has a name and can be reused.  A local class can
access the variables in the surrounding scope but they must be declared final.

Consider this example based on an example at https://www.geeksforgeeks.org/local-inner-class-java/.

---

Divider.java

``` java
package com.myname.myapplication;

// adapted from https://www.geeksforgeeks.org/local-inner-class-java/
public class Divider {
    public String getValues() {
        final int initialValue = 20;

        // Local inner Class inside method
        class Inner {
            public int divisor;

            public Inner() {
                divisor = 4;
            }

            private int getDivisor() {
                return divisor;
            }

            private int getRemainder() {
                return initialValue % divisor;
            }

            private int getQuotient() {
                return initialValue / divisor;
            }
        }

        Inner inner = new Inner();

        StringBuilder builder = new StringBuilder();
        builder.append("Divisor = ");
        builder.append(inner.getDivisor());
        builder.append(", Remainder = ");
        builder.append(inner.getRemainder());
        builder.append(", Quotient = ");
        builder.append(inner.getQuotient());

        return builder.toString();
    }
}

```

---

MainActivity.java

``` java
package com.myname.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    public void addText(StringBuilder builder, String text){
        builder.append(text);
        builder.append(System.lineSeparator());
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final TextView output = (TextView) findViewById((R.id.output));
        final EditText input = (EditText) findViewById(R.id.input);
        final Button button = (Button) findViewById(R.id.button);
        final StringBuilder builder = new StringBuilder();


        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Divider divider = new Divider();
                addText(builder, divider.getValues());
                output.setText(builder);
            }
        });
    }
}
```

Here, the *Divider.getValues()* method declares a class that
implements the necessary code in its body, creates an instance, and returns
data from the new instance.  Because the *Inner* class is declared within
a method and not within the *Divider* class body, it is not a member
class.

## Listeners

Often in our programs, objects will depend on information from other objects.
Suppose we are writing a news app.  Two classes we might create
are a *NewsViewer* that displays news items and a *NewsCollection* that
maintains a collection of news items.  One way we can update the the
*NewsViewer* is by having it regularly check with a *NewsCollection* instance
for new items. Alternatively, we can write *NewsCollection* to update
*NewsViewer* instances when new items are added to the collection.  Let's
look at how we might do this.

---

NewsCollection.java

``` java
package com.myname.myapplication;
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

class NewsCollection {
    private List<NewsItem> collection = new ArrayList<>();
    private List<NewsListener> listeners = new ArrayList<>();

    // add a listener
    public void addListener(NewsListener listener) {
        listeners.add(listener);
    }

    // update listeners
    public void updateListeners(NewsItem item) {
        for (NewsListener listener: listeners) {
            listener.displayNews(item);
        }
    }

    public void addNewsItem(NewsItem item) {
        collection.add(item);
        updateListeners(item);
    }

    public void addNewsItem(String title, String content) {
        addNewsItem(new NewsItem(title, content));
    }
}
```

---

NewsViewer.java

``` java
package com.myname.myapplication;

import android.widget.TextView;

import org.w3c.dom.Text;

interface NewsListener {
    public void displayNews(NewsItem item);
}

class NewsViewer implements NewsListener{
    private TextView output;

    public NewsViewer(TextView output) {
        this.output = output;
    }

    // method called by objects that update listeners
    public void displayNews(NewsItem item){
        StringBuilder newsBuilder = new StringBuilder();
        newsBuilder.append(item.getTitle());
        newsBuilder.append(": ");
        newsBuilder.append(item.getContent());
        newsBuilder.append(System.lineSeparator());

        output.append(newsBuilder);
    }
}
```

---

MainActivity.java

``` java
package com.myname.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    public void addText(StringBuilder builder, String text){
        builder.append(text);
        builder.append(System.lineSeparator());
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final TextView output = (TextView) findViewById((R.id.output));
        final EditText input = (EditText) findViewById(R.id.input);
        final Button button = (Button) findViewById(R.id.button);
        final StringBuilder builder = new StringBuilder();


        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                NewsListener consoleNews = new NewsViewer(output);
                NewsCollection allTheNews = new NewsCollection();
                allTheNews.addListener(consoleNews);

                allTheNews.addNewsItem("Lotto Winner", "Bob won the lottery!");
                allTheNews.addNewsItem("Forecast", "It's going to be nice outside.");

            }
        });
    }
}
```

In this code, we first created a *NewsItem* class to represent individual news
items with a title and content. We also created a *NewsCollection* class to
keep a collection of news items, to provide functionality for adding to the
collection, and to notify any listeners of updates to the collection.
Next, we declared a *NewsListener* interface that declared a single method:
*displayNews()* which has a single *NewsItem* parameter; this provides a
standard way of updating listeners.  The *NewsViewer* class displays
information about new news items in a TextBox.  It also implements the
*NewsListener* interface.  In *MainActivity.onCreate()*, we create instances of
both *NewsViewer* and *NewsCollection*. We then register the instance of
*NewsViewer* as a listener with the instance of *NewsCollection*.  When we
add news items to the instance of *NewsCollection*, the instance notifies the
instance of *NewsViewer* by calling the *NewsViewer.displayNews()* method.

One advantage to writing code like this is that it is easy to add another
listener.  If we wrote a *NewsTextToSpeech* class that implemented the
*NewsListener* interface, we could register it as a listener with the
*NewsCollection* object and the new instance would automatically be updated
with new news items. The alternative is to have the individual viewers regularly
check with the *NewsCollection* instance for new items.

We've been working with listeners for some time.  In order to define the
functionality of the button in our app that we've been working with, we've had
to use listeners.  To see this, let's look again at the current code in
MainActivity.java.

---

MainActivity.java

``` java
package com.myname.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    public void addText(StringBuilder builder, String text){
        builder.append(text);
        builder.append(System.lineSeparator());
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final TextView output = (TextView) findViewById((R.id.output));
        final EditText input = (EditText) findViewById(R.id.input);
        final Button button = (Button) findViewById(R.id.button);
        final StringBuilder builder = new StringBuilder();


        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                NewsListener consoleNews = new NewsViewer(output);
                NewsCollection allTheNews = new NewsCollection();
                allTheNews.addListener(consoleNews);

                allTheNews.addNewsItem("Lotto Winner", "Bob won the lottery!");
                allTheNews.addNewsItem("Forecast", "It's going to be nice outside.");

            }
        });
    }
}
```

---

Notice that we use the *Button*'s *setOnClickListener()* method to define the
button's functionality when it's clicked.  This method allows us to associate
a listener with the button's click event.  The *setOnClickListener()* method
requires one parameter - an instance of a class that implements the
*View.OnClickListener* interface and defines a *onClick()* method.  In
our code, we've created an anonymous class that implements the interface
and provides the functionality we need.

## Exercise

Add an on-click listener to the output text box that clears the contents of
the text box when a user taps/clicks on the text box.  Use an anonymous
class to do this.
