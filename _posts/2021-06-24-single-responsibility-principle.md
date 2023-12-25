---
title: 'Single Responsibility Principle - SRP'
date: 2021-06-24
permalink: /posts/solid/single-responsibility-principle/
tags:
  coding
  principles
  SOLID
  OOP
---

In this post, we'll discuss the first principle from the SOLID group - the Single Responsibility Principle (SRP).

Here's its definition:
>A class should have only one reason to change (there should never be more than one motive for modifying a class).

This principle implies that a class should have a single responsibility, a primary objective it accomplishes.
Consequently, it shouldn't be directly tied to other functionalities, making it easier to comprehend and modifying it shouldn't risk damaging other parts of the program.

Below is the code of an example class called `PackageSender`.
Its methods simply print information to the screen - this simplification aims to aid in understanding the SRP principle.

```java
public class PackageSender {
    public void send() {
        System.out.println("Package sent!");
    }

    public void saveToFile() {
        System.out.println("Package saved to file!");
    }

    public void validatePackage() {
        System.out.println("Package correct!");
    }
}
```

As you can see, this class is responsible for several areas in our project:

- Sending
- Writing to a file
- Validating the package

Since it has multiple responsibilities, there is more than one reason for changing this class in the future.
This isn't a good practice because if we make changes in how files are saved in the application, we'll have to modify the class responsible for sending packages.
Something doesn't seem right, does it?
The same goes for validating the package.
While creating this type of application, validating the package before sending might be necessary, so it would be convenient to add a relevant method in this class while creating the package sending method.
It's a kind of laziness on the part of the programmer.
After all, it's faster to add a new method than to come up with a name for a new class and find its proper place in the project.
Let's return to our example.
Here's how we should divide the `PackageSender` class to comply with the Single Responsibility Principle.

```java
public class PackageSender {
    public void send() {
        System.out.println("Package sent!");
    }
}

public class PackageSaver {
    public void saveToFile() {
        System.out.println("Package saved to file!");
    }
}

public class PackageValidator {
    public void validate() {
        System.out.println("Package correct!");
    }
}
```

Now we have three different classes, each with its own purpose.
The `PackageSender` class contains methods (actually only one, but in a real project, there might be more) for sending packages, `PackageSaver` for saving them, and `PackageValidator` for verifying their correctness.
This is the convention we should stick to, and that's what today's principle is all about.

In simpler words:
If we create a class whose main task is to save data to a file, the methods within it should be associated solely with saving them to the file, not with refreshing the view or sending an email that the data has been saved.

But what if we wanted to verify the package's correctness before sending it?
No problem!
All we need to do is delegate this action to the appropriate class and method.
Good examples are also model classes whose main task is to store data.
We shouldn't implement methods in them that validate this data – this should be done by another class intended for this purpose.

It's worth noting that the **SRP** principle, like other **SOLID** principles, applies not only to classes but also to methods and interfaces.

A red flag should pop up when a method you're writing seems to fit a name with a conjunction like "and" in the middle.
This immediately shows that it doesn't have just one task to perform but is doing two or more things at once.

## Summary

I hope these simple examples and tips helped you understand the essence of the SRP principle.
By adhering to it, every class and method you create will have a clearly defined purpose.
The readability of your code will be higher, and the risk of encountering issues when modifying it in the future should be lower.
