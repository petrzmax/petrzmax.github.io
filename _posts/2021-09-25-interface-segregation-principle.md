---
title: 'Interface Segregation Principle – ISP'
date: 2021-09-25
permalink: /posts/solid/interface-segregation-principle/
tags:
  coding
  principles
  SOLID
  OOP
---

We've reached the penultimate principle in the SOLID group!
This time, we'll focus on the Interface Segregation Principle.

Here's its definition:
>No client should be forced to depend on methods it does not use.

Or in other words:
>Many client-specific interfaces are better than one general purpose interface.

This definition doesn't require much explanation.
The principle conveys a straightforward message that it's better to create interfaces dedicated to specific functionalities rather than having one general interface that, in theory, would be universal.
In other words, "If something is for everything, it's for nothing."
Let's see how this looks in practice.
Let's start by creating an interface describing typical vehicle functionalities.

```java
public interface Vehicle {
    void startTheEngine();
    void driveForward();
    void driveBackwards();
}
```

Typically, a mechanical vehicle has the capability to start the engine and drive forward and backward, hence these methods are included in our interface.
Let's create a `Car` class that will implement it.

```java
public class Car implements Vehicle {
    @Override
    public void startTheEngine() {
        System.out.println("Engine started!");
    }

    @Override
    public void driveForward() {
        System.out.println("Driving forward");
    }

    @Override
    public void driveBackwards() {
        System.out.println("Driving backwards");
    }
}
```

Everything is correct, our interface fits well with classes describing cars.
However, vehicles aren't just cars, and our interface is meant to be universal.
Let's create a class representing an airplane.

```java
public class Plane implements Vehicle {
    @Override
    public void startTheEngine() {
        System.out.println("Jet engine started!");
    }

    @Override
    public void driveForward() {
        System.out.println("Driving forward");
    }

    @Override
    public void driveBackwards() {
        //Airplane can't drive backwards on its own!
    }
}
```

Most airplane models I know can't move backward on their own.
They need to be pushed into position by another vehicle.
Despite this, when implementing the `Vehicle` interface, we're forced to define the `driveBackwards()` method, even though planes don't have this functionality.
We also need to add a `fly()` method, so the Vehicle interface doesn't actually make our work easier.

Similarly, basic versions of ATVs don't have a reverse gear, so they can't move backward on their own.
Yet, we'd still need to have a definition for that method in the code and potentially throw an exception or indicate that this functionality isn't implemented.

This approach doesn't align with the Interface Segregation Principle, which states that many specific interfaces are better than one general interface.
There's no need to explain further.
Instead, let's see how the code would look in line with this principle.

Let's start by creating the interfaces:

```java
public interface MediumVehicle {
    void startTheEngine();
    void driveForward();
    void driveBackwards();
}

public interface FlyingVehicle {
    void startTheEngine();
    void driveForward();
    void fly();
}
```

This time, we have two more specific interfaces.
The first one pertains to medium-sized vehicles, which typically have a reverse gear.
Smaller vehicles like motorcycles or ATVs mostly lack this function.
The second interface concerns flying vehicles.
They generally can't move backward, and additionally, unlike land vehicles, they can also fly.
Below, you'll find classes for a car and an airplane implementing these new interfaces.

```java
public class Car implements MediumVehicle {
    @Override
    public void startTheEngine() {
        System.out.println("Engine started!");
    }

    @Override
    public void driveForward() {
        System.out.println("Going forward");
    }

    @Override
    public void driveBackwards() {
        System.out.println("Going backwards");
    }
}

public class Plane implements FlyingVehicle {
    @Override
    public void startTheEngine() {
        System.out.println("Jet engine started!");
    }

    @Override
    public void driveForward() {
        System.out.println("Driving forward");
    }

    @Override
    public void fly() {
        System.out.println("Taking off!");
    }
}
```

As you can see, there are no empty method implementations here, and the interfaces better suit the classes implementing them.
The code is therefore in line with the ISP.
In fact, we could push for an even finer breakdown and greater precision in our interfaces.
For instance, having separate interfaces for starting the engine, moving forward, moving backward, or just flying.
This would completely eliminate the scenario where an interface forces us to implement a method that doesn't make sense in a particular case.

## Summary

The principle discussed in this article is perhaps the simplest to apply among others in the SOLID group.
When programming, you should avoid creating overly general interfaces that could potentially force other programmers to implement empty methods or throw a NotImplementedException in the future.
Instead, we should divide them in such a way that it's impossible to use an interface in a class that might only implement part of its methods.
By adhering to the ISP, our code will be consistent, and people using it won't have to handle the NotImplementedException.
