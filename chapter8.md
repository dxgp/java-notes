# Chapter 8: Inheritance

## Basics

### Keywords
Some basic definitions:
* superclass: class that is inherited
* subclass: class that does the inheriting.
* `extends`: keyword for inheriting.

Take the following program:
```java
class A{
    int i,j;
    void showij(){
        System.out.println("i and j: "+i + " "+j);
    }
}

class B extends A{
    int k;
    void showk(){
        System.out.println("k: "+k);
    }
    void sum(){
        System.out.println("i+j+k: "+ (i+j+k));
    }
}

public class Main {
    public static void main(String[] args) {
        B subOb = new B();
        subOb.i = 1;
        subOb.j = 2;
        subOb.k = 3;
        subOb.showij();
        subOb.showk();
        subOb.sum();
    }
}
```
The output is:
```
i and j: 1 2
k: 3
i+j+k: 6
```

So, we can use B as if A's properties also apply to B (except for members that are declared as private in the superclass). Another interesting propery is that any variable that's declared of the superclass type can be assigned to a subclass object. 

### Using super
Consider the two implementations of the same thing:

<table>
<tr>
<th>Without Super</th>
<th>With Super</th>
</tr>
<tr>
<td>
```java
class BoxWeight extends Box {
    double weight;
    BoxWeight(double w, double h, double d, double m) {
        width = w;
        height = h;
        depth = d;
        weight = m;
    }
}
```
</td>
<td>

```java
class BoxWeight extends Box {
    double weight;
    BoxWeight(double w, double h, double d, double m) {
        super(w,h,d);
        weight = m;
    }
}
```

</td>
</tr>
</table>

Here, we used super to essentially tell Java to use the superclass's constructor for variables that belong to it. Besides the betteer look, there's another advantage: it can be used to initialize members of the superclass that may be private. Since we don't have to access it, they can be initialized by the superclass without interference. Another use for super is that it can be used like `this` but for referencing the superclass.

We can extend classes to create multilevel heirarchies. `super()` is a little bitch in the sense that it must be the first statement executed in a subclass' constructor. If `super()` is not actually called, the constructor of the lowest subclass calls the constructor of superclasses with void parameters. The code below demonstrates this:
<table>
<tr>
<td>
Class C is a subclass of B which is a subclass of A. When a C object is created in the main method, A's ctor is called followed by B's followed by C's. 
</td>
<td>

```java
class A{
    A(){
        System.out.println("Inside A's ctor");
    }
}
class B extends A{
    B(){
        System.out.println("Inside B's ctor");
    }
}
class C extends B{
    C(){
        System.out.println("Inside C's  ctor");
    }
}
```
</td>
</tr>
</table>

```java

```


