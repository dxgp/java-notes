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


### Method Overriding in Heirarchies
When a method in a subclass has the **same name and signature** as the one in the superclass, it's said to override the method in the superclass. The true power of method overridingg is demonstrated with the concept of *dynamic method dispatch*.

<table>
<tr>
<td>
Here, we use the fact that a superclass object can reference objects of its subclass to create three versions of one function at runtime. This is how <b>runtime polymorphism</b> is practiced in java.The output is shown below:

<pre>
Inside A's callme method.
Inside B's callme method.
Inside C's callme method.
</pre>

Another important thing to note is that a superclass variable can reference a subclass object. This is demonstrated using the variable r that is declared as an A variable but references both B and C variables.
</td>
<td>

```java
class A{
    void callMe(){
        System.out.println("Inside A's callme method.");
    }
}
class B extends A{
    void callMe(){
        System.out.println("Inside B's callme method.");
    }
}
class C extends B{
    void callMe(){
        System.out.println("Inside C's callme method.");
    }
}
public class Main {
    public static void main(String[] args) {
        A a = new A();B b = new B();C c = new C();
        A r;
        r = a; r.callMe();
        r = b; r.callMe();
        r = c; r.callMe();
    }
}
```
</td>
</tr>
</table>


## Abstract Classes

Situations where we want to define a parent class that specifies the methods that must be implemented but does not specify *how* to implement these methods class for the use of **abstract classes**. These methods that must be implemented by subclasses are referred to as **subclasser responsibility**. Here is an example:

<table>
<tr>
<td>
Objects of an abstract class cannot be initialized. However, this does not mean that we cannot create references. Creating a variable of that type without using the new keyword to create an object is acceptable for abstract classes.


Abstract classes don't need to be completely abstract .They can include some concrete implementation which then makes the implementation of the concrete method in the subclass optional.
</td>
<td>

```java

abstract class A {
	abstract void callme();
	void callmetoo() {
		System.out.println("This is a concrete method");
	}
}

class B extends A{
	void callme(){
		System.out.println("B's implementation of callme.");
	}
}

class AbstractDemo {
	public static void main(String[] args){
		B b = new B();
		b.callme();
		b.callmetoo();
	}
}
```

</td>
</tr>
</table>

## The Final Keyword
<table>
<tr>
<td>
The final keyword can prevent overriding and inheritance. The error thrown is:

<pre>meth() in B cannot override meth() in A</pre>

Similarly, the final keyword can also prevent inheritance. Also, declaring methods as final can also provide a <b>performance boost</b> because the java compiler then just copies the method into the function that's calling it (sort of like how the preprocessor in c works) which then avoid the overhead required for a method call. A term associated with the final keyword is <b>early binding</b>. This implies that Java resolves the calls to a final method at <i>compile time</i>. (<b>late binding</b> implies resolution at runtime).
</td>
<td>

```java
public class Final{
    public static void main(String[] args){
        B b = new B();
        b.meth();
    }
}
class A{
    final void meth(){
        System.out.println("This is a final method.");
    }
}
class B extends A{
    public void meth(){
        System.out.println("Illegal");
    }
}
```
</td>
</tr>
</table>


