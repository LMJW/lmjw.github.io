---
date: 2022-04-05
title: OOP design notes
author: LMJW
tags: [OOP, CS]
---

## Decomposition & Generalization

### Association
Loosely coupled relationship between two objects. Below are the code
representation.

UML represents this with solid line.

```java
public class Student{
    public void play(Sport sport){
        ...
    }
}

public class Wine{
    public void pair( food){
        ...
    }
}

```

### Aggregation
More like `has a` relationship where a whole has parts that belong to it. The
code example is below.

Uml represents with empty diamond. Diamond is on the class that has other class.

```java
public class Airliner{
    private ArrayList<CrewMember> crew;

    public Airliner(){
        crew = new ArrayList<CrewMember>();
    }
    public void add(CrewMember crewMember) {
        ...
    }
}

public class PetStore{
    private ArrayList<Pet> pets;

    public PetStore(){
        pets = new ArrayList<Pet>();
    }
    public void add(Pet pet){
        ...
    }
}
```

### Composition
Strong has a relation. Parts needs to co-exist. e.g. House & Room. Room will not
be exist without a house. See the example code.

UML represent this with a solid diamond. Diamond is on the class that has the
other class.

```java
public class Human{
    private Brain brain;

    public Human(){
        brain = new Brain();
    }
}

public class Employee {
    private Salary salary;

    public Employee(){
        salary = new Salary();
    }
}

```
## Generalization with inheritance
UML: a empty arrow. Superclass is the head of arrow and subclass is the tail of
the arrow.


## Generalization with Interfaces

UML: empty arrow with dash line. The head of arrow points to the interface and
the tail of arrow is the class implements the interface.

Interface are used to describe behaviors. E.g.

```java
public interface IAnimal{
    public void move();
    public void speak();
    public void eat();
}

public class Dog implement IAnimal{
    public void move() {...}
    public void speak() {...}
    public void eat() {...}
}

// assume the movement of the vehicle
public interface IVehicleMovement{
    public void moveOnX();
    public void moveOnY();
}

public interface IVehicleMovement3D extends IVehicleMovement {
    public void moveOnZ();
}

```

## Coupling & Cohesion

Technics to evaluate the design complexity. (Average person can only handle 7
thing at the same time)

- Coupling: complexity between modules
- Cohesion: complexity within the simple module

### Coupling

we want loosely coupled module. How do we evaluate our module? whether they are
tightly coupled or not?

- Degree
- Ease
- Flexibility

#### Degree
Number of the connections between the module and others. With coupling, you want
to keep the degree small. For instance, if the module needed to connect to other
modules through a few parameters or narrow interfaces, then the degree would be
small, and coupling would be loose.

#### Ease
How obvious are the connections between the module and others. With coupling,
you want the connections to be easy to make without needing to understand the
implementations of the other modules.

#### Flexibility
Flexibility is how interchangeable the other modules are for this module. With
coupling, you want the other modules easily replaceable for something better in
the future.

### Cohesion
Cohesion represents the clarity of the responsibilities of a module.

- High cohesion: one task and nothing else (clear purpose)
- Low cohesion: more than one task or unclear purpose

You want high cohesion for your module.

### Example

```java
public class Sensor {
    public double get(controlFlag: int){
        switch (controlFlag){
            case 0:
                return this.humidity;
                break;
            case 1:
                return this.temperature;
                break;
            default:
                throw new UnknowControlFlagException();
        }
    }
}

```
This above example is tight coupling & high cohesion. 

- the class has two purpose, which violates the cohesion principle. This is low
  cohesion.
- the get method is not clear. What is `ControlFlag` mean? we need to check the
  code to see how it works. This is tight coupling.

New design
```java

public interface ISensor{
    public double get(){}
}

public class HumiditySensor implements ISensor{
    public double get() {
        return this.humidity;
    }
}

public class TemperatureSensor implements ISensor{
    public double get() {
        return this.temperature;
    }
}
```

## Separation of Concerns

