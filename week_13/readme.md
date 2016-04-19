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
package com.myname.week_13;

public class Main {
    public static void main(String[] args) {
        Runnable runnable = new Runnable()
        {
            @Override
            public void run() {
                String name = Thread.currentThread().getName();
                int count = 0;
                while (count < 10)
                    System.out.println(name + ": " + count++);
            }
        };

        Thread threadA = new Thread(runnable);
        Thread threadB = new Thread(runnable);
        System.out.println("Starting threads");
        threadA.start();
        threadB.start();
        try {
            threadA.join();
            threadB.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("End of Main.main()");
    }
}
```

After starting the threads, we can call each thread's *join()* method to wait
for the thread to complete execution before continuing with the rest of the
code.  *Thread.join()* potentially throws an *InterruptedException* exception
if the thread is interrupted so we have to handle it.

Running the program will produce output similar to the following:

```
Starting threads
Thread-1: 0
Thread-1: 1
Thread-0: 0
Thread-0: 1
Thread-0: 2
Thread-0: 3
Thread-0: 4
Thread-0: 5
Thread-0: 6
Thread-0: 7
Thread-0: 8
Thread-0: 9
Thread-1: 2
Thread-1: 3
Thread-1: 4
Thread-1: 5
Thread-1: 6
Thread-1: 7
Thread-1: 8
Thread-1: 9
End of Main.main()
```

The output indicates that both threads were running simultaneously. The order
of output will likely change from one execution of the program to the next and
is also dependent on the underlying operating system.  If we did not wait for
the other threads to complete execution before resuming main by calling their
*join()* methods, we would likely have seen the "End of Main.main(()" message
earlier.

Exceptions can occur within threads.  If an exception occurs and it is not
handled, the thread will terminate.  While we can attempt to handle execptions
in the *Runnable.run()* method, we might wish to handle similar exceptions in
the same way regardless of the code in *Runnable.run()*.  To do this, we can
create an uncaught exception handler.  The following code will cause the thread
to terminate.

```
public class Main {
    public static void main(String[] args) {
        Runnable runnable = new Runnable()
        {
            @Override
            public void run() {
                int quotient = 1 / 0;
            }
        };

        Thread threadA = new Thread(runnable);
        System.out.println("Starting threads");
        threadA.start();
        try {
            threadA.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("End of Main.main()");
    }
}
```

Note that while the thread doing the calculation terminates, the main thread
continues execution.  We can specify uncaught exception handlers by
implementing the *UncaughtExceptionHandler* interface and associating with
all threads or with specific threads individually.  The following code creates
an uncaught exception handler and uses the
*Thread.setDefaultUncaughtExceptionHandler()* method to associate it will all
instances of the *Thread* class.  Java offers finer control of setting
uncaught exception handlers, for example we could have used the
*setUncaughtExceptionHandler()* method on the thread instance.

```Java
package com.myname.week_13;

public class Main {
    public static void main(String[] args) {
        Runnable runnable = new Runnable()
        {
            @Override
            public void run() {
                int quotient = 1 / 0;
            }
        };

        Thread.setDefaultUncaughtExceptionHandler(
            new Thread.UncaughtExceptionHandler() {
                @Override
                public void uncaughtException(Thread t, Throwable e) {
                    System.out.println("Caught throwable " + e + " for thread "
                            + t);
                }
            });

        Thread threadA = new Thread(runnable);

        System.out.println("Starting threads");
        threadA.start();
        try {
            threadA.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("End of Main.main()");
    }
}
```

The output is similar to the following:

```
Starting threads
Caught throwable java.lang.ArithmeticException: / by zero for thread Thread[Thread-0,5,main]
End of Main.main()
```

### Synchronization
While threads can execute independently from other other threads, they may not
be completely isolated.  Often threads will access and modify shared data. Care
must be taken to avoid problems due to this sharing of data.  The following
example demonstrates a shared checking account form which two people are both
withdrawing funds.  

```Java
package com.myname.week_13;

class CheckingAccount
{
    private int balance;

    public CheckingAccount(int initialBalance)
    {
        balance = initialBalance;
    }

    public int withdraw(int amount)
    {
        if (amount <= balance)
        {
            try {
                Thread.sleep((int) (Math.random() * 200));
            }
            catch (InterruptedException ie) {
            }

            balance -= amount;
        }
        return balance;
    }
}

public class Main {
    public static void main(String[] args) {
        CheckingAccount account = new CheckingAccount(100);

        Runnable r = new Runnable() {
            @Override
            public void run() {
                String name = Thread.currentThread().getName();
                for (int i = 0; i < 10; i++) {
                    System.out.println(name + " tries to withdraw $10, balance: " +
                            account.withdraw(10));
                }
            }
        };

        Thread thdHusband = new Thread(r);
        thdHusband.setName("Husband");

        Thread thdWife = new Thread(r);
        thdWife.setName("Wife");

        thdHusband.start();
        thdWife.start();
    }
}
```

Running this program will generate code that includes something similar to the
following:

```
Wife withdraws $10, balance: 0
Husband withdraws $10, balance: -10
```

While the code checks to make sure that the amount to be withdrawn doesn't
exceed the available balance, a problem arises when when both threads are
attempting to withdraw money at nearly the same time: both threads check the
balance unaware that the other thread is about decrease the available balance
so the `amount <= balance` condition is true in both threads and both
threads decrease the balance below the available amount. This is an example of
a race condition.  A **race condition** is a scenario in which multiple threads
are accessing data and the final result is dependent only on the timing of how
the threads are executed.  The race condition exists in this example because
checking the account balance and decreasing it are not atomic.  An
*atomic operation* is an operation that appears to the system to occur
instantaneously and appear to be indivisible.  Note that the call to the
*sleep()* method in the example serves to exaggerate the problem. Because the
comparison between the balance and the withdraw amount and the action of
decreasing the balance would normally execute so quickly, we might have to run
the program many times before seeing the effect of the race condition.

A solution to this problem is to synchronize access to the *withdraw()* method.
**Synchronized access** to a method allows only one thread to execute the
method at a time; other threads must wait for the executing thread to complete
before they can access the method.  To synchronize access, we can prefix the
method header with `synchronized`.  This modified code will not allow the
balance in the account to fall below zero.

```Java
package com.myname.week_13;

class CheckingAccount
{
    private int balance;

