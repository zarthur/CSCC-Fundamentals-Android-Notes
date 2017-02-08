# Week 4 - Collection Types

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
attribute when we used the `.length` property of an array.  The code that
represents behavior and acts on attributes are represented by methods. So far,
we've primarily worked with class methods but objects can have another type of
method know as instance methods. **Instance methods** are collections of code
that can alter the attributes of an object.

In addition to classes, Java also has interfaces. An **interface** can be used
to specify what attributes and methods an object will have.  We can't create
objects directly from interfaces but we can use classes to *implement* the
interface. If a class implements an interface, we know that we will be able to
use the attributes and fields specified by the interface with the class and
objects created from it.  

We'll discuss classes and interfaces in more detail in later lectures.

As an example of some of these concepts, let's look at the following code:

```java
package com.myname.week_04;

public class Main {
    public static void main(String[] args) {
        String testString = new String("Hello");
        testString = testString.concat(", World!");
        System.out.println("After concat method: " + testString);

        boolean hasHello = testString.contains("Hello");
        System.out.println("'Hello' in string: " + hasHello);

        System.out.println("Length: " + testString.length());
    }
}
```

The output is:

```
After concat method: Hello, World!
'Hello' in string: true
Length: 13
```

### Collection Interface
One of the things we'll be looking at is alternate ways of storing collections
of elements other than arrays.  One of these ways is by using lists.  Before we
look at lists, we should be aware of the *Collection* interface.  The
Collection interface specifies certain methods. Something implementing the
Collection interface will have the following methods
available.  There are additional methods but we'll focus on a subset.  

| Method Signature           | Description                                                                                                                                                 |
|:---------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| boolean add(E e)           | Add an element, *e* of type *E* to the collection; returns true if the element is added and false otherwise                                                 |
| void clear()               | Remove all elements from the collection                                                                                                                     |
| boolean contains(Object o) | Returns true if the element *o* is present in the collection and false otherwise                                                                            |
| boolean isEmpty()          | Returns true when the collection has no elements and false otherwise                                                                                        |
| boolean remove(Object o)   | Removes the element identified as *o* from the collection; returns true if the element is removed and false otherwise                                       |
| int size()                 | Returns the number of elements in the collection or java.lang.Integer.MAX_VALUE if there are more elements than the the largest value represented by an int |
| Object[] toArray()         | Returns an array containing all the elements stored in the collection                                                                                       |

Another important thing to note is that you cannot store primitive-type-based
values in objects implementing Collection.  So, what if we want to create a
collections of values with primitive types?  We have to use primitive type
wrappers.

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
package com.myname.week_04;

public class Main {
    public static void main(String[] args) {
        int number = 10;
        char letter = 'A';

        Integer wrappedInt = new Integer(number);
        Character wrappedChar = new Character(letter);

    }
}
```

This creates two objects, *wrappedInt* and *wrappedChar*, that represent an
integer value and character value, respectively, but use reference types rather
than primitive types.  

Another feature of primitive type wrappers is that they have various convenient
attributes and methods.  For example:

```java
package com.myname.week_04;

public class Main {
    public static void main(String[] args) {
        char letter = 'A';
        System.out.println("isAlphabetic: " + Character.isAlphabetic(letter));
        System.out.println("isLowerCase: " + Character.isLowerCase(letter));

        System.out.println("MAX_VALUE: " + Integer.MAX_VALUE);
    }
}
```


## Introduction to Packages
A **package** is a unique namespace that can contain classes and sub-packages. 
Packages allow us to organize our code in meaningful ways similar to how
folders and subfolders let us organize files in general on a computer.  In the
code we've written so far, we've started each Java class file with a `package`
statement and sorted our project into modules and packages.  

We can gain access to classes from other packages by writing out the full name
of the package and the class such as `java.util.Scanner` in this example:

```java
package com.myname.week_04;

public class Main {

    public static String getUserInput(){
        java.util.Scanner scanner = new java.util.Scanner(System.in);
        return scanner.nextLine();
    }

    public static void main(String[] args) {
        String input = getUserInput();
        System.out.println("The input was: " + input);
    }
}
```

Or we could use an `import` statement to avoid having to type `java.util`
repeatedly.

```java
package com.myname.week_04;

import java.util.Scanner;

public class Main {