Software design goal: create software that is *Flexible*, *Reusable* and
*Maintainable*.

One principle is `Separation of Concerns`. What is a `Concern`?

Concern: A concern is a very general notion, basically it is anything that
matters in providing a solution to a problem. 

Example: Think about supermarket. The concerns for supermarket would be:
- how do I store the meat?
- how do I bake bread?
- how do I accept payment?
- how do I stock the shelf?

These concerns matters when running the business to serve their customers. How
does the supermarket handle this concerns?

There's separate department focus on each concern. Each concern poses unique
subproblem and each department knows how to handle them. This is separation of
concerns.

Concerns in software: How these abstractions are implemented in the software can
lead to more concerns. Some of these concerns may involve:
- what information on the implementation represents
- what it manipulates
- what gets presented at the end

This is an ongoing process.

### Dog example of separation of concerns

Example code:
```java
public class Dog{
    public String eatFood(Food: food) {
        ///
    }
}

public class Food{}
```
which action dog can do it on its own, which action dog needs something to help.

- Who is actually giving dog food?
- Does dog always have food to eat?
- Or is dog given food to eat by its owner?

In reality, dog can eat food, but it doesn't know anything about food until its
owner feeds it.

We needs to separate two concerns, the action of eating, and the action of
feeding the food.

```java
public class DogOwner{
    private Dog mydog;
    private Food dogFood;

    public String feedMyDog(){}
    public String buyDogFood(){}
}

public class Dog{
    private String name;
    private Food food;
    public String eatFood(Food food){}
}

public class Food{
    private String food;
}
```
We removed the concern of how to get food to god from `Dog` class and let
`DogOwner` handle that issue.

### SmartPhone example of separation of concerns

Another example: smart phone

```java
public class SmartPhone{
    private byte camera;
    private byte phone;

    public SmartPhone(){}

    public void takePhoto(){}
    public void savePhoto(){}
    public void cameraFlash(){}

    public void makePhoneCall(){}
    public void encryptOutgoingSound(){}
    public void decipherIncomingSound(){}
}

```

Our SmartPhone class has two concerns:
- act as a traditional phone
- be able to use the built-in camera to take pictures

New design:
- separate concern into separate interfaces
- implement the interfaces correspondingly

```java
public interface ICamera{
    public void takePhoto();
    public void savePhoto();
    public void cameraFlash();
}

public interface IPhone {
    public void makePhoneCall();
    public void encryptOutgoingSound();
    public void decipherIncomingSound();
}

public class FirstGenCamera implements ICamera{}
public class TraditionalPhone implements IPhone{}


public class SmartPhone{
    private ICamera myCamera;
    private IPhone myPhone;

    public SmartPhone(ICamera aCamera, IPhone aPhone){
        this.myCamera = aCamera;
        this.myPhone = aPhone;
    }

    public void useCamera(){
        return this.myCamera.takePhone();
    }
    public void usePhone(){
        return this.myPhone.makePhoneCall();
    }
}
```

## Information hiding

Information hiding allows modules of system to give others the minimum amount of
information needed to use them correctly and "hide" everything else.

## Conceptual integrity

Everyone agree on the same design principle.

## 4 design principle question
- Abstraction: what attributes & behavior do you need to model an object through
  abstraction?
- Encapsulation: how are these attributes & behavior are grouped and accessed
  through encapsulation?
- Decomposition: can my class be simplified into smaller parts using decomposition?
- Generalization: are there common things across my objects that can be
  generalized?

## UML Sequence Diagram

A Sequence Diagram describes how objects in your system interact to complete a
specific task. So it mainly describes interaction between objects.

- class with doted lines (life line)
- arrows to show message send from one object to the other
- box represents a Process
- solid line arrow (send data)
- dashed line arrow (return data)

## UML State Diagram

A state diagram is a technique that you can use to describe how your system
behaves and responds.

state diagram can describe a single object and illustrate how that object
behaves in response to a series of events in your system.

each state should have:
- state name
- state variables
- activities
    - entry
    - do 
    - exit

## Model checking
