---
title: 'Dependency Inversion Principle – DIP'
date: 2021-09-25
permalink: /posts/solid/dependency-inversion-principle/
tags:
  coding
  principles
  SOLID
  OOP
---

```java
public class FileManager {
    FileRepository fileRepository = new FileRepository();

    public void Add(File file) {
        fileRepository.add(file);
    }
}

public class FileRepository {
    private List<File> files = new ArrayList<>();

    public void add(File file) {
        files.add(file);
    }
}

public class File {
    // Some cool stuff
}
```

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

## Summary

