# Week 4 - Collection Types and Exception Handling

An introduction to collections.

## Corresponding Text

*Learn Java for Android Development*, pp. 305-318, 404-425, 448-456

## Introduction to Classes, Instances, and Interfaces

You might have read or heard that Java is an object-oriented programming
language.  What does it mean to be object-oriented?  Basically, it means that
the code we write in Java makes use of objects created from classes.  A
**class** specifies a data structure containing related data attributes and
code for acting on these attributes. An **object** is a specific instance of a
class created using the *new* operator. A class specifies the general structure
and objects allow us to use specific values, unique to the object, and
perform actions based on or to those values.

Often, we describe a class as a combination of *state*, the attributes,
and *behavior*, the code that acts on the state.  We saw an example of an
attribute when we used the `.length` property of an array. The code that
represents behavior and acts on attributes are represented by methods. So far,
we've primarily worked with class methods but objects can have another type of
method know as instance methods. **Instance methods** are collections of code
that can alter the attributes of an individual object. An example of an
instance method is the *append()* method on *StringBuilder* objects.  This is
an instance method because it affects a specific instance of the
*StringBuilder* class.

In addition to classes, Java also has interfaces. An **interface** can be used
to specify what attributes and methods an object will have.  We can't create
objects directly from interfaces but we can use classes to *implement* the
interface. If a class implements an interface, we know that we will be able to
use the attributes and fields specified by the interface with the class and
objects created from it.

We'll discuss classes and interfaces in more detail later.

As an example of some of these concepts, let's look at the following code:

``` java
package com.myname.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final TextView output = (TextView) findViewById((R.id.output));
        final EditText input = (EditText) findViewById(R.id.input);
        Button button = (Button) findViewById(R.id.button);

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                StringBuilder builder = new StringBuilder();

                String testString = new String("Hello, ");
                testString = testString.concat(input.getText().toString());
                builder.append(testString);
                builder.append(System.lineSeparator());

                boolean hasHello = testString.contains("Hello");
                builder.append("'Hello' in string: " + hasHello);
                builder.append(System.lineSeparator());

                builder.append("Length: " + testString.length());
                builder.append(System.lineSeparator());

                output.setText(builder.toString());
            }
        });
    }
}
```

If you type *world* in the input, the output is:

``` text
After concat method: Hello, world!
'Hello' in string: true
Length: 13
```

In the code, we created an instance of the *StringBuilder* class named
`builder` and an instance of the *String* class named `testString`.  We made
use of the `concat()`, `contains()`, and `length()` methods of the *String*
object and the `append()` and `toString()` methods of the *StringBuilder*
object.

### Collection Interface

One of the things we'll be looking at is alternate ways of storing collections
of elements other than arrays.  One of these ways is by using lists.  Before we
look at lists, we should be aware of the *Collection* interface.  The
Collection interface specifies that certain methods will exist that we can use.
Something implementing the Collection interface will have the following methods
available.  There are additional methods but we'll focus on a subset.

| Method Signature           | Description                                                                                                                                                 |
|:---------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| boolean add(T e)           | Add an element, *e* of type *T* to the collection; returns true if the element is added and false otherwise                                                 |
| void clear()               | Remove all elements from the collection                                                                                                                     |
| boolean contains(Object o) | Returns true if the element *o* is present in the collection and false otherwise                                                                            |
| boolean isEmpty()          | Returns true when the collection has no elements and false otherwise                                                                                        |
| boolean remove(Object o)   | Removes the element identified as *o* from the collection; returns true if the element is removed and false otherwise                                       |
| int size()                 | Returns the number of elements in the collection or java.lang.Integer.MAX_VALUE if there are more elements than the the largest value represented by an int |
| Object[] toArray()         | Returns an array containing all the elements stored in the collection                                                                                       |

Another important thing to note is that you cannot store primitive-type-based
values in objects implementing Collection.  So, what if we want to create a
collections of values with numbers of type *int*?  We have to use primitive
type wrappers.

### Primitive Type Wrappers

**Primitive Type Wrappers** are classes that provide a way of representing
primitive-type-based values using reference types.  The primitive type based
wrappers are:

- Boolean
- Character
- Float and Double
- Byte, Short, Integer, and Long

