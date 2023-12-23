---
title: 'Builder pattern'
date: 2021-04-06
permalink: /posts/design-patterns/builder/
tags:
  + coding
  + design patterns
---

Every programmer sooner or later encounters a class that requires passing multiple parameters to create its object.
Creating such classes can be significantly streamlined by using the pattern we'll discuss today.

Before delving into it, let's start with creating some sample code.
Let's say we need a class whose object represents a house.
While creating it, we'll be able to set its parameters such as the number of floors, floor height, number of bathrooms, and so on.


```java
public class House {
    int numberOfFloors;
    int numberOfWindows;
    double floorHeight;
    int numberOfBathrooms;
    boolean heatedFloor = false;
    boolean solarPanels = false;
    boolean centralHeating = false;

    public House(int numberOfFloors, int numberOfWindows, double floorHeight, int numberOfBathrooms) {
        this.numberOfFloors = numberOfFloors;
        this.numberOfWindows = numberOfWindows;
        this.floorHeight = floorHeight;
        this.numberOfBathrooms = numberOfBathrooms;
    }

    public void setHeatedFloor(boolean heatedFloor) {
        this.heatedFloor = heatedFloor;
    }

    public void setSolarPanels(boolean solarPanels) {
        this.solarPanels = solarPanels;
    }

    public void setCentralHeating(boolean centralHeating) {
        this.centralHeating = centralHeating;
    }
}
```

When creating an instance of our class, we need to pass appropriate parameters through its constructor.
Additionally, it contains setters to configure optional parameters, responsible in this case for features like underfloor heating or solar panels.

Typically, this is how classes are created and their fields initialized—either through a constructor with arguments or a parameterless constructor and setters.
In this case, we have both setters and a constructor that requires parameters.
Let's now create an instance of this class.

```java
public class NewHouse {
    public static void main(String []args) {

        House house = new House(2, 10, 2.2, 3);
        house.setHeatedFloor(true);
        house.setSolarPanels(true);
    }
}
```

To create it, we need to pass four parameters through the constructor.
As seen in the example above, there's nothing particularly challenging here.
But what if you revisit this code after several months?
Or if someone else reads it?
While creating the object, we pass several different numbers that mean absolutely nothing to the reader!
To understand what's happening, they'd need to dive into the class code or rely on sophisticated IDEs, which nowadays can provide reasonable descriptions of the provided parameters.
Despite IDE capabilities, it's essential to ensure our code remains readable, even for someone who opens it in a simple text editor.

Imagine also what would happen if we needed to pass a lot more data to the object.
The constructor would become even less readable and inconvenient to use.
One way to address this issue is by using a parameterless constructor and setters for all class fields.

```java
public class NewHouse {
    public static void main(String []args) {

        House house = new House();
        house.setNumberOfFloors(2);
        house.setNumberOfWindows(10);
        house.setFloorHeight(2.2);
        house.setNumberOfBathrooms(3);
        house.setHeatedFloor(true);
        house.setSolarPanels(true);
    }
}
```

Now it's clear what those numbers refer to.
However, there's a certain issue with this method.
By using it, we can't compel the client to set all the required class fields.
In other words, an instance of this class could be created and used in the program without setting its required fields.

So, how do we address these aspects? This is where the Builder Pattern comes to our aid.

## Builder pattern

The Builder pattern belongs to the group of creational patterns.
It's used to encapsulate the object's construction process and to enable its multi-step initialization.
In general, a builder is an additional class that facilitates the creation of another class.

Let's try applying it to the class created earlier.

```java
public class House {
    int numberOfFloors;
    int numberOfWindows;
    double floorHeight;
    int numberOfBathrooms;
    boolean heatedFloor;
    boolean solarPanels;
    boolean centralHeating;

    private House() {};

    static final class Builder {
        private int numberOfFloors;
        private int numberOfWindows;
        private double floorHeight;
        private int numberOfBathrooms;
        private boolean heatedFloor = false;
        private boolean solarPanels = false;
        private boolean centralHeating = false;

        public Builder numberOfFloors(int numberOfFloors){
            this.numberOfFloors = numberOfFloors;
            return this;
        }

        public Builder numberOfWindows(int numberOfWindows){
            this.numberOfWindows = numberOfWindows;
            return this;
        }

        public Builder floorHeight(double floorHeight){
            this.floorHeight = floorHeight;
            return this;
        }

        public Builder numberOfBathrooms(int numberOfBathrooms){
            this.numberOfBathrooms = numberOfBathrooms;
            return this;
        }

        public Builder heatedFloor(boolean heatedFloor){
            this.heatedFloor = heatedFloor;
            return this;
        }

        public Builder solarPanels(boolean solarPanels){
            this.solarPanels = solarPanels;
            return this;
        }

        public Builder centralHeating(boolean centralHeating){
            this.centralHeating = centralHeating;
            return this;
        }

        public House build() {
            if(this.numberOfWindows < 2) {
                throw new IllegalStateException("House must have at least 2 windows");
            }
            if(this.floorHeight < 2) {
                throw new IllegalStateException("Floor must be at least 2 meters high");
            }
            if(this.numberOfBathrooms < 1) {
                throw new IllegalStateException("There has to be at least 1 bathroom");
            }

            House house = new House();
            house.numberOfFloors = this.numberOfFloors;
            house.numberOfWindows = this.numberOfWindows;
            house.floorHeight = this.floorHeight;
            house.numberOfBathrooms = this.numberOfBathrooms;
            house.heatedFloor = this.heatedFloor;
            house.solarPanels = this.solarPanels;
            house.centralHeating = this.centralHeating;

            return house;
        }
    }
}
```

In our House class, a new class called Builder has been created.
Its methods are used to set the values of House class fields.
Each of these methods returns a Builder object, enabling the chaining of consecutive method calls.
The `build()` method is used to construct the object—it returns a fully built House object.
In this case, it's also used to verify whether the class parameters are appropriately set.
If any parameter holds an invalid value, it throws the relevant exception.

```java
private House() {};
```

When reading the improved House class code, you probably noticed that empty, private constructor.
It prevents the creation of an instance of this class using a regular constructor.
Therefore, by writing that single line, we've enforced the usage of our builder to create a house object.

And that's all we need to do to implement the Builder pattern.
Simple, right?
From now on, we can create instances of our class like this:

```java
public class NewHouse {
    public static void main(String []args) {

        House house = new House.Builder()
                .numberOfBathrooms(3)
                .numberOfWindows(2)
                .centralHeating(true)
                .solarPanels(true)
                .floorHeight(2)
                .build();
    }
}
```

In my opinion, this is a much clearer and simpler way of instantiating objects, unlike those presented in the first part of the article.
Notice that apart from the increased readability, we gained the ability to pass values to our future object in any order.

## Summary

By using the Builder pattern, we ensure that the created object will be complete, and our code will be readable and easy to use.
We also have the ability to enforce the use of the builder to create an object, which, if handled properly within the builder, ensures consistency in our class.

### Advantages of the Builder pattern

- Encapsulates operations necessary to create a complex object.
- Enables creating objects in a step-by-step procedure without enforcing that procedure (unlike one-step patterns like Factory).
- Hides the internal representation of the created object from the client.
- Implementations of created objects can be swapped as the client only interacts with the abstract interface.

Like everything, this pattern also has its drawbacks. Unfortunately, while using the Builder, the client needs to possess a greater understanding of the problem domain compared to using the Factory pattern.
