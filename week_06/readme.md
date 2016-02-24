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
In Java, exceptions can belong to one of two categories: checked and unchecked.
A **checked exception** is an exception from which it is possible for the
program to recover and for which the programmer must provide a workaround.
The compiler checks all code and ensures that checked exceptions are handled.
Exception and it's subclasses except RuntimeException and it's subclasses
represent checked exceptions.

An **unchecked** or **runtime exception** is an exception that doesn't need
to be handled.  Runtime exceptions often represent a coding mistake.  
Rather than writing code to handle these exceptions caused by mistakes, one
should instead fix the coding mistake.  This was the design philosophy behind
the decision to implement both checked and unchecked exceptions in Java.  
Often, however, you will see code for handling unchecked exceptions such
as NumberFormatException which we encountered earlier.

## Throwing Exceptions
When exceptions occur in the code we write, one option we have for dealing with
it is to throw the exception.  When an exception occurs in our code, we can
have the method in which the exception occurs, throw the exception to the code
that called the method.  This means that the code that makes use of our method
must now deal with the exception.  For unchecked exceptions, we don't have
to do anything special.  Notice in the stack trace from the previous example,
an exception occurred in the UserInput.promptInt() method but because it wasn't
dealt with there, it became an issue for the code that called
UserInput.promptInt(), specifically Main.main().  If an exception occurs in
a program's main method and isn't handled, the program will terminate. This is
the behavior we saw.  

If a checked exception can occur in a method and the method doesn't deal with
the exception by handling it, it must throw the method to the calling code.  In
Java, all checked exceptions that can be thrown by a method but be indicated
as part of the method's header.  

If, in our code, we want to create and throw an exception, we can use the
*throw* keyword with an instance of an exception class.  Let's looks at an
example that illustrates both creating a new exception and throwing it.

```java
package com.myname.week_06;

class Arithmetic {
    public static double average(double values []) {
        double average = 0;
        for (double value: values) {
            average += value;
        }
        return average / values.length;
    }
}

public class Main {
    public static void main(String[] args) {
      double[] someValues = {1, 2, 3};
      double averageValue = Arithmetic.average(someValues);
      System.out.println(averageValue);
    }
}
```

This code returns the average of an array of values. What happens when the
array of values is empty? As the code is written, the method will return `NaN`,
which is a special floating-point value indicating that the result is not a
number.  The method returns `NaN` because zero divided by zero is undefined.
What if we wanted to avoid calculating the average of empty arrays?  One way
we could do that is to throw a checked exception and force whatever code
that called Arithmetic.average() to deal with it.

```java
package com.myname.week_06;

class Arithmetic {
    public static double average(double values []) throws Exception {
        if (values.length == 0) {
            throw new Exception("Cannot divide by zero.");
        }
        double average = 0;
        for (double value: values) {
            average += value;
        }
        return average / values.length;
    }
}

public class Main {
    public static void main(String[] args) {
      double[] someValues = {};
      double averageValue = Arithmetic.average(someValues);
      System.out.println(averageValue);
    }
}
```

In this example, we've updated the Arithmetic.average() to check if the length
of the array of values is zero.  If it is, we create a new instance of the
Exception class.  Since Exception is a subclass of Throwable, it has similar
constructors including one that takes a string parameter as a message.  Because
the Exception class represents a checked exception and because we don't address
the new exception in Arithmetic.average(), we have to indicate that the method
might throw it by updating the method's header.

This code will not compile and run.  The reason it won't compile is because
there is the possibility of encountering a checked exception in the Main.main()
method and we haven't handled it there or indicated that the method could throw
it.  Generally, we don't want the program's main method to throw exceptions, so
we should handle it.  

## Handling Exceptions
To deal with exceptions, both checked and unchecked, when they occur, we can
use try-catch statements.  A **try-catch statement** is code that executes
other code that might throw an exception and specifies what to do if the
exception occurs.  The syntax of a try-catch statement is:

```java
try {
  //code that can throw an exception
}
catch (ExceptionType e) {
  //code to address the exception
}
```

where *ExceptionType* is the specific type of exception that occurs and *e* is
the exception instance itself, which can be useful for debugging.

Let's add some exception handling to our previous example so that the program
will run.

