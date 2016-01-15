# Week 3 - Introduction to Methods and Enumerations

## Corresponding Text
*Learn Java for Android Development*, pp. 107-124, 273-280

## Methods
As our programs become more complex, it becomes import to divide and organize
our code in a meaningful way.  For example, if we perform the same set of
steps by executing similar sets of statements, we might want to be able to
group those steps together and write them only once rather than writing them
over and over again.  Often we can do this sort of grouping using looping
statements. Another way to group parts of our code is by behavior.
**Behaviors** refer to some sequence of code that performs a certain task.  
In addition to this week's discussion, we'll talk more about behaviors when we
discuss classes and objects.  

Java represents behavior using methods.  **Methods** are named blocks of code.
In your reading, you will see references to class methods and instance methods.
For now, we'll only be looking at class methods but we'll talk about both again
later. For the remainder of this week's discussion, method will technically
refer to class methods.  

Before we start working with methods, let's look at the following code and
think of how we can improve it:

```java
package com.myname.week_03;

public class Main {
    public static void main(String[] args) {
        //convert Celsius to Fahrenheit using F = 9/5 * C + 32

        double celsiusLowTemperature = 0.0;
        double celsiusHighTemperature = 9.3;
        double fahrenheitLowTemperature = 9/5 * celsiusLowTemperature + 32;
        double fahrenheitHighTemperature = 9/5 * celsiusHighTemperature + 32;
        System.out.println("The low will be " + fahrenheitLowTemperature
                + " and the high will be " + fahrenheitHighTemperature + ".");

        celsiusLowTemperature = -2.4;
        celsiusHighTemperature = 8.3;
        fahrenheitLowTemperature = 9/5 * celsiusLowTemperature + 32;
        fahrenheitHighTemperature = 9/5 * celsiusHighTemperature + 32;
        System.out.println("The low will be " + fahrenheitLowTemperature
                + " and the high will be " + fahrenheitHighTemperature + ".");

        celsiusLowTemperature = 7.7;
        celsiusHighTemperature = 16.8;
        fahrenheitLowTemperature = 9/5 * celsiusLowTemperature + 32;
        fahrenheitHighTemperature = 9/5 * celsiusHighTemperature + 32;
        System.out.println("The low will be " + fahrenheitLowTemperature
                + " and the high will be " + fahrenheitHighTemperature + ".");

        celsiusLowTemperature = 14.1;
        celsiusHighTemperature = 26.3;
        fahrenheitLowTemperature = 9/5 * celsiusLowTemperature + 32;
        fahrenheitHighTemperature = 9/5 * celsiusHighTemperature + 32;
        System.out.println("The low will be " + fahrenheitLowTemperature
                + " and the high will be " + fahrenheitHighTemperature + ".");
    }
}

```

Here, we're converting some low and high temperatures for four days from
Celsius to Fahrenheit.  One way to improve this code is by using what we know
about loop statements.  For example, we can use a for loop.

```java
package com.myname.week_03;

public class Main {
    public static void main(String[] args) {
        //convert Celsius to Fahrenheit using F = 9/5 * C + 32

        double[] celsiusLowTemperatures = {0.0, -2.4, 7.7, 14.1};
        double[] celsiusHighTemperatures = {9.3, 8.3, 16.8, 26.3};
        double fahrenheitLowTemperature;
        double fahrenheitHighTemperature;

        for (int i = 0; i < celsiusLowTemperatures.length; i++) {
            fahrenheitLowTemperature = 9 / 5 * celsiusLowTemperatures[i] + 32;
            fahrenheitHighTemperature = 9 / 5 * celsiusHighTemperatures[i] + 32;
            System.out.println("The low will be " + fahrenheitLowTemperature
                    + " and the high will be " + fahrenheitHighTemperature
                    + ".");
        }
    }
}
```

Using a for loop certainly made the code shorter and easier to read.  We can
still improve this code though.  Notice that we've basically written the same
calculation for converting Celsius to Fahrenheit.  Having the code for the
actual calculation written once would be an improvement.  One reason it would
be an improvement is that it makes the code easier to maintain.  Suppose we
made a mistake or what to change the calculation.  If the calculation was
written a single time, we'd only have to make the change in one place rather
than in multiple places.  But how can we do this?  We can use methods.

The syntax for declaring a method is:

```
static return_type name(parameter_list) {
    statements_to_execute
}
```

A method (technically, a class method) starts with the reserved word *static*.
Next, the data type of the method's return value is given by *return_type*;
the return type is a primitive type, a reference type, or `void` when the
method does not return a value.  After the return type, the methods name is
specified.  Following the name and enclosed in parentheses is a list of
parameters, *parameter_list* that lists the kinds of of data items that are
passed to the method.  We'll talk about each of the these parts in more detail
soon.  A method's name and the number, types, and order of its parameters are
known as the method's **signature**.  The reserved word *static*, the return
type, and the method's signature are known as the **method header**; the set
of statements within a method, *statements_to_execute*, are known as the
**method body**.  

Let's look at how we can move the conversion of Celsius to Fahrenheit to a
method.  We're going to change the output of our program for now.

```java
package com.myname.week_03;

public class Main {
    static void celsiusToFahrenheit(double celsiusValue) {
        double fahrenheitValue = 9/5 * celsiusValue + 32;
        System.out.println(celsiusValue + " degrees Celsius is equal to "
                + fahrenheitValue + " degrees Fahrenheit.");
    }

    public static void main(String[] args) {
        //convert Celsius to Fahrenheit using F = 9/5 * C + 32

        double[] celsiusLowTemperatures = {0.0, -2.4, 7.7, 14.1};
        double[] celsiusHighTemperatures = {9.3, 8.3, 16.8, 26.3};

        for (int i = 0; i < celsiusLowTemperatures.length; i++) {
            celsiusToFahrenheit(celsiusLowTemperatures[i]);
            celsiusToFahrenheit(celsiusHighTemperatures[i]);
        }
    }
}
```

### Passing Arguments to Methods
### Returning from a Method
### Method Overloading

## Access Control

## Enumerations
