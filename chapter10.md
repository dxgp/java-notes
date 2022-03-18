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
The fact that the exception is called <pre>ArithmeticException</pre> is shown by the stack trace. The program outputs:

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
