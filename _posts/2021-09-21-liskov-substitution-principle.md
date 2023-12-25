---
title: 'Liskov Substitution Principle – LSP'
date: 2021-09-22
permalink: /posts/solid/liskov-substitution-principle/
tags:
  coding
  principles
  SOLID
  OOP
---

We've reached the third principle from the SOLID group – the Liskov Substitution Principle (LSP).

Let's start with the definition:
>Functions that use pointers or references to base classes must be able to use objects of derived classes without knowing the details of those objects.

Therefore, if we pass an object of a derived class as an argument to a method that expects an object of the base class, the method's behavior should remain the same – we shouldn't lose its correct functionality.
If we write the code following the OCP principle, we shouldn't have an issue with this.
Ultimately, we'd only be extending the functionality of the class without modifying its behavior, such as by overriding an inherited method.
It's best to see this with an example.
First, the code that doesn't meet the LSP:

```java
public class Duck {
    private void quack() {
        System.out.println("Quack! Quack!");
    };

    public void scare() {
        this.quack();
    }
}

public class WildDuck extends Duck{
    @Override
    public void scare() {
        System.out.println("The wild duck ran away!");
    }
}
```

Here's the base class `Duck`, which has a `quack()` method that makes the duck sound and a `scare()` method defining the behavior when the duck is scared.
The `WildDuck` class inherits from `Duck` and overrides the `scare()` method – modifying its behavior.
As we know, this violates the OCP principle – we shouldn't modify previously defined functionalities.
Let's see what happens with the proper invocation of our code.

```java
public class Main {
   public static void main(String[] args) {
      List<Duck> ducks = new ArrayList<>();

      ducks.add(new Duck());
      ducks.add(new WildDuck());

      scareDucks(ducks);
   }

   public static void scareDucks(List<Duck> ducks) {
      for(Duck duck : ducks) {
         duck.scare();
      }
   }
```

I've added an instance of a regular duck and a wild duck to the duck list.
Additionally, I've written a short method that scares each duck in the given list.
Notice that this method has no knowledge of the specific duck type.
It only knows that the duck is of type `Duck` or a subclass.
Here's the result of executing this code:

```text
Quack! Quack!
Wild duck ran away!
```

As you can see, two different lines were printed even though the same method was called on both objects.
It's not surprising since `WildDuck` overrides the `quack()` method, but if other parts of the program expected a specific sound after scaring a duck, they wouldn't get it in this case.
In a real implementation, this would lead to the loss of correct functionality, which contradicts the discussed Liskov Substitution Principle.

An example often used to illustrate this principle involves rectangles and squares, calculating their areas.
There, methods return specific values, and by overriding one of them in a derived class and calling them similarly to the duck example, we can get inaccurate results.
Notice that these aren't errors that cause problems with the compilation of the program.
They simply result in its undesired behavior.

How can we fix our code to comply with the LSP?
The solution in this case isn't spectacular – we simply shouldn't override the `scare()` method.

```java
public class WildDuck extends Duck{
    public void runAway() {
        System.out.println("The wild duck ran away!");
    }
}
```

Instead, I have encapsulated the behavior related to fleeing within the new `runAway()` method.
But what if, besides the expected quacking, we wanted to add behavior to the `scare()` method without overriding methods?
This is where the Template Method pattern might come in handy!
In my opinion, it's a very simple and useful pattern that's worth knowing.
Unfortunately, that's a topic for another article 😉

## Summary

Code compliant with the Liskov Substitution Principle must always allow substituting a derived type for a base type without losing the program's correct functionality.
This functionality shouldn't be achieved using conditional statements that perform different actions based on the type of the passed object.

Remember!
Good inheritance means that derived classes don't override methods from the base class.
Derived classes should extend its functionality rather than change it!
