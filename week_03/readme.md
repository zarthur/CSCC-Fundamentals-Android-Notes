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

### Returning from a Method
### Method Overloading

## String/Integer Methods

## Access Control

## Enumerations
