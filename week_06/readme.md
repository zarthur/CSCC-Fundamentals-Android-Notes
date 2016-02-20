# Week 6- Exception Handling

## Corresponding Text
*Learn Java for Android Development*, pp. 217-232

##Exceptions
An **exception** is a interruption in the normal behavior of a program.  
Suppose your program tries to load data from a resource on the Internet. The
normal behavior is that the data will be loaded and the program will continue.
If the resource is unavailable or unreachable, the data can't be loaded and
the program might stop.

In this example, the exception cannot be prevented but a workaround can be
written into the code - perhaps the program could make another attempt
to load the data or use some default set of data.  Generally, unpreventable
exceptions where workarounds are possible should not be ignored.

You might have encountered this type of exception in code we've written before.
For example, when we ask the user for input, store the input as a string,
and try to convert the string to an integer, we will encounter an exception
when the user's input doesn't correspond to an integer.

```java
package com.myname.week_06;

import java.util.Scanner;

class UserInput {
    Scanner scanner = new Scanner(System.in);

    public int promptInt(String message) {
        System.out.println(message);
        String userInput = scanner.nextLine();
        int userInt = Integer.parseInt(userInput);
        return userInt;
    }
}


public class Main {
    public static void main(String[] args) {
        UserInput input = new UserInput();
        int aNumber = input.promptInt("Enter an integer.");
        System.out.println("Twice your number is " + aNumber * 2);
    }
}
```

In this example, if the user enters an integer, everything works as expected.
If the user enters something other than an integer, the program stops.  We
can write code to handle the possibility of the user entering a non-integral
response.

Another example of an exception occurring is due to poorly written code.  For
example, suppose we try to access an elements beyond the length of an array.
This will cause the program to end.  While we could introduce a workaround like
the previous example, we should really fix the code.

A third type of exception is one that is not preventable and for which there
is no workaround.  An example of this is running out of memory.  These types
of exceptions are known as **errors** and are so serious that it's impossible
or inadvisable to workaround the issue and thus the application must terminate.

In this lecture, we'll explore how exceptions work in Java and how we can
handle them in our code.

### Throwable Class Hierarchy
When an exception or error occurs in Java, an object is created with details
of the context in which the exception or error occurred.  The new object is
then *thrown*. The Java Virtual Machine (JVM) in which all our Java code runs
then searches for a *handler*, code that can deal with the exception. If no
handler can be found, the program is terminated.  Our programs often include
a collection of classes and methods where one method will call another method.
When one method calls another and that method calls yet another, the JVM keeps
track of the order using the *stack*.  When an exception or error occurs,
information about which methods were called and their order is often available
as a *stack trace*.

Java provides two classes serve as the base classes of exceptions and errors.
The base class is `Throwable` and has several methods that are useful in
diagnosing problems when exceptions or errors occur. When a The classes for
exceptions and errors are `Exception` and `Error`.  These subclasses implement
all the methods the `Throwable` class does. We'll discuss base classes,
subclasses, and inheritance later in the course.

For now, let's look at the method's available to instances of the `Throwable`
class.

|Method | Description |
|--|--|
|Throwable() | Constructor with null detail message and cause.|
|Throwable(String message) | Constructor with specified message and null cause. |
|Throwable(Throwable cause) | Constructor with null message and specified cause. |
|Throwable(String message, Throwable cause) | Constructor with specified message and cause |
|Throwable getCause() | Return the cause of the throwable. |
|String getMessage() | Return the message of the throwable. |
|StackTraceElement[] getStackTrace() | Return stack trace information. |

In our example of user input, an exception occurs when we call
`Integer.parseInt()`.  The exception is created and thrown from within the
code associated with the parseInt method but because we haven't specified a
way to handle it it, the program terminates.  IntelliJ displays the statck
trace.

```
Exception in thread "main" java.lang.NumberFormatException: For input string: "2a"
	at java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
	at java.lang.Integer.parseInt(Integer.java:580)
	at java.lang.Integer.parseInt(Integer.java:615)
	at com.myname.week_06.UserInput.promptInt(Main.java:11)
	at com.myname.week_06.Main.main(Main.java:20)
```

The stack trace is displayed such that the most recent method is displayed
near the top.  From the stack trace, we can see that the exception was created
in the `NumberFormatException` class and occurred in the `Integer.parseInt()`
method.  What we're really interested in, though, is where, in our code, was
the statement that caused the exception.  If we continue examining the stack
traces, we see that the JVM was executing our code at in the
`UserInput.promptInt()` method; specifically on line 11.  The stack trace
also tells us that the `UserInput.promptInt()` method was called by
the `Main.main()` method on line 20 in our code.

This information will help us deal with the exception.

### Checked vs Runtime Exceptions

### Custom Exception Classes

## Throwing Exceptions

## Handling Exceptions

## Cleanup

## Exercise
