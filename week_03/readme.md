# Week 3 - Introduction to Methods and Enumerations

## Corresponding Text
*Learn Java for Android Development*, pp. 107-124, 275-276

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

The syntax for declaring a method (specifically, a class method) is:

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

We've created a method with the header
`static void celsiusToFahrenheit(double celsiusValue)`, where `void` indicates
that the method will not return a value, `celsiusToFahrenheit` is the method's
name, and `double celsiusValue` is the method's only parameter. The method's
name and parameters, `celsiusToFahrenheit(double celsiusValue)`, are the
method's signature.  The body of the method consists of the calculation to
convert a Celsius temperature to Fahrenheit and a statement to display both the
Celsius and Fahrenheit values.  In the *main* method, we call, or make use of
the *celsiusToFahrenheit* method twice per iteration of the for loop, once to
convert a low temperature and again to convert a high temperature.  Notice the
syntax used to call or invoke the method: the methods name followed by
parentheses containing any values that take the place of the method's
parameters.  Here, instead of doing the temperature conversion in multiple
places, we've been able to write it once; if we ever need to change, it we only
have to change it in one place.  We've also captured the temperature-conversion
behavior in the method.

It's important to note that the program begins execution in the *main* method.

### Passing Arguments to Methods
In the previous example, our *celsiusToFahrenheit* method took one argument,
*celsiusValue*.  A method can take zero or more arguments which are specified
within parentheses in the method header.  In the method header, a parameters
type and its name within the method are specified.  The scope of the parameter
names is limited to the method body, that is, they generally cannot be used
outside the method.  In the previous example, the scope of *celsiusValue* was
limited to the *celsiusToFahrenheit* method so an attempt to use it outside the
method would have resulted in an error.  Similarly, variables declared within
a method body have a scoped limited to the method.  The variable
*fahrenheitValue*, is only accessible from within the method.

Arguments are passed to functions in a style known as pass-by-value.  
**Pass-by-value** passes the value of a variable to the method.  In the case
of primitive types, the value associated with the  variable is passed to the
method. In the case of reference variables, the value associated with the
variable is a memory location, so that location is passed to the variable.
Let's look at an example of why this distinction is important.

```java
package com.myname.week_03;

public class Main {
    static void methodExample(int integerValue, int[] integerArray) {
        integerValue += 1000;
        integerArray[0] = -integerArray[0];
    }

    public static void main(String[] args) {
        int anInteger = 10;
        int anIntegerArray[] = {10, 20};

        System.out.println("Initial integer: " + anInteger);
        System.out.println("The first element of an integer array: " + anIntegerArray[0]);

        methodExample(anInteger, anIntegerArray);

        System.out.println("Integer after method call: " + anInteger);
        System.out.println("First element after method call: " + anIntegerArray[0]);
    }
}
```
The output of this program is:

```
Initial integer: 10
The first element of an integer array: 10
Integer after method call: 10
First element after method call: -10
```

If we follow the program's execution starting in *main*, we see that we start
with two variables: *anInteger*, an integer, and *anIntegerArray*, an array of
integers. Recall that the *int* type is a primitive type so it's value is the
associated integer assigned to it. Arrays are reference types so the value
stored in the variable is actually a location in memory where the array's
values are stored. When we call the *methodExample* method, the values of the
parameters, *anInteger* and *anIntegerArray*, are passed to the
method - the values are copied and made available to the method. Because
*anInteger* is a primitive data type, the copied value *10* is associated with
the variable *integerValue* in the method body. A change to *integerValue*
doesn't affect *anInteger*. The variable *anIntegerArray*, is different.  It's
value is a memory location of the integer values that constitute the array.  
Because *integerArray* and *anIntegerArray* both have values that correspond to
the same memory location, changes to *anIntegerArray* within the method will
affect the *integerArray* variable outside the method.  

While strings are reference types, they are immutable in Java. So, they don't
exhibit the same behavior as arrays.

### Returning from a Method
In a previous example, our method signature header was
`static void celsiusToFahrenheit`.  Recall that `void` specified the return
type of the method.  In addition to being able to pass values into a method, we
can also have the method produce a value to the method's caller. This is done
using the `return` statement.  In addition to using the return statement, we
must also specify the appropriate return type when defining the method.  Let's
look at an example that includes a temperature conversion method that returns
the calculated value.

```Java
package com.myname.week_03;

public class Main {
    static double celsiusToFahrenheit(double celsiusValue) {
        double fahrenheitValue = 9/5 * celsiusValue + 32;
        return fahrenheitValue;
    }

    public static void main(String[] args) {
        double[] celsiusLowTemperatures = {0.0, -2.4, 7.7, 14.1};
        double[] celsiusHighTemperatures = {9.3, 8.3, 16.8, 26.3};


        for (int i = 0; i < celsiusLowTemperatures.length; i++) {
            double fahrenheitLowTemperature;
            double fahrenheitHighTemperature;
            fahrenheitLowTemperature = celsiusToFahrenheit(celsiusLowTemperatures[i]);
            fahrenheitHighTemperature = celsiusToFahrenheit(celsiusHighTemperatures[i]);
            System.out.println("The low will be " + fahrenheitLowTemperature
                    + " and the high will be " + fahrenheitHighTemperature + ".");
        }
    }
}
```