    public static String getUserInput(){
        Scanner scanner = new Scanner(System.in);
        return scanner.nextLine();
    }

    public static void main(String[] args) {
        String input = getUserInput();
        System.out.println("The input was: " + input);
    }
}
```

As we use lists, sets, and maps, we'll make use of import statements to avoid
repetition.

## Lists
A **list** is an ordered collection of elements.  Lists are described by the
*List* interface. We'll discuss interfaces in more detail later but for now,
it's important to know that an interface tells us what methods we might be able
to use when something *implements* the interface.  List itself extends the
*Collection* interface, adding new methods. Some of these are:

| Method                    | Description                                                                                          |
|:--------------------------|:-----------------------------------------------------------------------------------------------------|
| void add(int index, E e)  | Insert element *e* into the list at position *index* and shift elements to the right as necessary    |
| E get(int index)          | Returns the element of type *E* stored at position *index*                                           |
| int indexOf(Object o)     | Returns the index of the first occurrence of element *o* in the list                                 |
| int lastIndexOf(Object o) | Returns the index of the last occurrence of element *o* in the list                                  |
| E remove(int index)       | Remove and return the element at *index* in the list                                                 |
| E set(int index, E e)     | Replace the element at position *index* in the list with element *e* and return the previous element |

List itself is only an interface so we can't create instances of List. We can
however, create instances of *ArrayList* and *LinkedList*, which both implement
the list interface.

### ArrayList
The **ArrayList** class provides an implementation of the List interface based
on arrays. Because of this, access to elements is fast but updates are
relatively slow.

```java
package com.myname.week_04;

import java.util.ArrayList;
import java.util.List;

public class Main {

