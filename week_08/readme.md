# Week 8 - Exception Handling and Logging

## Corresponding Text

*Learn Java for Android Development*, pp. 217-232

## Exceptions

An **exception** is an interruption in the normal behavior of a program.
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

``` java
package com.myname.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

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
                int userInput = Integer.parseInt(input.getText().toString());
                String result = "Twice your number is " + userInput * 2;
                addText(builder, result);
                output.setText(builder);
            }
        });
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

Java provides a class that serves as the base class of exceptions and
errors. This base class is `Throwable` and has several methods that are useful in
diagnosing problems when exceptions or errors occur. Based on `Throwable` are
two classes unique to exceptions and errors, `Exception` and `Error`,
respectively.  These subclasses implement all the methods the `Throwable`
class does. We'll discuss base classes, subclasses, and inheritance
later in the course.

For now, let's look at the method's available to instances of the `Throwable`
class.

| Method                                     | Description                                        |
|:-------------------------------------------|:---------------------------------------------------|
| Throwable()                                | Constructor with null detail message and cause.    |
| Throwable(String message)                  | Constructor with specified message and null cause. |
| Throwable(Throwable cause)                 | Constructor with null message and specified cause. |
| Throwable(String message, Throwable cause) | Constructor with specified message and cause       |
| Throwable getCause()                       | Return the cause of the throwable.                 |
| String getMessage()                        | Return the message of the throwable.               |
| StackTraceElement[] getStackTrace()        | Return stack trace information.                    |

In our example of user input, an exception occurs when we call
`Integer.parseInt()`.  The exception is created and thrown from within the
code associated with the parseInt method but because we haven't specified a
way to handle it it, the program terminates.  Android Studio displays this
stack trace:

``` text

2020-12-12 10:33:22.655 2976-2976/com.arthurneuman.myfirstapplication E/AndroidRuntime: FATAL EXCEPTION: main
    Process: com.arthurneuman.myfirstapplication, PID: 2976
    java.lang.NumberFormatException: For input string: "2a"
        at java.lang.Integer.parseInt(Integer.java:608)
        at java.lang.Integer.parseInt(Integer.java:643)
        at com.myname.myapplication.MainActivity$1.onClick(MainActivity.java:30)
        at android.view.View.performClick(View.java:6256)
        at android.view.View$PerformClick.run(View.java:24701)
        at android.os.Handler.handleCallback(Handler.java:789)
        at android.os.Handler.dispatchMessage(Handler.java:98)
        at android.os.Looper.loop(Looper.java:164)
        at android.app.ActivityThread.main(ActivityThread.java:6541)
        at java.lang.reflect.Method.invoke(Native Method)
        at com.android.internal.os.Zygote$MethodAndArgsCaller.run(Zygote.java:240)
        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:767)
```

The stack trace is displayed such that the most recent method is displayed
near the top.  From the stack trace, we can see that the exception was created
in the `NumberFormatException` class and occurred in the `Integer.parseInt()`
method.  What we're really interested in, though, is where, in our code, was
the statement that caused the exception.  If we continue examining the stack
traces, we see that this message that indicates that our code was being
executed in MainActivity.java at line 30.

``` text
at com.myname.myapplication.MainActivity$1.onClick(MainActivity.java:30)
```

This information will help us deal with the exception.

### Checked vs Runtime Exceptions

In Java, exceptions can belong to one of two categories: checked and unchecked.
A **checked exception** is an exception from which it is possible for the
program to recover and for which the programmer must provide a workaround.
The compiler checks all code and ensures that checked exceptions are handled.
Exception and its subclasses except RuntimeException and its subclasses
represent checked exceptions.

An **unchecked** or **runtime exception** is an exception that doesn't need
to be handled or can't be handled well ahead of time. If possible, code that
often generates runtime exceptions should be rewritten. This was the design
philosophy behind the decision to implement both checked and unchecked
exceptions in Java. Sometimes, unchecked exceptions cannot be avoided and
occur often enough that we will add code to handle them; one example is
NumberFormatException which we encountered earlier.

## Throwing Exceptions

When an exception occurs in the code we write, one option we have for dealing
with it is to throw the exception.  If the exception occurs in a method called
by some other code, throwing the exception forces the code that called the
method to handle the exception. For unchecked exceptions, we don't have
to do anything special.  Notice in the stack trace from the previous example,
an exception occurred in the onClick() method but because it wasn't
dealt with there, it became an issue for the code that called it. If an
exception occurs in a program's main method - where program execution starts -
and isn't handled, the program will terminate. This is the behavior we saw.

If a checked exception can occur in a method and the method doesn't deal with
the exception by handling it, it must throw the method to the calling code.  In
Java, all checked exceptions that can be thrown by a method must be indicated
as part of the method's header.

If, in our code, we want to create and throw an exception, we can use the
*throw* keyword with an instance of an exception class.  Let's looks at an
example that illustrates both creating a new exception and throwing it.

---

Arithmetic.java

``` java
package com.myname.myapplication;

