---
title: 'Dependency Inversion Principle – DIP'
date: 2021-09-26
permalink: /posts/solid/dependency-inversion-principle/
tags:
  coding
  principles
  SOLID
  OOP
---

This is the last article in the series about the SOLID principles!
So, it's time to discuss the Dependency Inversion Principle (DIP).

Here is its definition:
>High-level modules should not depend on low-level modules. Both should depend on abstractions.

To understand this principle, we should start by explaining what high-level and low-level modules are, and the easiest way to do that is through an example.
Here's code that doesn't follow the DIP principle.

```java
public class FileManager {
    FileRepository fileRepository = new FileRepository();

    public void Add(File file) {
        this.fileRepository.add(file);
    }
}

public class FileRepository {
    private List<File> files = new ArrayList<>();

    public void add(File file) {
        this.files.add(file);
    }
}

public class File {
    // Some cool stuff
}
```

In this case, the high-level module is the `FileManager` class, while the low-level module is `FileRepository`.
Notice that the `FileManager` class contains an instance of `FileRepository`.
It uses an object of this class directly, which means that depends on it.
The above code is, therefore, not compliant with the dependency inversion principle!
Let's try to fix this by modifying the code in a way that dependencies between our modules stem from abstractions.

```java
public interface AddableFile {
    void add(File file);
}

public class FileManager {
    AddableFile fileRepository;

    public FileManager(AddableFile fileRepository) {
        this.fileRepository = fileRepository;
    }

    public void Add(File file) {
        this.fileRepository.add(file);
    }
}

public class FileRepository implements AddableFile {
    private List<File> files = new ArrayList<>();

    public void add(File file) {
        this.files.add(file);
    }
}

public class File {
    // Some cool stuff
}
```

As you can see, I've created the `AddableFile` interface here.
I'm not entirely happy with that name, it seems a bit unfortunate to me, but I couldn't come up with anything better.
Let's overlook that minor inconvenience, and focus on the fact that the `FileRepository` class now implements the created interface.
Besides that, its code remains exactly the same.
The FileManager underwent more significant changes.
It no longer creates an instance of the repository.
Instead, it has a field of type `AddableFile`, which is set through the constructor.
This allows us, during the creation of `FileManager`, to pass an object of any class that implements the `AddableFile` interface, making it easy to change the behavior of our manager.

This is how we've inverted the dependencies.
Now, `FileManager` no longer depends on the implementation of `FileRepository` but rather on the abstraction - `AddableFile`.
From now on, changes in low-level modules won't directly impact high-level modules.
The high-level modules don't need to know what actually happens or how the `add()` method works.
They only care that this method exists, guaranteed by implementing the appropriate interface.

## Summary

The correct application of the Dependency Inversion Principle should result in code where high and low-level modules are not strongly dependent on each other.
This outcome is achieved by relying on abstraction rather than implementation.
This approach allows us to easily use other classes with the appropriate interface, having different method implementations.
Consequently, the code becomes more flexible and open to extensions.

As always, it's important to approach this with common sense.
It's not always beneficial to use interfaces in every possible scenario.
There should be a balance and a practical consideration of when and where to apply these principles based on the specific needs and architecture of the project.
