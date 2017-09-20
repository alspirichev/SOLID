# SOLID

**SOLID** is an acronym for the first five object-oriented design principles

- [The Single Responsibility Principle (SRP)](#the-single-responsibility-principle-srp)
- [The Open/Closed Principle (OCP)](#the-open-closed-principle-ocp)
- [The Liskov Substitution Principle (LSP)](#the-liskov-substitution-principle-lsp)
- [Source](#source)


# The Single Responsibility Principle (SRP)

> <h3> A class should have only one reason to change.

### :pushpin: Description:

For example, consider the next design of the app.

The *Rectangle* class has **two** methods shown. One draws the rectangle on the screen, and the other computes the area of the rectangle.

![Violates SRP](https://github.com/alspirichev/SOLID/blob/master/SRP/1.Png)

Two different applications use the *Rectangle* class. One application does computational geometry. Using Rectangle to help it with the mathematics of geometric shapes but never drawing the rectangle on the screen. The other application is graphical in nature and may also do some computational geometry, but it definitely draws the rectangle on the screen.

:fire:**This design violates SRP.**:fire:

The *Rectangle* class has **two** responsibilities. The **first** responsibility is to provide a mathematical model of the geometry of a rectangle. The **second** responsibility is to render the rectangle on a GUI.

The violation of **SRP** causes several nasty problems:

* We must include *GUI* in the computational geometry application. In .NET, the *GUI* assembly would have to be built and deployed with the computational geometry application.

* If a change to the **GraphicalApplication** causes the *Rectangle* to change for some reason, that change may force us to rebuild, retest, and redeploy the **ComputationalGeometryApplication**. If we forget to do this, that application may **break** in unpredictable ways.

:minidisc: A better design is to **separate** the two responsibilities into two completely different classes, as shown below:

![SRP compliant image](https://github.com/alspirichev/SOLID/blob/master/SRP/2.png)

> 
> In the context of the **SRP**, we define a responsibility to be a *reason for change*. If you can think of more than one motive for changing a class, that class has more than one responsibility. This is sometimes difficult to see.
> 

### Responsibility

In general, a class **is assigned the responsibility to know or do something (one thing)**.

### Examples:

* Class **PersonData** is responsible for knowing the data of a person.

* Class **CarFactory** is responsible for creating Car objects.

### Cohesion (conceptual view)

* Cohesion measures the degree of togetherness among the elements of a class.

* In a class with high cohesion every element is part of the implementation of exactly one concept. The elements of the class work together to achieve one common functionality.

* A class with high cohesion often implements only one responsibility.

* Classes with high cohesion:
	* can be reused easily,

	* are easily understood,

	* protect clients from changes, that should not affect them

### When to apply the SRP?

* We **should split** a class that has two responsibilities if:
	* Both responsibilities will change separately.

	* The responsibilities are used separately by other classes.

	* Responsibilities pertain to optional features of the system.

* We **should not split** responsibilities if:
	* Both responsibilities will only change together, e.g. if they together implement one common protocol.

	* Both responsibilities are only used together by other classes.

	* Responsibilities pertain to mandatory features.
	
# The Open Closed Principle (OCP)

> <h3> Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.

### Description of OCP

Modules that conform to OCP have two primary attributes:

1. They are **open** for extension.

This means that the behavior of the module can be **extended**. As the requirements of the application change, we can extend the module with new behaviors that satisfy those changes. In other words, we are able to change what the module does.

2. They are **closed** for modification.

Extending the behavior of a module **does not result** in changes to the source, or binary, code of the module. The binary executable version of the modulewhether in a linkable library, a DLL, or a .EXE fileremains untouched.

- How is it possible that the behaviors of a module can be modified without **changing** its source code?
- Without changing the module, how can we change what a module does?

:tada: The answer is **abstraction**. :tada:

In C# or any other object-oriented programming language, it is possible to create **abstractions** that are fixed and yet represent an unbounded group of possible behaviors. The abstractions are abstract base classes, and the unbounded group of possible behaviors are represented by all the possible derivative classes.

### Example:

Figure 1 shows a simple design that does **not conform** to OCP. Both the **Client** and **Server** classes are **concrete**. The **Client** class uses the **Server** class. If we want for a **Client** object to use a **different** server object, the **Client** class must be changed to name the new server class.

Figure 1:

![OCP](https://github.com/alspirichev/SOLID/blob/master/OCP/1.png)

Figure 2:

![OCP](https://github.com/alspirichev/SOLID/blob/master/OCP/2.png)

Figure 2 shows the corresponding design that **conforms** to the OCP by using the *STRATEGY* pattern. In this case, the **ClientInterface** class is abstract with abstract member functions.

The **Client** class uses this abstraction. However, objects of the **Client** class will be using objects of the derivative **Server** class. If we want **Client** objects to use a **different** server class, a new derivative of the **ClientInterface** class can be created. The **Client** class can remain unchanged.

:interrobang: Why didn't I call it **AbstractServer**? 

The reason, as we will see later, is that *abstract classes are more closely associated to their clients than to the classes that implement them*.

> <h3> No matter how closed a module is, there will always be some kind of change against which it is not closed.

# The Liskov Substitution Principle (LSP)

> <h3> Subtypes must be substitutable for their base types.

This principle:

* gives us a way to characterize **good inheritance hierarchies**

* increases our awareness about traps that will cause us to create hierarchies that do not conform to the open-closed principle.

### Example:

* Assume, we have implemented a class **Rectangle** in our system.

```objective-c
class Rectangle {
	public void setWidth(int width) { this.width = width; }

	public void setHeight(int height) { this.height = height;}

	public void area() { return height * width;}
}
```

* Let's now assume that we want to implement a class **Square** and want to maximize **reuse**.

![Class hierarchy](https://github.com/alspirichev/SOLID/blob/master/LSP/1.png)

Implementing **Square** as a *subclass* of **Rectangle**:

```objective-c
class Square extends Rectangle {

	public void setWidth(int width) {
		super.setWidth(width);
		super.setHeight(width);
	}

	public void setHeight(int height) {
		super.setWidth(height);
		super.setHeight(height);
	}
}
```

* Now we can pass **Square** wherever **Rectangle** is expected.

A client that works with instances of **Rectangle**, but **breaks** when instances of **Square** are passed to it.

```objective-c
void clientMethod(Rectangle rec)
{
	rec.setWidth(5);
	rec.setHeight(4);
	assert(rec.area() == 20);
}
```

### LSP Compliant Solution

![LSP Compliant Solution](https://github.com/alspirichev/SOLID/blob/master/LSP/2.png)

### Behavioral Substitutability

So what does the **LSP** add to the common object-oriented subtyping rules?

The **LSP** additionally requires *behavioral substitutability*.

![Behavioral Substitutability](https://github.com/alspirichev/SOLID/blob/master/LSP/3.png)

* It’s not enough that instances of **SomeSubclass1** and **SomeSubclass2** provide all methods declared in **SomeClass**. These methods should also behave like their heirs!

* A client method should not be able to distinguish the behavior of objects of **SomeSubclass1** and **SomeSubclass2** from that of objects of **SomeClass**.

### Behavioral Subtyping

**S** is a behavioral subtype of **T**, if objects of type **T** in a program **P** may be replaced by objects of type **S** without altering any of the properties of **P**.

Consider a function **f** parameterized over type **T**:

* **S** is a derivate of **T**

* When passed to **f** in the guise of objects of type **T**, objects of type **S** cause f to **misbehave**.

* **S** violates the **LSP**. 

=> **f** is fragile in the presence of **S**.

# :books: Source

* [Software Engineering Design & Construction](http://stg-tud.github.io/sedc/Lecture/ws16-17/)
* Agile Software Development; Robert C. Martin; Prentice Hall, 2003