They can be used by creating instances with the *new* operator and specifying a
value using either a literal or variable.

For example:

```java
int number = 10;
Integer wrappedInt = new Integer(number)

Character wrappedChar = new Character('A');
```

This creates two objects, *wrappedInt* and *wrappedChar*, that represent an
integer value and character value, respectively, but use reference types rather
than primitive types.

Another feature of primitive type wrappers is that they have various convenient
attributes and methods.  For example:

```java
package com.myname.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final TextView output = (TextView) findViewById((R.id.output));
        final EditText input = (EditText) findViewById(R.id.input);
        Button button = (Button) findViewById(R.id.button);

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
            }
        });

        StringBuilder builder = new StringBuilder();

        char letter = 'A';

        builder.append("isAlphabetic: " + Character.isAlphabetic(letter));
        builder.append(System.lineSeparator());

        builder.append("isLowerCase: " + Character.isLowerCase(letter));
        builder.append(System.lineSeparator());

        System.out.println("MAX_VALUE: " + Integer.MAX_VALUE);
        output.setText(builder.toString());

    }
}
```

Note that we didn't make any changes to the interface and removed code form the
*OnClickListener()* method for now.

## Lists

A **list** is an ordered collection of elements.  Lists are described by the
*List* interface. We'll discuss interfaces in more detail later but for now,
it's important to know that an interface tells us what methods we might be able
to use when something *implements* the interface.  List itself extends the
*Collection* interface, adding new methods. Some of these are:

| Method                    | Description                                                                                          |
|:--------------------------|:-----------------------------------------------------------------------------------------------------|
| void add(int index, T e)  | Insert element *e* of type *T* into the list at position *index* and shift elements to the right as necessary    |
| T get(int index)          | Returns the element of type *T* stored at position *index*                                           |
| int indexOf(Object o)     | Returns the index of the first occurrence of element *o* in the list                                 |
| int lastIndexOf(Object o) | Returns the index of the last occurrence of element *o* in the list                                  |
| T remove(int index)       | Remove and return the element at *index* in the list                                                 |
| T set(int index, T e)     | Replace the element at position *index* in the list with element *e* and return the previous element |

List itself is only an interface so we can't create instances of List. We can
however, create instances of *ArrayList* and *LinkedList*, which both implement
the list interface.

### ArrayList
The **ArrayList** class provides an implementation of the List interface based
on arrays. Because of this, access to elements is fast but updates are
relatively slow.

```java
package com.myname.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final TextView output = (TextView) findViewById((R.id.output));
        final EditText input = (EditText) findViewById(R.id.input);
        Button button = (Button) findViewById(R.id.button);

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
            }
        });

        StringBuilder builder = new StringBuilder();

        char letter = 'A';

        List<String> cities = new ArrayList<String>();
        cities.add("Columbus");
        cities.add("New York");
        cities.add("Tokyo");

        //Use Collection.contains()
        builder.append("Cleveland in list: " + cities.contains("Cleveland"));
        builder.append(System.lineSeparator());

        //Use Collection.size()
        builder.append("Number of cities: " + cities.size());
        builder.append(System.lineSeparator());

        //Use List.add() to add a city after Columbus
        cities.add(1, "Cleveland");

        //Use List.remove() to remove New York
        cities.remove(2);

        //display all cities
        for (String city: cities) {
            builder.append(city);
            builder.append(System.lineSeparator());
        }

        output.setText(builder.toString());

    }
}
```

In this example, we created a new ArrayList and used some of the methods
available to us.  Notice that a key difference between a list and an array is
that we are able to easily add new elements and increase the size of the list.

### LinkedList

The **LinkedList** provides an implementation based on linked nodes.  A *node*
is a data structure that includes a value and possibly the memory location of
another node. In a LinkedList, the first node will have a value and the
location of the next node. With this structure, it's relatively fast to add or
remove elements (the memory locations just need to be updated) but accessing
an element is slow because each node that precedes the element has to be
examined.

An example of using a LinkedList is very similar to one for an ArrayList, we
can replace

`import java.util.ArrayList;` and
`List<String> cities = new ArrayList<String>();`


with

