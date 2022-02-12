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

The values of a and b are changed here.






