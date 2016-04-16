# Week 13 - Concurrency and Threading

## Corresponding Text
*Learn Java for Android Development*, pp. 324-354, 487-536

## Threads
A **thread** is a path of execution through a program's code.  So far, all our
programs have executed following one path in a single thread, the main thread.
While this is fine for a lot of programs, it is sometimes necessary to allow
multiple threads to execute simultaneously.  For example, if we had a lot of
different, independent tasks to perform, we could executive them at the same
time in different threads.  Another example is if we had a computationally-
intensive task that we wanted to perform in the background without interfering
with the user interface, we might execute that task in a thread separate from
the one handling the UI.  

Since Java 5, Java has included a collection of concurrency utilities that are
typically preferred for working with threads.  Before we look that those
utilities, it's important to understand some low-level concepts.

### Runnable and Thread
The *Runnable* interface is used to supply code for threads to execute.  The
interface declares a single method, `void run()`, that takes no parameters and
returns no value.  The following is an example of an an instance created
using an anonymous class implementing *Runnable*

```Java
Runnable r = new Runnable() {
  @Override
  public void run() {
    // code to execute
  }
};
```

The *Thread* class provides a method of interfacing with the underlying
operating system's thread management.  A single operating system thread is
associated with a thread instance.  The *Thread* class has a variety of
constructors, some which take a instance of *Runnable* and some that do not; if
we do not plan on providing an instance of *Runrable* we must extend *Thread*
and override its *run()* method, providing code to execute.

The following is a simple example of a multi-threaded program.  In addition to
the main thread, the two threads we create both execute code specified by the
*Runnable* instance.  Note that we have to explicitly start a thread by calling
its *start()* method.

```Java
```

Running the program will produce output similar to the following:

```
Thread-0: 2
Thread-1: 0
Thread-1: 1
Thread-0: 3
Thread-1: 2
Thread-1: 3
Thread-0: 4
Thread-1: 4
Thread-0: 5
Thread-1: 5
Thread-0: 6
Thread-0: 7
Thread-1: 6
Thread-0: 8
Thread-1: 7
Thread-0: 9
Thread-1: 8
Thread-1: 9
```

The output indicates that both threads were running simultaneously. Note that
the order of output will likely change from one execution of the program to the
next and is also dependent on the underlying operating system.  

### Synchronization
### Deadlock
### Thread-Local Variables

## Concurrency Utilities
### Executors
### Synchronizers
### Concurrent Collections
### Locking Framework
### Atomic Variables

## Exercise