`import java.util.LinkedList` and
`List<String> cities = new LinkedList<String>();`

When reading about the object-oriented design paradigm and interfaces, you
might encounter the following phrase: "Program to an interface, not an
implementation."  Part of what this means is that when declaring a variable or
defining a method, it's often more convenient to use the name of the most basic
class or interface that supports your needs as the data type than the class of
the instance you might be creating or passing to a method.

Consider this example that allows us to see that adding to an ArrayList is
slower than adding to a LinkedList but iterating through an ArrayList is faster.

```java
package com.myname.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    static List<Integer> addEntries(int number, List<Integer> list){
        for (int i = 0; i < number; i++)
            list.add(i);
        return list;
    }

    static long benchmarkAddList(int entries, List<Integer> testList) {
        long startTime = System.currentTimeMillis();
        addEntries(entries, testList);
        long endTime = System.currentTimeMillis();
        return endTime - startTime;
    }

    static long benchmarkIterateList(int entries, List<Integer> testList) {
        addEntries(entries, testList);
        long startTime = System.currentTimeMillis();
        long sum = 0;
        for (int entry: testList) {
            sum += entry;
        }
        long endTime = System.currentTimeMillis();
        return endTime - startTime;
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final TextView output = (TextView) findViewById((R.id.output));
        final EditText input = (EditText) findViewById(R.id.input);
        Button button = (Button) findViewById(R.id.button);

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
            }
        });

        StringBuilder builder = new StringBuilder();

        int testEntries = 1000000;  //1 million
        List<Integer> arrayList = new ArrayList<>();
        List<Integer> linkedList = new LinkedList<>();

        long arrayAddTime = benchmarkAddList(testEntries, arrayList);
        long linkedAddTime = benchmarkAddList(testEntries, linkedList);

        arrayList = new ArrayList<>();
        linkedList = new LinkedList<>();

        long arrayIterateTime = benchmarkIterateList(testEntries, arrayList);
        long linkedIterateTime = benchmarkIterateList(testEntries, linkedList);

        builder.append("Time (ms) to add to ArrayList: " + arrayAddTime);
        builder.append(System.lineSeparator());

        builder.append("Time (ms) to add to LinkedList: " + linkedAddTime);
        builder.append(System.lineSeparator());

        builder.append("Time (ms) to iterate ArrayList: " + arrayIterateTime);
        builder.append(System.lineSeparator());

        builder.append("Time (ms) to iterate LinkedList: " + linkedIterateTime);
        builder.append(System.lineSeparator());

        output.setText(builder.toString());

    }
}
```

Notice that the *addEntries(), benchmarkAddList(), and benchmarkIterateList()*
methods take parameters of the List type and not ArrayList or LinkedList. If
the methods had used ArrayList or LinkedList, we would have needed separate
methods.  Because the List interface specifies the methods we need to use, we
can use List instead of ArrayList or LinkedList.

## Sets

A set is a collection that contains no duplicate elements.  Specifically, a set
contains no pair of elements *e1* and *e2* such that *e1.equals(e2)* is true.
Like Lists, Sets extend the Collection interface and is an interface itself.

### TreeSet

A TreeSet stores elements based on a tree structure with each element linked to
others.  This results in elements being stored in a sorted order but accessing
the elements can be slow compared to non-sorted sets.

```java
package com.myname.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

import java.util.Set;
import java.util.TreeSet;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final TextView output = (TextView) findViewById((R.id.output));
        final EditText input = (EditText) findViewById(R.id.input);
        Button button = (Button) findViewById(R.id.button);

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
            }
        });

        StringBuilder builder = new StringBuilder();

        Set<String> cities = new TreeSet<>();
        cities.add("Dayton");
        cities.add("Columbus");
        cities.add("Cleveland");
        cities.add("Columbus");

        for (String city: cities) {
            builder.append(city);
            builder.append(System.lineSeparator());
        }

        output.setText(builder.toString());

    }
}
```

### HashSet