class Arithmetic {
    public static double average(double values []) {
        double average = 0;
        for (double value: values) {
            average += value;
        }
        return average / values.length;
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
                String userInput = input.getText().toString();
                String[] values = {};

                if (userInput.length() > 0) {
                    values = input.getText().toString().split(",");
                }

                double[] numbers = new double[values.length];

                for (int i=0; i<values.length; i++) {
                    numbers[i] = Double.parseDouble(values[i]);
                }

                double average = Arithmetic.average(numbers);

                addText(builder, Double.toString(average));
                output.setText(builder);
            }
        });
    }
}
```

This code allows a user to specify a comma-separated list of nunbers in the
EditText object and will display the average when the button is clicked.
What happens when the input is empty? As the code is written, the method will
return `NaN`, which is a special floating-point value indicating that the
result is not a number.  The method returns `NaN` because the length of the
array is zero and division by zero is undefined.

What if we wanted to avoid calculating the average when the user doesn't enter
any values?  One way we could do that is to throw a checked exception and force
whatever code that called Arithmetic.average() to deal with it.

---

Arithmetic.java

``` java
package com.myname.myapplication;

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
```

In this example, we've updated the Arithmetic.average() to check if the length
of the array of values is zero.  If it is, we create a new instance of the
Exception class.  Since Exception is a subclass of Throwable, it has similar
constructors including one that takes a string parameter as a message.  Because
the Exception class represents a checked exception and because we don't address
the new exception in Arithmetic.average(), we have to indicate that the method
might throw it by updating the method's header.

This code will not compile and run.  The reason it won't compile is because
there is the possibility of encountering a checked exception in the
MainActivity.onCreate() method and we haven't handled it there or indicated
that the method could throw it.  Generally, we don't want the onCreate() method
to throw exceptions, so we should handle it.

## Handling Exceptions

To handle exceptions, both checked and unchecked, when they occur, we can
use try-catch statements.  A **try-catch statement** is code that executes
other code that might throw an exception and specifies what to do if the
exception occurs.  The syntax of a try-catch statement is:

``` java
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
                String userInput = input.getText().toString();
                String[] values = {};

                if (userInput.length() > 0) {
                    values = input.getText().toString().split(",");
                }

                double[] numbers = new double[values.length];

                for (int i=0; i<values.length; i++) {
                    numbers[i] = Double.parseDouble(values[i]);
                }

                try {
                    double average = Arithmetic.average(numbers);
                    addText(builder, Double.toString(average));
                }
                catch (Exception e) {
                    addText(builder,
                            "Unable to calculate average: " + e.getMessage());
                }
                output.setText(builder);
            }
        });
    }
}
```

We've added a try-catch block to the on-click listener.  Inside the try block
of code, we included the statement that could generate an exception. Inside the
catch block, we've added code to output some text that includes the message
associated with the exception itself.

Generally, you should put as few statements as necessary in a try block and
use the most specific type of exception in the catch block.  This makes your
code much easier to read and understand.

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
                String userInput = input.getText().toString();
                String[] values = {};

                if (userInput.length() > 0) {
                    values = input.getText().toString().split(",");
                }

                try {
                    double[] numbers = new double[values.length];

                    for (int i=0; i<values.length; i++) {
                        numbers[i] = Double.parseDouble(values[i]);
                    }
                    double average = Arithmetic.average(numbers);
                    addText(builder, Double.toString(average));
                }
                catch (Exception e) {
                    addText(builder,
                            "Unable to calculate average: " + e.getMessage());
                }
                finally {
                    addText(builder, "This code is always executed.");
                }
                output.setText(builder);
            }
        });
    }
}
```

