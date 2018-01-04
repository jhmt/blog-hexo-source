title: OOP Principles - SOLID
date: 2016-02-21 11:19:40
categories: [OOP, SOLID]
tags: [OOP, SOLID]
---

**SOLID** is an acronym of the following five basic principles of Object Oriented Programming and design.

- <a href="#SRP">Single Responsibility Principle</a>
- <a href="#OCP">Open Closed Principle</a>
- <a href="#LSP">Liskov Substitution Principle</a>
- <a href="#ISP">Interface Segregation Principle</a>
- <a href="#DIP">Denedency Inversion Principle</a>


<h2 id="SRP">1. Single Responsibility Principle</h2>
*There should never be more than one reason for a class to change*

#### What:
- 2 reasons to change means 2 responsibilities.

#### Why:
- A class with multiple responsibilities is likely unrobust.

#### Example:
- Modem class

<img align="left" src="{% post_path SOLID-Principles %}Modem.png" />
<br clear="left">

Responsibilities of Modem class can be separated into **Managing connection(Connect/Disconnect)** and **Data communication(Send/Receive)**. If these responsibilities are modified separately, this class violates the SRP.

#### How:
1. Design abiding by <a href="https://en.wikipedia.org/wiki/GRASP_(object-oriented_design)">GRASP</a>
2. Work with design patterns: Proxy, Facade or Iterator
3. TDD


<h2 id="ODP">2. Open-Closed Principle</h2>
*Classes should be open for extension but closed for modification.*

#### What:
- "Open"
<ul><li>Extensible for behaviors</li></ul>
- "Closed"
<ul><li>Changes to behaviors DO NOT affect to the source code or assemblies</li></ul>

#### Why:
- For benefits of OOP: Flexibility, Reusability and Maintenancibility

#### Example:
- Violated case

<img align="left" src="{% post_path SOLID-Principles %}OCP1.png" />
<br clear="left">

Client is using Server class. The Client class has to be changed to connect to another Server.

- Open-Closed case:

<img align="left" src="{% post_path SOLID-Principles %}OCP2.png" />
<br clear="left">

No changes have to be made when the Server is modified.

#### How:
1. Work with design patterns: Strategy or Template Method
2. TDD


<h2 id="LSP">3. Liskov Substitution Principle</h2>
*Subtypes must be substitutable for their base types.*

#### What:
- A method uses the base class can use any sub classes.
- Designer/Programmer has to guarantee the behavior of sub class as same as the base class.

#### Why:
- Violoation of LSP means violoation of OCP

#### Example:
- Violated case

<img align="left" src="{% post_path SOLID-Principles %}LSP1.png" />
<br clear="left">

<pre class="brush: c-sharp;">
public void DrawShape(Shape shape)
{
    if (shape is Square)
    {
        Square sq = (Square)shape;
        sq.Draw();
    }
    else if (shape is Circle)
    {
        Circle c = (Circle)shape;
        c.Draw();
    }
}
</pre>

Square and Circle classes cannot substitute Shape the base class.


#### How:
1. Design by Contract


<h2 id="ISP">4. Interface Segregation Principle</h2>
*Clients should not be forced to depend upon interfaces that they do not use.*

#### What:
- An interface should not have too many features. (That is called 'fat' interface.)

#### Why:
- A client class has to implements all the 'fat' interface's methods even if the class doesn't need all of them. This could cause unnessesary modifications.

#### Example:
- Violated case

<img align="left" src="{% post_path SOLID-Principles %}ISP1.png" />
<br clear="left">

IStorage interface has *RollBack* and *Save* methods. However, *RollBack* method is used only by DBStorage class.


#### How:
Split interfaces according to its feature.

<img align="left" src="{% post_path SOLID-Principles %}ISP2.png" />
<br clear="left">


<h2 id="DIP">5. Dependency Inversion Principle</h2>
*High level modules should not depend upon low level modules. Both should depend upon abstractions. Secondly, abstractions should not depend upon details. Details should depend upon abstractions.*

#### What:
- A module should receive low level modules upon using them without having strong dependency.

#### Why:
- A high level module depending on low level ones is affected by the low level modifications.
- This should be inverted because high level should be superior.

#### Example:

<img align="left" src="{% post_path SOLID-Principles %}DIP1.png" />
<br clear="left">

High level modules don't depend on low level modules.


#### How:
Design with DI(IoC) container.


