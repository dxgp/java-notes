# Chapter 11: Multithreaded Programming

## Basics
Processes are heavy tasks that require their own address space. On the other hand, threads are lighter and share the same address space. 

While single threaded systems use an approach called an **event loop with polling** in which a single thread continuously polls a single event queue to decide what to do next. Once the queue replies, the thread gives up control and waits for the new thread to finish execution. Nothing else can happen until this new thread returns.

## Thread Priorities
Priorities are integers that decide the relative importance of threads. As an absolute value, a priority is meaningless because priority is used to decide if a context switch should be made. A context switch can occur under two scenarios:

1. The thread voluntarily gives up control. In this case, the next highest priority task is given to the CPU.
2. The thread is preempted by a higher priority thread. This is called *preemptive multitasking*.

For synchronization, java uses monitors. However, there's no `Monitor` class. Instead, each object has its own monitor that is automatically entered when a synchronization method is called.

## Thread Class
To create a new thread, a program will either extend Thread or implement the Runnable interface. Some methods defined by the thread class are:

1. `getName`: Get a thread's name.
2. `getPriority`: Get a thread's priority
3. `isAlive`: Determine if a thread is still running.
4. `join`: Wait for a thread to terminate.
5. `run`: Entry point for the thread.
6. `sleep`: Suspend a thread for a period of time.
7. `start`: Start a thread by calling its run method.

## The Main Thread

<table>
<tr>
<td>
The output here is:
<pre>
Current thread:Thread[main,5,main]
After name change:Thread[My Thread,5,main]
5
4
3
2
1
</pre>

First, we get a reference to the main thread using <code>currentThread()</code>. The <code>InterruptedException</code> try and catch is added in case a thread wanted to interrupt the sleeping main thread. 

Another thing to note that when the thread information is printed, it's printed as: [<i>name</i>, <i>priority</i>, <i>group name</i>] (default name is main and default priority is 5). This is known as a **thread group**, which is a data structure that controls the state of a collection of threads as a whole.
</td>
<td>

```java
class CurrentThreadDemo{
    public static void main(String[] args){
        Thread t = Thread.currentThread();
        System.out.println("Current thread:" + t);
        t.setName("My Thread");
        System.out.println("After name change:" + t);
        try{
            for(int n=5;n>0;n--){
                System.out.println(n);
                Thread.sleep(1000);
            }
        } catch(InterruptedException e){
            System.out.println("Main thread interrupted");
        }
    }
}
```

</td>
</tr>
</table>

## Creating a Thread

### Method 1: Implementing Runnable
To implement runnable, a class only needs to implement a single method run as `public void run()`. 

<table>
<tr>
<td>
Here, the main method creates a new thread (using the <code>NewThread</code> constructor which creates a new thread by instantiating an object of type <b>Thread</b>. Also notice how the constructor uses <code>this</code> in the constructor) and calls <code>start()</code> which then calls <code>run()</code>. The output of the program is:

<pre>
Child thread: Thread[Demo Thread,5,main]
   Main Thread: 5
   Child Thread: 5
   Child Thread: 4
   Main Thread: 4
   Child Thread: 3
   Child Thread: 2
   Main Thread: 3
   Child Thread: 1
   Exiting child thread.
   Main Thread: 2
   Main Thread: 1
   Main thread exiting.
</pre>

Also, the main thread is the last one to exit because it has to run some cleanup functions.
</td>
<td>

```java
class NewThread implements Runnable{
    Thread t;
    NewThread(){
        t = new Thread(this, "Demo thread");
        System.out.println("Child thread:" + t);
    }
    public void run(){
        try{
            for(int i=5; i>0;i--){
                System.out.println("Child thread: " + i);
                Thread.sleep(500);
            }
        } catch (InterruptedException e){
            System.out.println("Child interrupted");
        }
        System.out.println("Exiting child thread");
    }
}

class ThreadDemo{
    public static void main(String[] args){
        NewThread nt = new NewThread();
        nt.t.start();
        try{
            for(int i=5; i>0;i--){
                System.out.println("Main thread:"+ i);
                Thread.sleep(1000);
            } 
        } catch (InterrtuptedException e){
            System.out.println("Main thread interrupted.");
        }
        System.out.println("Main thread exiting");
    }
}
```
</td>
</tr>
</table>

