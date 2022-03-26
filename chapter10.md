# Chapter 10: Exception Handling

## Basics

> **Exception**: An obnormal condition that arises in a code sequence at run time.

When an exception occurs, an object representing that exception is created and *thrown* in the method that caused the error. The five horsemen of Java error handling are:

1. **try**: We monitor for exceptionms in the try block. If an exception occurs in the try block, it is *thrown*.
2. **catch**: The code catches the exception using catch.
3. **throw**: Keyword used to manually throw an exception
4. **throws**: Any exception thrown out of a method uses the throws keyword.
5. **finally**: Used to specify code that absolutely must be executed, regardless of the exception.


## Types of Exceptions
All exception types are subclasses of the class `Throwable` (which itself must be a subclass of the object class). So, `Throwable` sits at the top of the class heirarchy. Below it are the two subclasses: `Exception` (class we subclass to create user defined exceptions) and `Error` (used by the JRE to indicate errors that have to do with the run-time system).

<table>
<tr>
<td>
This code throws an exception:
<pre>
Exception in thread "main" java.lang.ArithmeticException: / by zero
        at Exc1.subroutine(Exc1.java:4)
        at Exc1.another_subroutine(Exc1.java:7)
        at Exc1.main(Exc1.java:10)
</pre>
It also prints a <b>stack trace</b>. This shows the call stack (main called another_subroutine which called subroutine which caused the exception).

</td>
<td>

```java
class Exc1 {
    static void subroutine(){
        int d = 0;
        int a = 10/d;
    }
    static void another_subroutine(){
        Exc1.subroutine()l
    }
    public static void main(String[] args){
        Exc1.another_subroutine();
    }
}

```
</td>
</tr>
</table>

To actually catch the exceptions, we use the **try and catch block**. This is illustrated by the following program:

<table>
<tr>
<td>
The fact that the exception is called <code>ArithmeticException</code> is shown by the stack trace. The program outputs:

<pre>
Division by zero attempted
After the try catch block
</pre>

</td>
<td>

```java
public class Exc2{
    public static void main(String[] args){
        int d,a;
        try{
            d = 0;
            a = 42/d;
            System.out.println("This won't be printed");
        } catch (ArithmeticException e){
            System.out.println("Division by zero attempted");
        }
        System.out.println("After the try catch block");
    }
}
```
</td>
</tr>
</table>

The goal of a try and catch block should be to make the program continue as smoothly as possible.

To deal with multiple exceptions, we can have more than one catch clause. The key is to remember that once an exception is thrown, catches are inspected one by one until a catch matching the exception is found. All other catches after the matching catch are ignored.

<table>
<tr>
<td>
When you use multiple catch statements, it is important to remember that exception subclasses must come before any of their superclasses. This is because a catch statement that uses a superclass will catch exceptions of that type plus any of its subclasses. Thus, a subclass would never be reached if it came after its superclass. Further, in Java, unreachable code is an error. Hence, this program is <b>erroneous</b>.
</td>
<td>

```java
class SuperSubCatch{
    public static void main(String[] args){
        try{
            int a = 0;
            int b = 42/a;
        } catch(Exception e){
            System.out.println("Generic exception catch");
        } catch(ArithmeticException e){
            System.out.println("This is never reached and causes an error");
        }
    }
}
```
</td>
</tr>
</table>


<table>
<tr>
<td>
Try statements can also be nested. When an exception occurs in a try statement on the "inside", the catch corresponding to the statement is tried. However, if none is found, it moves one level up and tries the catches on the "outside". Here, suppose we supply
</td>
<td>

```java
class NestTry{
    public static void main(String[] args){
        try{
            int a = args.length;
            int b = 42/a;
            System.out.println("a = " + a);
            try{
                if(a==1){
                    a = a/(a-a);
                }
                if(a==2){
                    int[] c = {1};
                    c[42] = 99;
                }
            } catch(ArrayIndexOutOfBoundsException e){
                System.out.println("Array index out of bounds:" + e);
            }
        } catch(ArithmeticException e){
            System.out.println("Divide by 0: " + e);
        }
    }
}
```

</td>
</tr>
</table>

## Throwing Exceptions

The flow of execution stops immediately after a **throw** is encountered. The closest try block is then checked for matching catch clause. If none is found, the default exception handler halts the program and prints the stack trace.

<table>
<tr>
<td>
The <code>throw</code> keyword is used to throw a <code>NullPointerException</code> in this code. Note that an instance of the exception must exist to be thrown.
</td>
<td>

```java
class ThrowDemo{
    static void demoproc(){
        try{
            throw new NullPointerException("demo");
        } catch (NullPointerException e){
            System.out.println("Caught inside demoproc");
        }
    }
    public static void main(String[] args){
        demoproc();
    }
}
```
</td>
</tr>
</table>


### The throws Clause
If a method is capable of throwing an exception that it does not handle, it must warn the callers by adding a `throws` keyword in the declaration (except for exceptions of type `Error`, `RuntimeException` or any of their subclasses). Take the following program:

```java
class ThrowsDemo{
    static void throwOne(){
        System.out.println("Inside throwOne");
        throw new IllegalAccessException("demo");
    }
    public static void main(String[] args){
        throwOne();
    }
}
```

This code won't compile until two changes are made:
1. We must add a `throws IllegalAccessException()` clause to the function definition.
2. Try and catch block must be added to `main` to handle the exception.

So, this would look like:

```java
class ThrowsDemo{
    static void throwOne() throws IllegalAccessException{
        System.out.println("Inside throwOne");
        throw new IllegalAccessException("demo");
    }
    public static void main(String[] args){
        try{
            throwOne();
        } catch(IllegalAccessException e){
            System.out.println("Caught:"+e);
        }
        
    }
}
```

### The finally Keyword
`finally` creates a block of code that is executed after the try/catch blocks. Whether or not an exception is thrown, the `finally` block will be executed. Note that **the finally clause is optional**.

## Creating Exception Subclasses
Self defined exceptions are defined as subclasses of `Exception`, which does not have any methods of its own but is itself a subclass of `Throwable` which provides a few methods.

<table>
<tr>
<td>
The output produced by this code is:
<pre>
Called compute(1)
Normal exit
Called compute(20)
Caught MyException[20]
</pre>
</td>
<td>

```java
class MyException extends Exception{
    private int detail;
    MyException(int a){
        detail = a;
    }
    public String toString(){
        return "MyException[" + detail + "]";
    }
}
class ExceptionDemo{
    static void compute(int a) throws MyException{
        System.out.println("Called compute(" + a + ")");
        if(a>10){
            throw new MyException(a);
        }
        System.out.println("Normal Exit");
    }
    public static void main(String[] args){
        try{
            compute(1);
            compute(20);
        } catch (MyException e){
            System.out.println("Caught:" + e);
        }
    }
}
```

</td>
</tr>
</table>

## Chained Exceptions