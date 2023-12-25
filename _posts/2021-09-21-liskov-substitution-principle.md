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

```java
public class Duck {
    private void quack() {
        System.out.println("Quack! Quack!");
    };

    public void scare() {
        quack();
    }
}

public class WildDuck extends Duck{
    @Override
    public void scare() {
        System.out.println("The wild duck ran away!");
    }
}
```

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

```text
Quack! Quack!
Wild duck ran away!
```

```java
public class WildDuck extends Duck{
    public void runAway() {
        System.out.println("The wild duck ran away!");
    }
}
```

## Summary
