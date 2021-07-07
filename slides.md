layout: true

.signature[@algogrit]

---
class: center, middle

# Go Design Patterns

Gaurav Agarwal

---
class: center, middle

## Who is this class for?

---

- Experienced Go developers

- Looking to skill up & work with more complex challenges

- Looking to refactor & simplify existing code bases

---
class: center, middle

## What are we going to learn?

---

- Why everything your friends know about factory patterns is wrong?

- Find out how `Starbuzz` coffee doubled their stock price with the Decorator pattern

- Avoiding those embarrassing coupling mistakes

- Discover the secrets of the pattern guru

.content-credits[https://www.oreilly.com/library/view/head-first-design/0596007124/]

---

class: center, middle

![Me](assets/images/me.png)

Software Engineer & Product Developer

Principal Consultant & Founder @ https://codermana.com

ex-Tarka Labs, ex-BrowserStack, ex-ThoughtWorks

---

class: center, middle

Co-organizer of Chennai Go meetup

Volunteer at Golang India - Remote study group

---

class: center, middle

*What we wanted*

![In-class Training](assets/images/professional-training-courses.jpg)

---

class: center, middle

*What we got*

![WFH](assets/images/wfh.jpg)

---

## As an instructor

- I promise to

  - make this class as interactive as possible

  - use as many resources as available to keep you engaged

  - ensure everyone's questions are addressed

---

## What I need from you

- Be vocal

  - Let me know if there any audio/video issues ASAP

  - Feel free to interrupt me and ask me questions

- Be punctual

- Give feedback

- Work on the exercises

- Be *on mute* unless you are speaking

---
class: center, middle

## Class Progression

---
class: center, middle

Here you are trying to *learn* something, while here your *brain* is doing you a favor by making sure the learning doesn't stick!

.content-credits[https://www.oreilly.com/library/view/head-first-design/0596007124/]

---

### Some tips

- Slow down => stop & think
  - listen for the questions and answer

- Do the exercises
  - not add-ons; not optional

- There are no dumb questions!

- Drink water. Lots of it!

---

### Some tips (continued)

- Take notes
  - Try: *Repetitive Spaced Out Learning*

- Talk about it out loud

- Listen to your brain

- Design something

---
class: center, middle

### 📚 Content ` > ` 🕒 Time

---
class: center, middle

## Show of hands

*Yay's - in Chat*

---
class: center, middle

## Before we dive into patterns

---

- Methods, interfaces and inheritance

- **SOLID** principles

---
class: center, middle

### Methods, interfaces and inheritance

---
class: center, middle

#### Methods

---
class: center, middle

Methods are known as receivers or receiver functions in Go

---

- Receivers can be defined on any user-defined type in Go

- You can define multiple receivers with same name but on different receiver types, within a package

- They need to be defined in the same package as the `type` is defined in

- Getters & Setters are anti-patterns in Go

- Two kinds of receiver functions: *value* receivers & *pointer* receivers

---

##### Value receivers

- `func (<varName> <receiverType>) <funcNameA>() { ... }`
  - Eg. `func (f MyFloat) Abs() float64 { ... }`

---
class: center, middle

There is no `this` or `self` keyword in go, which could give you a hold of the variable on which you are dispatching a receiver.

---
class: center, middle

They are instead passed in to the receiver, the same way you have arguments to a function.

---

##### Pointer receivers

- `func (<varName> *<receiverType>) <funcNameB>() { ... }`
  - Eg. `func (v *Vertex) Scale(by float64) { ... }`

---

Pointer receivers can help in:

- Avoiding copy of a large-ish struct variable

- Update fields of a struct variable

- Useful for dispatching even on `nil` values

---

The value & pointer receivers can be dispatched interchangeably:

- `var v <receiverType>`
  - `v.funcNameA()` & `v.funcNameB()`

- `p := &v`
  - `p.funcNameA()` & `p.funcNameB()`

---
class: center, middle

#### Interfaces

---
class: center, middle

Interfaces are implicit in Go!

---

- `interface` keyword

- `type Foo interface { ... }`

- It contains only method definitions

---

##### Rules

1. If any type implements all of the methods required by the interface, only then it implicitly implements the interface

2. if a value type implements an interface, then the pointer to the value type also implements the interface

3. If any of the receiver methods, required by interface `I` on type `A` are pointer receiver, then only `*A` will be implementing an interface

---

```go
type I interface {
  Foo()
  Bar()
}

type A struct{}

func (a *A) Foo() {}

func (a A) Bar() {}
```

then,

```go
var i I

// i = A{} // Will result in compiler error

i = &A{}
```

---
class: center, middle

#### Inheritance

---
class: center, middle

go has inheritance through *composition*!

---
class: center, middle

##### Struct "inheritance"

---
class: center, middle

A struct type can compose or "embed" another type

---

```go
type Bar string

func (b Bar) bar() {}

type Foo struct {
  Bar
  // Bar Bar
}
```

---

- Here the custom type `Foo` "inherits" Bar's bar method

- If a struct composes a struct, it can also "inherit" Fields

---
class: center, middle

Multiple inheritance is possible and easy to deal with

---
class: center, middle

##### Interface "inheritance"

---
class: center, middle

An interface can compose another interface

---
class: center, middle

## Software Design

---
class: center, middle

### Code Reviews

---
class: center, middle

What makes you say "that code is ugly" or "wow that code is beautiful"?

---
class: center, middle

### Bad code!?

---
class: center, middle

What are some of the properties of bad code that you might pick up on in code review?

---

- *Rigid*. Is the code rigid? Does it have a straight jacket of overbearing types and parameters, that making modification difficult?

- *Fragile*. Is the code fragile? Does the slightest change ripple through the code base causing untold havoc?

- *Immobile*. Is the code hard to refactor? Is it one keystroke away from an import loop?

- *Complex*. Is there code for the sake of having code, are things over-engineered?

- *Verbose*. Is it just exhausting to use the code? When you look at it, can you even tell what this code is trying to do?

.content-credits[https://dave.cheney.net/2016/08/20/solid-go-design]

---
class: center, middle

### Good Design

---
class: center, middle

### **SOLID** principles

.content-credits[https://dave.cheney.net/2016/08/20/solid-go-design]

---
class: center, middle

#### Single-Responsibity principle

---
class: center, middle

> A class should have one, and only one, reason to change. - Robert C Martin

---

- Coupling & Cohesion

Coupling is simply a word that describes two things changing together – a movement in one induces a movement in another.

In the context of software, cohesion is the property of describing pieces of code are naturally attracted to one another.

---

- Package names

A package’s name is both a description of its purpose, and a name space prefix.

---

- Bad package names

What does package `server` provide?... well a server, hopefully, but which protocol?

What does package `private` provide? Things that I should not see? Should it have any public symbols?

And package `common`, just like its partner in crime, package `utils`, is often found close by these other offenders.

---
class: center, middle

Catch all packages like these become a dumping ground for miscellany, and because they have many responsibilities they change frequently and without cause.

---

##### Go's UNIX philosophy

> Do one thing, and do it well.

---
class: center, middle

#### Open / Closed Principle

---
class: center, middle

> Software entities should be open for extension, but closed for modification. - Bertrand Meyer

---
class: center, middle

Go does not support function overloading or even overriding

---
class: center, middle

#### Liskov Substitution principle

---
class: center, middle

> Coined by Barbara Liskov, the Liskov substitution principle states, roughly, that two types are substitutable if they exhibit behaviour such that the caller is unable to tell the difference.

---
class: center, middle

> In a class based language, Liskov’s substitution principle is commonly interpreted as a specification for an abstract base class with various concrete subtypes. But Go does not have classes, or inheritance, so substitution cannot be implemented in terms of an abstract class hierarchy.

---
class: center, middle

##### Interface

---
class: center, middle

> Require no more, promise no less. – Jim Weirich

---
class: center, middle

#### Interface segregation principle

---
class: center, middle

> Clients should not be forced to depend on methods they do not use. - Robert C Martin

---
class: center, middle

In Go, the application of the interface segregation principle can refer to a process of isolating the behavior required for a function to do its job.

---
class: center, middle

> A great rule of thumb for Go is **accept interfaces, return structs**. - Jack Lindamood

---
class: center, middle

#### Dependency Inversion principle

---
class: center, middle

> High-level modules should not depend on low-level modules. Both should depend on abstractions.
> Abstractions should not depend on details. Details should depend on abstractions.
> – Robert C. Martin

---

- If you’ve applied all the principles we’ve talked about up to this point then your code should already be factored into discrete packages, each with a single well defined responsibility or purpose.

- Your code should describe its dependencies in terms of interfaces, and those interfaces should be factored to describe only the behaviour those functions require.

---
class: center, middle

> In other words, there shouldn’t be much left to do.

---
class: center, middle

In Go, your import graph must be acyclic. A failure to respect this acyclic requirement is grounds for a compilation failure, but more gravely represents a serious error in design.

---

class: center, middle

![Clean Architecture](assets/images/CleanArchitecture.jpg)

---

#### Layers

- Entities (entities)

  Defines all the Models in the application

- Repository

  Encapsulates the interaction with the database. This is the lowest layer in the application.

- Services

  Orchestrates the interaction with repository and other services.

---

#### Layers (continued)

- Transport

  Encapsulates the interaction with application over an http API

- Binaries

  The code in `cmd/server` ties all the layers together, in order the start the app.

---
class: center, middle

Sample App: [YAES Server](https://github.com/algogrit/yaes-server)

---

#### SOLID - summary

The Single Responsibility Principle encourages you to structure the functions, types, and methods into packages that exhibit natural cohesion; the types belong together, the functions serve a single purpose.

The Open / Closed Principle encourages you to compose simple types into more complex ones using embedding.

The Liskov Substitution Principle encourages you to express the dependencies between your packages in terms of interfaces, not concrete types. By defining small interfaces, we can be more confident that implementations will faithfully satisfy their contract.

---

#### SOLID - summary (continued)

The Interface Substitution Principle takes that idea further and encourages you to define functions and methods that depend only on the behaviour that they need. If your function only requires a parameter of an interface type with a single method, then it is more likely that this function has only one responsibility.

The Dependency Inversion Principle encourages you move the knowledge of the things your package depends on from compile time–in Go we see this with a reduction in the number of import statements used by a particular package–to run time.

---
class: center, middle

> interfaces let you apply the SOLID principles to Go programs. - Dave Cheney

---
class: center, middle

> **Design** is the art of arranging code that needs to work today, and to be easy to change forever. - Sandi Metz

---
class: center, middle

### Design Patterns

---
class: center, middle

Design patterns are typical solutions to commonly occurring problems in software design. They are like pre-made blueprints that you can customize to solve a recurring design problem in your code.

---
class: center, middle

There are at least 24 known design patterns.

.content-credits[https://refactoring.guru/design-patterns/catalog]

---
class: center, middle

![Catalog of Design Patterns](assets/images/catalog.png)

.content-credits[https://refactoring.guru/design-patterns/catalog]

---
class: center, middle

![Relationship between patterns](assets/images/patterns-relationship.png)

.content-credits[Design Patterns: Elements of Reusable Object-Oriented Software by GoF]

---

Patterns can be categorized into 3:

- Creational

- Structural

- Behavioral

---
class: center, middle

#### Creational Pattern

---
class: center, middle

*Creational* design patterns abstract the instantiation process. They help make a system independent of how it's objects are created, composed and represented.

---
class: center, middle

The basic form of object creation could result in design problems or in added complexity to the design. *Creational* design patterns solve this problem by somehow controlling this object creation.

---

- Builder

- Factories

- Prototype

- Singleton

---
class: center, middle

##### Builder Pattern

---
class: center, middle

Lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

---

Imagine a complex object that requires laborious, step-by-step initialization of many fields and nested objects. Such initialization code is usually buried inside a monstrous constructor with lots of parameters. Or even worse: scattered all over the client code.

---
class: center, middle

![House builder](assets/images/patterns/builder-house-problem.png)

---
class: center, middle

![House Constructor](assets/images/patterns/house-constructor-problem.png)

---
class: center, middle

##### Factory Pattern

---
class: center, middle

The Factory Method pattern suggests that you replace direct object construction calls with calls to a special factory method.

---

Imagine that you’re creating a logistics management application. The first version of your app can only handle transportation by trucks, so the bulk of your code lives inside the Truck class.

At present, most of your code is coupled to the Truck class. Adding Ships into the app would require making changes to the entire codebase. Moreover, if later you decide to add another type of transportation to the app, you will probably need to make all of these changes again.

---
class: center, middle

![logistics Solution](assets/images/patterns/factory-logistics-sol.png)

---
class: center, middle

##### Prototype Pattern

---
class: center, middle

Prototype is a creational design pattern that lets you copy existing objects without making your code dependent on their classes.

---

Say you have an object, and you want to create an exact copy of it. How would you do it? First, you have to create a new object of the same class. Then you have to go through all the fields of the original object and copy their values over to the new object.

---
class: center, middle

![Prototype Problem](assets/images/patterns/factory-logistics-sol.png)

---
class: center, middle

To perform a copy of a prototype, we need to be able to perform a **deep copy**

---

- The Prototype pattern delegates the cloning process to the actual objects that are being cloned.

- The pattern declares a common interface for all objects that support cloning.

- This interface lets you clone an object without coupling your code to the class of that object.

- Usually, such an interface contains just a single clone method.

---
class: center, middle

##### Singleton Pattern

---
class: center, middle

Singleton is a creational design pattern that lets you ensure that a class has only one instance, while providing a global access point to this instance.

---

Why would anyone want to control how many instances a class has? The most common reason for this is to control access to some shared resource—for example, a database or a file.

Or the construction call is expensive.

Or provide a global access point to that instance.

---
class: center, middle

![Singleton client view](assets/images/patterns/singleton-client.png)

---

###### Cons of Singleton

- Violates the Single Responsibility Principle. The pattern solves two problems at the time.

- The Singleton pattern can mask bad design, for instance, when the components of the program know too much about each other.

---

###### Cons of Singleton (continued)

- The pattern requires special treatment in a multi-threaded environment so that multiple threads won’t create a singleton object several times.

- It may be difficult to unit test the client code of the Singleton because many test frameworks rely on inheritance when producing mock objects. Since the constructor of the singleton class is private and overriding static methods is impossible in most languages, you will need to think of a creative way to mock the singleton. Or just don’t write the tests. Or don’t use the Singleton pattern.

---
class: center, middle

Code
https://github.com/AgarwalConsulting/Go-Training/tree/master/patterns/design

Slides
https://go-design-patterns.slides.algogrit.com
