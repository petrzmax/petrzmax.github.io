---
title: 'Open/Closed Principle - OCP'
date: 2021-09-15
permalink: /posts/solid/open-closed-principle/
tags:
  coding
  principles
  SOLID
  OOP
---

This time, let's discuss another principle from the SOLID group – the Open/Closed principle (OCP).
Let's start with the definition:

>Classes (entities) should be open for extensions but closed for modifications.

In my opinion, this principle is quite straightforward to understand, even based solely on the definition.
A class open for extension will often use abstraction rather than concrete classes, while closed for modification means that once code is created, it shouldn't be modified.
Based on this, we can infer that code development should involve adding new elements without the need to modify existing ones.
This can be achieved consciously through the use of composition or inheritance.

I think you're getting the gist of it already.
However, let's delve into some code to see this in practice.
This time, I've come up with an example unrelated to cars!
(It's about games, and it's remarkably similar to the example discussed in the Strategy pattern post).
Let's begin by creating a class representing a player – `Player`.

```java
public class Player {
    private String currentWeapon;

    public void setWeapon(String weapon) {
        switch (weapon) {
            case "rock", "knife", "pistol" -> this.currentWeapon = weapon;
            default -> System.out.println("Weapon not implemented");
        }
    }

    public void attack() {
        switch (this.currentWeapon) {
            case "rock" -> System.out.println("Throw at the enemy");
            case "knife" -> System.out.println("Stab enemy");
            case "pistol" -> System.out.println("Shoot the enemy");
        }
    }
}
```

The `Player` class has a field containing information about the currently held weapon and two methods.
One is for setting the type of weapon, and the other is for attacking with it.
Of course, this code has unhandled cases that could potentially break a game based on such a class.
However, as usual, I'm trying not to complicate the code to focus on the principle we're discussing today.

So, we have implemented three types of weapons.
What would we have to do to add another one?
We'd need to modify not only the `Player` class but also its methods.
This is completely contradictory to the OCP principle and, frankly, inconvenient.
Let's modify our code to make it open for extensions and closed for modifications.
To achieve this, we'll move the common element of each type of weapon into the `Weapon` interface.

```java
public interface Weapon {
    void attack();
}
```

Each weapon has a method defining its attack pattern.
This is their common part, which is why our interface contains such a method.
Let's now create classes for each type of weapon.

```java
public class Rock implements Weapon {
    @Override
    public void attack() {
        System.out.println("Throw at the enemy");
    }
}

public class Knife implements Weapon {
    @Override
    public void attack() {
        System.out.println("Stab enemy");
    }
}

public class Pistol implements Weapon {
    @Override
    public void attack() {
        System.out.println("Shoot the enemy");
    }
}
```

As you can see, each type of weapon that was previously within the `Player` class is now its own separate class, and each of them implements the `Weapon` interface.
This is already starting to look much better.
All that's left for us is to modify the Player class.
To distinguish it, I've named it `BetterPlayer`.

```java
public class BetterPlayer {
    private Weapon currentWeapon;

    public void setWeapon(Weapon weapon) {
        this.currentWeapon = weapon;
    }

    public void attack() {
        this.currentWeapon.attack();
    }
}
```

This class is much simpler and shorter.
Instead of a String storing information about the current weapon, we have a field of type `Weapon`.
We can set it using the `setWeapon()` method by passing an instance of a class implementing our weapon interface.
The `attack()` method simply delegates the action to the weapon object.

The effect of this code is exactly the same as in the previous case.
So, what has changed?
Well, this class should now comply with the open/closed principle!

Consider what we'd have to do now to add several more types of weapons.
We'd only need to create a new class implementing the `Weapon` interface and then pass an object of that class to the `setWeapon()` method.

Certainly, we wouldn't modify any existing weapon class or the BetterPlayer class.
This means that the BetterPlayer class is open for extension and closed for modification, aligning with the Open/Closed principle!

Applying the open/closed principle is crucial when creating a public library.
If you were to change the behavior of methods already created, which other programs rely on from your library, you might cause numerous issues for those users.
After such an update, they'd be forced into unnecessary code updates or even repairs.

## Summary

By applying the OCP principle, the code we create will be:

1. Backward compatible
2. More flexible
3. Easier to maintain and extend