    public static void main(String[] args) {

        List<String> cities = new ArrayList<String>();
        cities.add("Columbus");
        cities.add("New York");
        cities.add("Tokyo");

        //Use Collection.contains()
        System.out.println("Cleveland in list: " + cities.contains("Cleveland"));

        //Use Collection.size()
        System.out.println("Number of cities: " + cities.size());

        //Use List.add() to add a city after Columbus
        cities.add(1, "Cleveland");

        //Use List.remove() to remove New York
        cities.remove(2);

        //display all cities
        for (String city: cities) {
            System.out.println(city);
        }


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

An example of using a LinkedList is very similar to one for an ArrayList:

```java
package com.myname.week_04;

import java.util.LinkedList;
import java.util.List;

public class Main {

    public static void main(String[] args) {

        List<String> cities = new LinkedList<String>();
        cities.add("Columbus");
        cities.add("New York");
        cities.add("Tokyo");

        //Use Collection.contains()
        System.out.println("Cleveland in list: " + cities.contains("Cleveland"));

        //Use Collection.size()
        System.out.println("Number of cities: " + cities.size());

        //Use List.add() to add a city after Columbus
        cities.add(1, "Cleveland");

        //Use List.remove() to remove New York
        cities.remove(2);

        //display all cities
        for (String city: cities) {
            System.out.println(city);
        }


    }
}
```

When reading about the object-oriented design paradigm and interfaces, you
might encounter the following phrase: "Program to an interface, not an
implementation."  Part of what this means is that when declaring a variable or 
defining a method, it's often more convenient to use the name of the most basic 
class or interface that supports your needs as the data type than the class of 
the instance you might be creating or passing to a method.

Consider this example that allows us to see that adding to an ArrayList is
slower than adding to a LinkedList but iterating through an ArrayList is faster.

```java
package com.myname.week_02;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class Main {
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


    static void benchmark() {
        int testEntries = 1000000;  //1 million
        List<Integer> arrayList = new ArrayList<>();
        List<Integer> linkedList = new LinkedList<>();

        long arrayAddTime = benchmarkAddList(testEntries, arrayList);
        long linkedAddTime = benchmarkAddList(testEntries, linkedList);

        arrayList = new ArrayList<>();
        linkedList = new LinkedList<>();

        long arrayIterateTime = benchmarkIterateList(testEntries, arrayList);
        long linkedIterateTime = benchmarkIterateList(testEntries, linkedList);

        System.out.println("Time (ms) to add to ArrayList: " + arrayAddTime);
        System.out.println("Time (ms) to add to LinkedList: " + linkedAddTime);
        System.out.println("Time (ms) to iterate ArrayList: " + arrayIterateTime);
        System.out.println("Time (ms) to iterate LinkedList: " + linkedIterateTime);

    }
    public static void main(String[] args) {
        benchmark();
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
package com.myname.week_04;

import java.util.Set;
import java.util.TreeSet;

public class Main {

    public static void main(String[] args) {

        Set<String> cities = new TreeSet<>();
        cities.add("Dayton");
        cities.add("Columbus");
        cities.add("Cleveland");
        cities.add("Columbus");

        for (String city: cities) {
            System.out.println(city);
        }

    }
}
```

### HashSet
A HashSet stores elements based on a HashMap (which we'll look at soon) and
uses an elements hash code (the value returned by the hashCode() method) to
determine if an element is in the set or not.  While there is no guarantee
about order, using a HashSet is typically faster than using as TreeSet.

```java
package com.myname.week_02;

import java.util.Set;
import java.util.HashSet;

public class Main {

    public static void main(String[] args) {

        Set<String> cities = new HashSet<>();
        cities.add("Dayton");
        cities.add("Columbus");
        cities.add("Cleveland");
        cities.add("Columbus");

        for (String city: cities) {
            System.out.println(city);
        }

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
package com.myname.week_04;

import java.util.Map;
import java.util.TreeMap;

public class Main {
    static void displayTemperatures(Map<String, Integer[]> tempMap) {
        for (String key: tempMap.keySet()) {
            Integer[] temps = tempMap.get(key);
            System.out.println("City: " + key
                    + ", Low: " + temps[0]
                    + ", High: " + temps[1]);
        }
    }

    static Map<String, Integer[]> assignTemperatures(String[] cities,
                                                     int[] lowTemps,
                                                     int[] highTemps) {
        Map<String, Integer[]> temperatures = new TreeMap<>();
        for (int i = 0; i < cities.length; i++) {
            Integer[] tempArray = {lowTemps[i], highTemps[i]};
            temperatures.put(cities[i], tempArray);
        }
        return temperatures;
    }

    public static void main(String[] args) {
        String[] cityNames = {"Columbus", "New York", "Houston", "Columbus"};
        int[] lowTemps = {-10, -5, 50, 0};
        int[] highTemps = {20, 40, 65, 15};

        Map<String, Integer[]> temperatures = assignTemperatures(cityNames,
                lowTemps, highTemps);

        displayTemperatures(temperatures);

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
the `.hashCode()` method.  A HashMap makes no guarantee about order. Let's look
at a modified version of the previous example. In general, a HashMap performs
better than a TreeMap with regard to accessing elements.

```java
package com.myname.week_04;

import java.util.HashMap;
import java.util.Map;

public class Main {
    static void displayHashCodes(Map<String, Integer[]> tempMap) {
        for (String key: tempMap.keySet()) {
            System.out.println("Key: " + key + ", Hash: " + key.hashCode());
        }
    }


    static void displayTemperatures(Map<String, Integer[]> tempMap) {
        for (String key: tempMap.keySet()) {
            Integer[] temps = tempMap.get(key);
            System.out.println("City: " + key
                    + ", Low: " + temps[0]
                    + ", High: " + temps[1]);
        }
    }

    static Map<String, Integer[]> assignTemperatures(String[] cities,
                                                     int[] lowTemps,
                                                     int[] highTemps) {
        Map<String, Integer[]> temperatures = new HashMap<>();
        for (int i = 0; i < cities.length; i++) {
            Integer[] tempArray = {lowTemps[i], highTemps[i]};
            temperatures.put(cities[i], tempArray);
        }
        return temperatures;
    }

    public static void main(String[] args) {
        String[] cityNames = {"Columbus", "New York", "Houston", "Columbus"};
        int[] lowTemps = {-10, -5, 50, 0};
        int[] highTemps = {20, 40, 65, 15};

        Map<String, Integer[]> temperatures = assignTemperatures(cityNames,
                lowTemps, highTemps);

        displayTemperatures(temperatures);
        displayHashCodes(temperatures);

    }
}
```

This example also prints the hash code of each key.  Besides this change,
notice that we only had to make a change in one place to use a HashMap instead
of a TreeMap.  This is the convenience of programming to interfaces.

## Exercise
Suppose we have read the contents of a text file into a string array.  Write 
a program that determines and displays the number of times each word in the 
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

