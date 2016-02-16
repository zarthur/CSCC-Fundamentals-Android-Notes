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

### Checked vs Runtime Exceptions

### Custom Exception Classes

## Throwing Exceptions

## Handling Exceptions

## Cleanup

## Exercise