In this example, we have done two things.  First, we moved the code that
converts the input into integers into the try block - this allows us  to handle
the case when the user enters input that can't be converted. Second, we added a
finally block that simply adds some text to the output. Regardless of whether
the user enters valid input or not, the text will be displayed.  If no
exception occurs after in the try block, the code in the finally block is
executed and then execution continues as expected.  If an exception does occur
in the try block, the code in the catch block will execute, and then the
code in the finally block will execute (the finally block will always execute
before execution leaves the current method).

## Logging

As we develop applications, we're likely to run into exceptions and errors.
When this happens, we can debug our application. At the most basic level, we
can log messages about our application's state to help determine what the
problem is.  We can also use more advanced tools to find and address issues
that we'll discuss later.

Let's look at the following code.

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

        final int[] values = {1, 2, 3, 4, 5};

        button.setOnClickListener(new View.OnClickListener() {
            int index = 0;

            @Override
            public void onClick(View view) {
                int value = values[index];
                addText(builder, Integer.toString(value));
                output.setText(builder);
                index++;
            }
        });
    }
}
```

---

When we run the app and click the button six times, the app crashes.  In this
simple example, it might be easy to see that the reason it crashes is that
we are trying to access the sixth element (at index=5) of an array with only
five elements.  If we look at the log window in Android Studio we should see a
stack trace that indicates a *ArrayIndexOutOfBoundsException* was thrown. The
details indicate that the error occurred at `int value = values[index];`.
We tried to access an element outside the bounds of the array.

Let's pretend we don't know why why the app is trying to access an element
outside the bounds of the arary.  One thing we could do is add code to log
relevant information when the button is pressed.

Modify *onCreate()* to include the following code for the the *mNextButton*'s
listener:

---

MainActivity.java

``` java
package com.myname.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {
    private static final String TAG = MainActivity.class.getSimpleName();

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

        final int[] values = {1, 2, 3, 4, 5};

        button.setOnClickListener(new View.OnClickListener() {
            int index = 0;

            @Override
            public void onClick(View view) {
                Log.d(TAG, "Current index = " + index);
                int value = values[index];
                addText(builder, Integer.toString(value));
                output.setText(builder);
                index++;
            }
        });
    }
}
```

We added a call to the static method named *d* (for debug) in the *Log* class.
The method takes two arguments: a tag that is used to identify and filter log
messages and the log message itself.  We defined the tag to be the name of
the *MainActivity* class - this is a common way of defining tags.

Before we run our app, let's create a log filter that will only show log
messages generated by code in our package and with the debug level or higher.
To do this, open the **logcat** window by clicking the **logcat** tab if it's
not selected. Near the right of the logcat window, there is a drop-down menu
with the text **Show only selected application**; we can create a new filter by
selecting **Edit Filter Configuration**.  Give the filter a name, set the
package name to the package you are working with
(like `com.myname.myfirstapplication`), unselect the **Regex** option, and set
the  log level to debug.

Now if we run the app and click the **Next** button, we should see log messages similar to the following:

``` text
2021-01-03 11:00:08.993 2447-2447/com.myname.myfirstapplication D/MainActivity: Current index = 0
2021-01-03 11:00:16.181 2447-2447/com.myname.myfirstapplication D/MainActivity: Current index = 1
2021-01-03 11:00:59.485 2447-2447/com.myname.myfirstapplication D/MainActivity: Current index = 2
2021-01-03 11:01:01.546 2447-2447/com.myname.myfirstapplication D/MainActivity: Current index = 3
```

We can use the log messages to see how the value of *index* changes as the
button is pressed.  Eventually, we'll see that we increment *index* to the
point that it corresponds to a value outside the bounds of the array.

## Exercise

Add a method to the *Arithmetic* class that calculates the quotient of two
numbers. Write the method so that it throws an exception if the divisor is zero.
Update the code in MainActivity.java to use the new method and to catch any
exceptions that occur.
