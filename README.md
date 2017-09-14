# SOLID

**SOLID** is an acronym for the first five object-oriented design principles

- [The Single Responsibility Principle (SRP)](#the-single-responsibility-principle-srp)
  - [Description](#description)
  - [Responsibility](#responsibility)
  - [Examples](#examples)
  - [Cohesion (conceptual view)](#cohesion-conceptual-view)
  - [When to apply the SRP?](#when-to-apply-the-srp)
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
  
  # :books: Source

* [Software Engineering Design & Construction](http://stg-tud.github.io/sedc/Lecture/ws16-17/)
* Agile Software Development; Robert C. Martin; Prentice Hall, 2003
