# 04-05 Threads and HTTP Lab

For this lab, you will learn about and practice with **concurrency and threading**, which are used extensively by Android, using a framework-specific set of classes and options. However, in order to get a feel for what it means for a program to work on separate threads, you'll be implementing concurrency in pure Java (though we can use similar code in Android).

As an additional component of this lab, you will review the Java code used to send **network requests**. Android will use _exactly_ this code, but in order to experiment with it separate from the Android framework you'll be making network connections directly from Java.

## Part 1. Algorithm Races!
One of the main concerns of computer science and software in general is speed: how fast will a particular program or algorithm run? For example, give two of the [many sorting algorithms](https://en.wikipedia.org/wiki/Sorting_algorithm#Popular_sorting_algorithms) that have been invented, which one can sort a list of numbers more quickly?

- Sorting algorithms are usually covered in _CSE 373_, but don't worry if you haven't taken that course yet! All you need to know is that there are different techniques for sorting numbers, these techniques are given funny names, and one technique may be faster than another

Consider the provided `SortRacer.java` class (found in the `src/main/java` folder). The `main` method for this program runs two different sorting algorithms (currently [Merge Sort](https://en.wikipedia.org/wiki/Merge_sort) and [Quicksort](https://en.wikipedia.org/wiki/Quicksort)), reporting when each one is finished.

You can run this program using gradle: `./gradlew -q runSorts`. Note that it may take a few seconds for it to build and begin running, and the sorting itself may take a few seconds!

### Concurrency
Of course, it's not really a "race" at the moment: rather, each sorting algorithm is run **serially** (that is, one after another). If we really wanted them to race, we'd like the algorithms to run **concurrently** (at the same time).

Computers as a general rule do exactly one thing a time: your central processing unit (CPU) just adds two number together over and over again, billions of times a second 

- The standard measure for _rate_ (how many times per second) is the `hertz` (Hz). So a 2 gigahertz (GHz) processor can do 2 billion operations per second.

However, we don't realize that computers do only one thing at a time! This is because computers are really good at _multitasking_: they will do a tiny bit of one task, and then jump over to another task and do a little of that, and then jump over to another task and do a little of that, and then back to the first task, and so on.

These "tasks" are divided up into two types: **processes** and **threads**. [Read this brief summary of the difference between them](https://docs.oracle.com/javase/tutorial/essential/concurrency/procthread.html).

So by breaking up a program into threads (which are "interwoven"), we can in effect cause the computer to do two tasks at once. This is _especially_ useful if one of the "tasks" might take a really long time--rather than **blocking** the application, we can let other tasks also make some progress while we're waiting for the long task to finish.

### Threading the Race
Currently the two sorting algorithms run in the same thread, one after another. For this lab, you will break them into two _different_ threads that can run **concurrently**, letting them actually be able to race!

In Java, we create a Thread by creating a class that `implements` the [**`Runnable`**](https://docs.oracle.com/javase/8/docs/api/java/lang/Runnable.html) interface. This represents a class that can be "run" in a separate thread! The `run()` method required by the interface acts a bit like the "main" method for that Thread: when we start the Thread running, that is the method that will get called.

**Create two new `Runnable` classes, one for each Sorting method**. 

- These should be [nested classes](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html) (think: should they be `static`?). 
- When each `Runnable` is `run`, you should create a new _shuffled_ array of numbers and then call the appropriate sorting method on that list. Remember to print out when you start and finish sorting (just like is currently done in the `main()` method).

If we just instantiate the `Runnable()` and call its `run()` method, that won't actually execute the method on a different thread (remember: an interface is just a "sign"; we could have called the interface and method whatever we wanted and it would still compile). Instead, we execute code on a separate thread by using an instance of the [**`Thread`**](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html) class. This class actually does the work of running code on a separate thread.

`Thread` has a [constructor](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html#Thread-java.lang.Runnable-) that takes in a `Runnable` instance as a parameter---you pass an object representing the "code to run" to the `Thread` object (this is an example of the _Strategy pattern_). You then can actually **start** the `Thread` not by calling its `.start()` method (_not_ the `run` method!).

**Modify the `main()` method so you create new `Threads` to execute each `Runnable`** Make sure you actually `start()` the threads!

- Anonymous variables will be useful here; you don't need to assign a variable name to the `Runnable` objects or even the `Thread` objects if you just use them directly.

Now run your program! Do you see the Threads running at the same time? Try running the program multiple times and see what kind of differences you get.

- There are some print statements you can uncomment in the `Sorting` class if you want to see more concrete evidence of the Threads running concurrently.

- You are also welcome to try racing different sorting algorithms (you'll want to use a smaller list of numbers, particularly for the painfully slow BubbleSort). You can even race more than two algorithms---just create additional Threads!
    - But don't spend too long on this, as there is still more to do for the lab!

And that's the basics of creating Threads in Java! If you have any questions, please let me know!


## Part 2. HTTP Requests
For your second task, consider the provided `MovieDownloader.java` class (found in the `src/main/java/` folder). This Java code (which is _directly_ portable to Android) accesses the database at [omdbapi.com](http://www.omdbapi.com/), a wrapper around the IMDB API calls for getting information about movies.

You can run this program with the `./gradlew -q runMovies` task. It will prompt you for a movies to search for, and then print out the results (in JSON format).

**Your task is to add ___comments___ to the `downloadMovieData()` method**, explaining what the code does and how it works. The goal is to understand the classes and methods are that are being used here (particularly the use of [`HttpUrlConnection`](https://docs.oracle.com/javase/8/docs/api/java/net/HttpURLConnection.html), [`InputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/InputStream.html), and [`BufferedReader`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)), and demonstrate that understanding through explanatory comments. You should also pay particular attention to the use of `try/catch` blocks (see [here](https://docs.oracle.com/javase/tutorial/essential/exceptions/handling.html) for one explanation).

Note that we'll utilize this exact code in Android, so you should be familiar with what it is doing!