```java
package com.myname.week_06;

class Arithmetic {
    public static double average(double values []) throws Exception {
        if (values.length == 0) {
            throw new Exception("Cannot divide by zero.");
        }
        double average = 0;
        for (double value: values) {
            average += value;
        }
        return average / values.length;
    }
}

public class Main {
    public static void main(String[] args) {
        double[] someValues = {};
        double averageValue = 0;
        try {
            averageValue = Arithmetic.average(someValues);
        }
        catch (Exception e) {
            System.out.println("Encountered an exception: " + e.getLocalizedMessage());
        }
        System.out.println(averageValue);
    }
}
```

We've added a try-catch block to Main.main().  Inside the try block of code,
we included the statement that could generate an exception.  Inside the
catch block, we've printed some text that includes the message associated
with the exception itself.  

Generally, you should put as few statements as necessary in a try block and
use the most specific type of exception in the catch block.  This makes your
code much easier to read and understand.  

Let's return to our first example:

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

Recall that UserInput.promptInt() will throw an unchecked NumberFormatException
when the user specifies a value that cannot be parsed as an int.  Let's make
use of the fact that Integer.parseInt() throws the exception to repeatedly
prompt the user until they enter a valid value.  

```java
package com.myname.week_06;

import java.util.Scanner;

class UserInput {
    Scanner scanner = new Scanner(System.in);

    public int promptInt(String message) {
        System.out.println(message);
        String userInput = scanner.nextLine();

        int userInt = 0;
        boolean isInt = false;
        while (!isInt) {
            try {
                userInt = Integer.parseInt(userInput);
                isInt = true;
            }
            catch (NumberFormatException e) {
                System.out.println(userInput + " is not a valid integer. " + message);
                userInput = scanner.nextLine();
            }
        }

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

Here, we've moved the call to Integer.parseInt() into a try block.  The
try-catch block itself is inside a while loop.  The program will keep prompting
the user for input until Integer.parseInt() doesn't throw an exception, which
means the user entered a valid value. When the user enters an invalid value,
the program displays a message and get's new input. Notice that when an
exception occurs, the value of *isInt* isn't set to *true*.  When an exception
occurs, execution is stopped and the JVM begins searching for code to handle
the exception.  Since we specified code to handle the exception in the catch
block, the JVM then begins executing that code.  Execution continues after
the try-catch block and not where it stopped in the try block. If we did not
handle the exception in the UserInput.promptInt() method, the JVM would look
to the method that called UserInput.promptInt() for code to handle the
exception.

## Cleanup
In some cases, there might be code we'd like to run before execution leaves
a method because of a thrown exception.  Java provides the ability to execute
such code using a *finally block*.  The **finally block** consists of the
reserved word `finally` followed by code to be executed after a try block
regardless of whether or not an exception occurred.  If a catch block is
specified, the code in the catch block will be executed before the finally
block.  If no catch block is specified, the code in the finally block will be
executed before the exception is thrown and execution leaves the method.  
You'll often see finally blocks when consuming data from outside the program,
from files for example. In these cases, we have to open a resource to access
the data and should close them when we are finished using them regardless of
whether an exception occurs or not; the code to close the resource appears in
a finally block.  We'll look at examples of this later in the course.  But, to
demonstrate a finally block, consider this example:

```java
package com.myname.week_06;

import java.util.Scanner;

class UserInput {
    Scanner scanner = new Scanner(System.in);

    public int promptInt(String message) {
        System.out.println(message);
        String userInput = scanner.nextLine();

        int userInt = 0;
        boolean isInt = false;
        while (!isInt) {
            try {
                userInt = Integer.parseInt(userInput);
                isInt = true;
            }
            catch (NumberFormatException e) {
                System.out.println(userInput + " is not a valid integer. " + message);
                userInput = scanner.nextLine();
            }
            finally {
                System.out.println("This line is always executed.");
            }
        }

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

In this example, we have a finally block that simply displays some text.  
Regardless of whether the user enters valid input or not, the text will be
displayed.  Note that when you run this program and enter an invalid integer,
the finally block will not immediately execute because the catch block is
waiting for user input.  After user input is supplied and the catch block has
no more code for execution, the finally block is executed.


## Exercise
Write a class that can be used to collect user input and has three methods:

- public String promptString(String message)
- public int promptInt(String message)
- public double promptDouble(String message)

Each of these methods should prompt the user for input using the specified
message and return the a String, int, or double, depending on the method.  The
methods should catch any exceptions due to bad input and continue to prompt the
user for input until valid input is supplied.
