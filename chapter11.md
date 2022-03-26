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
```
</td>
</tr>
</table>