In this example, the method not only calculates the Fahrenheit value but also
returns that value.  This allows us to use the value outside the method.  
Notice that the method header now has the word `double` where `void` appered in
previous methods.  This indicates that the method will return a value and the
data type of that value will be `double`.  The `return` statement can also be
used to control the flow of execution within a method.

```Java
package com.myname.week_03;

public class Main {
    static String willSnow(double temperature, double probabilityPrecipitation) {
        if (temperature <= 32 && probabilityPrecipitation > 0.5) {
            return "likely";
        }
        return "unlikely";
    }

    public static void main(String[] args) {
        double[] temperatures = {28, 31, 46, 37};
        double[] probabilities = {.3, .9, .4, .7};

        for (int i = 0; i < temperatures.length; i++) {
            String snowy = willSnow(temperatures[i], probabilities[i]);
            System.out.println("When the temperature is " + temperatures[i]
                    + " and the probability of precipitation is "
                    + probabilities[i] + ", it is " + snowy + " to snow.");

        }
    }
}
```

The return statement can also be used for flow control when the method's return
type is `void`.  This is done by simply using `return;` with no value.

### Method Overloading
Java allows us to define methods with the same names but with different
parameter lists.  This allows us to use the same name for a collection of
methods that exhibit the same behavior but use different types of data.  

```java
package com.myname.week_03;

public class Main {
    static int add(int a, int b) {
        System.out.println("add(int, int) called");
        return a + b;
    }

    static int add(int a, int b, int c) {
        System.out.println("add(int, int, int) called");
        return a + b + c;
    }

    static double add(double a, double b) {
        System.out.println("add(double, double) called");
        return a + b;
    }

    public static void main(String[] args) {
        System.out.println("Adding two integers: 2, 3");
        int firstSum = add(2, 3);
        System.out.println(firstSum);

        System.out.println("Adding three integers: 2, 3, 4");
        int secondSum = add(2, 3, 4);
        System.out.println(secondSum);

        System.out.println("Adding two doubles: 2.0, 4.5");
        double thirdSum = add(2.0, 4.5);
        System.out.println(thirdSum);

    }
}
```

The ouput is:

```
Adding two integers: 2, 3
add(int, int) called
5
Adding three integers: 2, 3, 4
add(int, int, int) called
9
Adding two doubles: 2.0, 4.5
add(double, double) called
6.5
```

In this example, we created three methods that did something similar: added
numeric values.  Each was different in types or number of parameters they took.
Recall that the method signature is given by a method's name and the number,
type, and order of the method's parameters.  When defining methods, the
method's signature must be unique.  There are additional factors that affect
this but we will discuss them later.  For example, the following is not
valid:

```java
package com.myname.week_03;

public class Main {
    static int add(int a, int b) {
        return a + b;
    }

    static long add(int a, int b) {
        return a + b;
    }

    public static void main(String[] args) {
        int firstSum = add(2, 3);
        long secondSum = add(2, 3);
    }
}
```

Even though they have different reutrn types, the two `add` methods above have
the same method signatures.

## The Java Standard Library
We've used System.out.println() to display output in the console.  `println()`
is an example of a method available for our use from the standard library.  The
standard library is a collection of features including methods that are
included with Java for convenience.  As we continue to explore Java, we'll
make extensive use of the standard library.

Another method that might be of interest to us is one that allows us to convert
strings to integers.  

```Java
package com.myname.week_03;

public class Main {
       public static void main(String[] args) {
            String userInput = "1234";
            int intValue = Integer.parseInt(userInput);
            System.out.println(intValue + 4321);

    }
}
```

The method `Integer.parseInt()` takes a string parameter and returns an int.  
Be careful when using methods that converts from one type to another as errors
can occur.  We will talk about handling these errors later.

## Enumerations
Sometimes it's convenient to specify a set of related values as possible
values. An **enmeration** is a data type consisting of a set of named values of
the type.  The syntax for creating an enumeration in Java is:

```java
enum name { value1, value2, ..., valueN};
```

where *name* is the name of the enumeration and each of *value1* through
*valueN* are the possible values associated with the enumeration.  

```java
package com.myname.week_03;

public class Main {
    enum DIRECTION { NORTH, WEST, EAST, SOUTH};

    static void describeWind(DIRECTION windDirection) {
        switch (windDirection) {
            case NORTH:
                System.out.println("The wind is blowing from the north.");
                break;
            case WEST:
                System.out.println("The wind is blowing from the west.");
                break;
            case EAST:
                System.out.println("The wind is blowing from the east.");
                break;
            case SOUTH:
                System.out.println("The wind is blowing from the south.");
                break;
        }
    }
    public static void main(String[] args) {
        DIRECTION measuredWindDirection = DIRECTION.EAST;
        describeWind(measuredWindDirection);
    }
}
```

In this example, we define an enumration of possible wind directions: north,
west, east, and south.  We also have a method that displays information about
the wind depending on the arguement's value.  We use a switch statement that
works with the enumeration's possible values.  While we could have acheived the
same effect using strings or integers with each direction corresponding to a
direction, the enumeration makes our code more readable and maintainable.  
Because we've used an enumeration, we know precisely what values are possible
for variables like *measuredWindDirection*.

## Exercise