A HashSet stores elements based on a HashMap (which we'll look at soon) and
uses an elements hash code (the value returned by the hashCode() method) to
determine if an element is in the set or not.  While there is no guarantee
about order, using a HashSet is typically faster than using as TreeSet.

```java
package com.myname.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

import java.util.Set;
import java.util.HashSet;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final TextView output = (TextView) findViewById((R.id.output));
        final EditText input = (EditText) findViewById(R.id.input);
        Button button = (Button) findViewById(R.id.button);

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
            }
        });

        StringBuilder builder = new StringBuilder();

        Set<String> cities = new HashSet<>();
        cities.add("Dayton");
        cities.add("Columbus");
        cities.add("Cleveland");
        cities.add("Columbus");

        for (String city: cities) {
            builder.append(city);
            builder.append(System.lineSeparator());
        }

        output.setText(builder.toString());

    }
}
```

Compare the output from the example using a TreeSet with this example. The
elements of the TreeSet is guaranteed to be in order; the elements of a HashSet
may or may not be in order.

## Maps

A **Map** is a group of key/value pairs where each key is unique and
corresponds to at most one value.  Maps are declared using `Map<K,V>` where *K*
is the type of the keys and *V* is the type of the values.  The Map interface
includes the following methods:

| Method                              | Description                                                                                                                                        |
|:------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|
| void clear()                        | Removes all entries from the map.                                                                                                                  |
| boolean containsKey(Object key)     | Returns true when the map contains an entry for the specified *key* and false otherwise.                                                           |
| boolean containsValue(Object value) | Returns true when the map maps one or more keys to *value*.                                                                                        |
| V get(Object key)                   | Return the value to which *key* is mapped or null if the map doesn't contain the key.                                                              |
| Set<K> keySet()                     | returns a Set view of the keys contained in the map.  Because this is a view, changes made to the map are reflected in the set and vice versa.     |
| V put(K key, V value)               | Associates *value* with *key* in the map.  If the map previously contained a value for *key*, the value is replaced and the old value is returned. |
| V remove(Object key)                | Removes the entry for *key* from the map and returns the associated value if the map contained an entry for *key*.                                 |
| int size()                          | Returns the number of entries in the map.                                                                                                          |
| Collection<V> values()              | Returns a Collection view of the values contained in the map.                                                                                      |


### TreeMap

A **TreeMap** is a map implementation based on a tree structure.  As a result,
the entries are stored in sorted order based on keys.  Accessing entries is
somewhat slower than other map implementations.

```java
package com.myname.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

import java.util.Map;
import java.util.TreeMap;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final TextView output = (TextView) findViewById((R.id.output));
        final EditText input = (EditText) findViewById(R.id.input);
        Button button = (Button) findViewById(R.id.button);

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
            }
        });

        StringBuilder builder = new StringBuilder();

        Integer[] columbus_temps = {20, 40};
        Integer[] cleveland_temps = {0, 30};

        Map<String, Integer[]> temperatures = new TreeMap<>();

        temperatures.put("Columbus", columbus_temps);
        temperatures.put("Cleveland", cleveland_temps);

        for (String key: temperatures.keySet()) {
            Integer[] temps = temperatures.get(key);

            builder.append(
                "City: " + key
                + ", Low: " + temps[0]
                + ", High: " + temps[1]
            );

            builder.append(System.lineSeparator());
        }

        output.setText(builder.toString());
    }
}
```

In this example, we created a TreeMap to pair city names with an array of
temperatures.  Notice that we used *Columbus* for a key twice and the original
values for temperatures was replaced by the values the second time we used
*Columbus*.

### HashMap
A **HashMap** provides a map implementation based on on a hash table data
structure.  A simple hash table has slots for storing various values.  The
slot in which a value will be stored is determined by the hash code of the
corresponding key.  A hash code is an integer value mapped to from the key
value using a hash function.  In Java, the default hash code is determined by
the `.hashCode()` method.  A HashMap makes no guarantee about order. We can
reuse the code in the previous example by replacing `TreeMap` with `HashMap`.

## Exercise

Suppose we have read the contents of a text file into a string array.  Write
a program then determines and displays the number of times each word in the
array appears. For example, if the string array were

```java
String[] data = {"It", "was", "the", "best", "of", "times", "It", "was", "the",
        "worst", "of", "times"};
```

the output of the program would be

```
It: 2
was: 2
the: 2
best: 1
of: 2
times: 2
worst: 1
```

The order of the words might vary.