### Method 2: Extending Thread
<table>
<tr>
<td>
Here, we pass the string to the <code>Thread</code> superclass.
</td>
<td>

```java
class NewThread extends Thread{
    NewThread(){
        super("Demo thread");
        System.out.println("Child thread:" + this);
    }
    public void run(){
        try{
            for(int i = 5; i>0; i--){
                System.out.println("Child thread:" + i);
                Thread.sleep(500);
            }
        } catch (InterruptedException e) {
            System.out.println("Child interrupted");
        }
        System.out.println("Exiting child thread.");
    }
}

class ExtendThread{
    public static void main(String[] args){
        NewThread nt = new NewThread();
        nt.start();
        try{
            for(int i=5; i>0;i--){
                System.out.println("Main thread:"+ i);
                Thread.sleep(1000);
            } 
        } catch (InterrtuptedException e){
            System.out.println("Main thread interrupted.");
        }
        System.out.println("Main thread exiting.");
    }
}
```
</td>
</tr>
</table>

Now, the question becomes: 

> Which approach is better?
Neither. It's a matter of taste. Kind of like the old vim vs emacs war.

## Creating Multiple Threads
<table>
<tr>
<td>
<pre>
   New thread: Thread[One,5,main]
   New thread: Thread[Two,5,main]
   New thread: Thread[Three,5,main]
   One: 5
   Two: 5
   Three: 5
   One: 4
   Two: 4
   Three: 4
   One: 3
   Three: 3
   Two: 3
   One: 2
   Three: 2
   Two: 2
   One: 1
   Three: 1
   Two: 1
   One exiting.
   Two exiting.
   Three exiting.
   Main thread exiting.
</pre>
</td>
<td>

```java
class NewThread implements Runnable{
    String name;
    Thread t;
    NewThread(String threadname){
        name = threadname;
        t = new Thread(this, name);
        System.out.println("New thread:" + t);
    }
    public void run(){
        try{
            for(int i = 5; i>0; i--){
                System.out.println(name + ":" + i);
                Thread.sleep(1000);
            }
        } catch (InterruptedException e){
            System.out.println(name + "Interrupted");
        }
        System.out.println(name + " exiting.");
    }
}
class MultiThreadDemo {
    public static void main(String[] args){
        NewThread nt1 = new NewThread("One");
        NewThread nt2 = new NewThread("Two");
        NewThread nt3 = new NewThread("Three");

        nt1.t.start();
        nt2.t.start();
        nt3.t.start();
        try{
            Thread.sleep(10000);
        } catch (InterruptedException e){
            System.out.println("Main thread interrupted");
        }
        System.out.println("Main thread interrupted");
    }
}
```

</td>
</tr>
</table>

## Using isAlive() and join()
To ensure that the main thread finishes last, we can use `join()`. The `isAlive()` function, when called on a thread returns `true` if the thread on which it is called is still running. 

<table>
<tr>
<td>
Here, we called <code>join()</code> on the three threads. The main thread can only exit once these three joins are done which will only happen once they have finished execution.
</td>
<td>