    public CheckingAccount(int initialBalance)
    {
        balance = initialBalance;
    }

    synchronized public int withdraw(int amount)
    {
        if (amount <= balance)
        {
            try {
                Thread.sleep((int) (Math.random() * 200));
            }
            catch (InterruptedException ie) {
            }

            balance -= amount;
        }
        return balance;
    }
}

public class Main {
    public static void main(String[] args) {
        CheckingAccount account = new CheckingAccount(100);

        Runnable r = new Runnable() {
            @Override
            public void run() {
                String name = Thread.currentThread().getName();
                for (int i = 0; i < 10; i++) {
                    System.out.println(name + " tries to withdraw $10, balance: " +
                            account.withdraw(10));
                }
            }
        };

        Thread thdHusband = new Thread(r);
        thdHusband.setName("Husband");

        Thread thdWife = new Thread(r);
        thdWife.setName("Wife");

        thdHusband.start();
        thdWife.start();
    }
}
```

## Concurrency Utilities
Working with the Threads API and *Thread* objects gives us a a low-level way of
creating code that will execute in multiple threads.  The Concurrency Utilities
Framework provides a higher-level framework that can be used to address
problems often encountered when working with the Threads API.  We'll briefly
examine some of the features of the Concurrency Utilities Framework.

### Executors
An *Executor* is an objet that implements the *java.util.concurrent.Executor*
interface and decouples task submission from task execution.  When working with
*Thread* objects, the same object is responsible for thread creation and
running the task.  *Executor* alone isn't all that useful - it doesn't provide
a way to track running tasks, it can't execute a collection of tasks (like
*Thread*), and there's no easy way to get a return value from any code executed
in a thread (also like *Thread*).  For these features, we can instead use an
instance of *ExecutorService*.  The *java.util.concurrent.Executors* class
provides methods for creating instances of *ExecutorService*.  

Executors also allow us to represent our tasks by classes that implement the
*Callable<V>* generic interface where *V* represents the type of a result
returned by the *call()* method used to represent a task.  This is different
than the *Runnable* interface and its *run()* method that doesn't allow us to
return a value.  

When working with an instance of *ExecutorService*, results will often be
wrapped in *Future* objects.  the *Future* interface represents results that
will not be available until some time in the future.  Instance of *Future* have
methods such as *isDone()* and *get()* to check if a value has been assigned
and to get a value.  

As an example of working with executors, suppose we want to retrieve data from
several websites.  We can create a thread pool, a collection of threads to
which tasks are assigned, to collect the data.  While this example won't
actually get data from the internet, code in a future lecture will.  

```java
package com.myname.week_13;


import java.util.*;
import java.util.concurrent.*;

// a class to simulate data retrieval
class SiteDownloader {
    static final Map<String, String> data;

    // class initializer to set fake site data
    static {
        Map<String, String> fakeData = new HashMap<>();
        fakeData.put("http://www.weather.gov", "Weather Forecast");
        fakeData.put("http://www.espn.com", "Sports Scores");
        fakeData.put("http://www.marketwatch.com","Stock Market Data");
        fakeData.put("http://www.fandango.com", "Movie Showtimes");
        data = Collections.unmodifiableMap(fakeData);
    }

    // a method to retrieve
    static String get(String URL) {
        return data.get(URL);
    }
}

public class Main {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(2);
        List<Future<String>> results = new ArrayList<>();
        String[] urls = {"http://www.weather.gov", "http://www.espn.com",
                "http://www.marketwatch.com", "http://www.fandango.com"};

        //submit tasks to executor service
        for (String url : urls) {
            Callable callable = new Callable<String>() {
                @Override
                public String call() throws Exception {
                    System.out.println("Retrieving URL: " + url);
                    return SiteDownloader.get(url);
                }
            };

            System.out.println("Submitting task for " + url);
            results.add(executor.submit(callable));
        }

        // wait for tasks to finish
        boolean done = false;
        while (!done) {
            System.out.println("Still getting data...");
            done = true;
            for (Future<String> result : results) {
                if (!result.isDone()) {
                    done = false;
                }
            }
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        try {
            for (Future<String> result : results) {
                System.out.println("Site data: " + result.get());
            }
        } catch (InterruptedException | ExecutionException e) {
            e.printStackTrace();
        }
    }
}
```


### Synchronizers

### Concurrent Collections

## Exercise
