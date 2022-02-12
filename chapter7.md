## A Closer Look at Methods and Classes

### Overloading Methods
Possible to have two methods with the same name in a class if their parameter definitions are different.

```java
package com.company;
class OverloadDemo{
    void test(){
        System.out.println("No parameters");
    }
    void test(int a){
        System.out.println("a: " + a);
    }
    void test(int a, int b){
        System.out.println("a and b: " + a + " " + b);
    }
    double test(double a){
        System.out.println("double a: " + a);
        return a * a;
    }
}


public class Main {

    public static void main(String[] args) {
	    OverloadDemo demoObj = new OverloadDemo();
        demoObj.test();
        demoObj.test(25.0);
        demoObj.test(2, 4);
        demoObj.test(53);
    }
}
```

We overload the test method 4 times. Even constructors can be overloaded.

### Objects Passed as Parameters
When primitive types are passed, they are passed by value. however, when objects are passed, they are passed by reference. Take the following example:

```java
class Test{
    void meth(int i, int j){
        i*=2;
        j/=2;
    }
}

public class Main {
    public static void main(String[] args) {
	    Test ob = new Test();
        int a = 15, b = 20;
        System.out.println("a,b (before call): " + a + "," + b);
        ob.meth(a,b);
        System.out.println("a,b (before call): " + a + "," + b);
    }
}
```
Here, a and b are copied and then passed to the method. However, consider the following scenario:

```java
class Test{
    int a, b;
    Test(int i, int j){
        a = i;
        b = j;
    }
    void meth(Test t){
        t.a*=2;
        t.b/=2;
    }
}

public class Main {
    public static void main(String[] args) {
	    Test ob = new Test(15, 20);
        System.out.println("a,b (before call): " + ob.a + "," + ob.b);
        ob.meth(ob);
        System.out.println("a,b (before call): " + ob.a + "," + ob.b);
    }
}
```
The output is:

```
a,b (before call): 15,20
a,b (before call): 30,10
```

The values of a and b are changed here. Another off topic point is that deallocation is not really a huge concern in java. As long as there's even a single reference to an object, Java will not delete it.

### Access Control

Java defines three access modifiers:

* *public*: when a member of a class is defined as public, it can be accessed by any other code.
* *private*: when a member of a class is defined as private, it can only be accessed by the members of its class.

The following code demonstrates it pretty concisely:
```java
class Test{
    public int a;
    private int b;
    void setA(int a){
        this.a = a;
    }
    void setB(int b){
        this.b = b;
    }
}
```
The following code will throw a tantrum:
```java
public class Main {
    public static void main(String[] args) {
        Test myObj = new Test();
        myObj.a = 20;
        myObj.b = 20;
    }
}
```
To modify b, we must use the method `setB()`. This is encapsulation. Data can only be modified and read in the ways we see fit.

### Using Static
Again, we start with a code sample:

```java
public class Main {
    static int a = 3;
    static int b;

    static void meth(int x){
        System.out.println("x = "+x);
        System.out.println("a = "+a);
        System.out.println("b = "+b);
    }
    static{
        System.out.println("Static block initialized!");
        b = a*4;
    }
    public static void main(String[] args) {
        meth(42);
    }
}
```
Output:
```
Static block initialized!
x = 42
a = 3
b = 12
```

All instances of the class share static variables. It's essentially a global variable in a class. Static methods come with some restriction:

1. Can only directly call other static methods.
2. Can only access static variable of their class.
3. Cannot use `this` or `super` in any way, shape, form.

When a class is loaded, everything static is executed first. The `static{}` block above can only be declared once per class. Another interesting propery of static methods is that they can be used without creating an object of that class. For example:

```java
class StaticDemo{
    static int a = 40;
    static int b = 100;
    static void callThisFunction(){
        System.out.println("a = "+ a);
        System.out.println("b = "+ b);
    }
}
public class Main {
    public static void main(String[] args) {
        StaticDemo.callThisFunction();
    }
}
```
Output:
```
a = 40
b = 100
```
Also note that you can't really abuse this propery of static functions to get access to private variables because of rule#2 of static methods.

\# Note:Another access modifier (but not really tho) is `final`. The `final` keyword allows us to create constants. Any final field cannot be modified during runtime.

## Fucking with Arrays