```java
class NewThread implements Runnable {
         String name; // name of thread
         Thread t;
         NewThread(String threadname) {
           name = threadname;
           t = new Thread(this, name);
           System.out.println("New thread: " + t);
}
         // This is the entry point for thread.
         public void run() {
           try {
             for(int i = 5; i > 0; i--) {
               System.out.println(name + ": " + i);
               Thread.sleep(1000);
             }
           } catch (InterruptedException e) {
             System.out.println(name + " interrupted.");
}
           System.out.println(name + " exiting.");
         }
}
class DemoJoin {
  public static void main(String[] args) {
    NewThread nt1 = new NewThread("One");
    NewThread nt2 = new NewThread("Two");
    NewThread nt3 = new NewThread("Three");
    // Start the threads.
    nt1.t.start();
    nt2.t.start();
    nt3.t.start();
    System.out.println("Thread One is alive: "
                        + nt1.t.isAlive());
    System.out.println("Thread Two is alive: "
                        + nt2.t.isAlive());
    System.out.println("Thread Three is alive: "
                        + nt3.t.isAlive());
    // wait for threads to finish
    try {
      System.out.println("Waiting for threads to finish.");
      nt1.t.join();
      nt2.t.join();
      nt3.t.join();
    } catch (InterruptedException e) {
      System.out.println("Main thread Interrupted");
}
    System.out.println("Thread One is alive: "
                        + nt1.t.isAlive());
    System.out.println("Thread Two is alive: "
                        + nt2.t.isAlive());
    System.out.println("Thread Three is alive: "
                        + nt3.t.isAlive());
    System.out.println("Main thread exiting.");
  }
}
```

</td>
</tr>
</table>

> Thread priorities: to set thread priorities, call the `getPriority()` method on the thread and pass the new priority as the argument. The new priority must be within the range [**MIN_PRIORITY**,**MAX_PRIORITY**] which is currently [1,10]. There's another constant called **NORM_PRIORITY**, which is the default priority (currently, this value is 5).

## Synchronization
There are two methods to synchronize code:

<hr>
<center>Method 1: Using Synchronized Methods</center>
<hr>
Suppose we want to print the following:
<pre>
[Hello]
[Synchronized]
[World]
</pre>

Here, the catch is that each of these words must be printed by a separate thread. Say that to achieve this, we write the following code:

<table>
<tr>
<td>
The output of this program will resemble:
<pre>
[Hello[Synchronized[World]
]
]
</pre>
To fix this, we just need to add the <code>synchronized</code> keyword in front of the <code>callme</code> method i.e. 
<pre>
synchronized void call(String msg)
</pre>
</td>
<td>

```java
class Callme{
    void call(String msg){
        System.out.print("[" + msg);
        try{
            Thread.sleep(1000);
        } catch(InterruptedException e){
            System.out.println("Interrupted");
        }
        System.out.println("]");
    }
}
class Caller implements Runnable{
    String msg;
    Callme target;
    Thread t;
    public Caller(Callme targ, String s){
        target = targ;
        msg = s;
        t = new Thread(this);
    }
    public void run(){
        target.call(msg);
    }
}
class Synch{
    public static void main(String[] args){
        Callme target = new Callme();
        // make the shared object same i.e. target
        Caller ob1 = new Caller(target, "Hello");
        Caller ob2 = new Caller(target, "Synchronized");
        Caller ob3 = new Caller(target, "World");
        ob1.t.start();
        ob2.t.start();
        ob3.t.start();
        try{
            ob1.t.join();
            ob2.t.join();
            ob3.t.join();
        } catch (InterruptedException e){
            System.out.println("Interrupted");
        }
    }
}
```

</td>
</tr>
</table>

<hr>
<center>Method 2: Using Synchronized Statement</center>
<hr>

This can be best explained using code. Here's how we can achieve the same effect as the above code using the `synchronized` statement:

Just change `public void run()` from:

```java
public void run(){
    target.call(msg);
}
```
To:
```java
public void run(){
    synchronized(target){
        target.call(msg);
    }
}
```

## Interthread Communication
Java provides three methods to facilitate interthread communication (all three are implemented as final and so, all threads have them). These three methods can only be called from a synchronized context:

1. `wait()`: Tells the calling thread to give up the monitor and go to sleep until some othher thread enters the same monitor and calls `notify()` or `notifyAll()`.
2. `notify()`: Wakes up a thread that called `wait()` on the same object.
3. `notifyAll()`: Wakes up all the threads that called `wait()` on the same object.


