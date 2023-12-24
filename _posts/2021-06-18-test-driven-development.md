---
title: 'Test Driven Development'
date: 2021-06-18
permalink: /posts/coding/test-driven-development/
tags:
  + coding
  + agile
  + methodology
---

Test Driven Development (TDD), a methodology in programming, which falls under the agile methodologies.

It stands out by crafting a functionality from its endpoint.
We start by creating tests for a function that hasn't been written yet.
Initially, the tests might not even compile because the elements they use might not exist yet.
It involves the repetitive execution of three steps:

1. Red - Creating a failing test for the functionality we want to write.
2. Green - Implementing the functionality to make the test pass.
3. Refactor - Refactoring the newly written code - both the functionality and the test.

If after step three, all tests pass, we go back to step one.
This is an iterative approach, meaning that while writing code, we repeat these steps multiple times until the completion of work on a particular functionality.

A crucial aspect is performing each step using the shortest possible code.
So, transitioning from the red to green stage might involve, for example, creating an empty method.

By using the TDD method, we obtain code which has tests prepared right from the start.
This enables us to continuously test our evolving functionality, helping to avoid errors caused by, for example, accidental breakage of existing code during further development.
Additionally, thanks to regular refactoring, the code remains clean and shouldn't require significant effort once a particular functionality is completed.

We can also use this method to fix bugs in existing code.
To do so, start by creating a test that reproduces the bug in our application.

Before we move on to presenting TDD in practice, it's worthwhile to familiarize yourself with a few concepts related to unit tests, especially if you haven't dealt with them before.

## Stub

Stubs are sample implementations of some code whose behavior we want to test.
They might return, for instance, appropriately prepared data necessary to test a selected functionality.
Stubs work well when testing simple methods.
However, for testing more complex methods with a larger number of test conditions, they might not be the best solution.
In such cases, it's better to use a Mock.

## Mock

Mocks are objects that simulate the behavior of real objects and real code.
They can be dynamically created during application runtime and offer much greater flexibility compared to stubs.
They also provide significantly more functionality.

To create a robust mock, external libraries like Mockito are commonly used.

## Spy

A Spy object is a kind of wrapper whose behavior we can precisely track and verify, much like with mock objects.
Additionally, if desired, we can also mock the behavior of selected methods within it.
Hence, we can say that a Spy object is partially a mock and partially a normal object.
That's why Spy objects are referred to as partial mocks.

They come in handy in situations where we want to leverage the real behavior of some methods in the object while mocking the behavior of others.
Spies are also useful when we want to verify method calls while retaining their actual behavior.

## TDD in practice

The examples were written in Java using JUnit 5.
I aimed to avoid complicating it with mocks and similar objects so that you could focus on the methodology itself.
We'll aim to create a simple class representing a user account in an online store.
Commonly in such services, before placing an order, the user will need to activate their account by entering a code sent to their email.
So, let's begin.
Following the TDD methodology, we'll first create a test that will yield a negative result.

```java
@Test
void accountShouldBeAbleToActivate() {
    //given
    Account newAccount = new Account();
}
```

As you can see, our test is very short.
It only contains the declaration of the `newAccount` object.
The `Account` class does not exist yet, so the code doesn't compile - it shows a red light.
We can move on to the next step.
Our task now is to make the above test compile and pass successfully.
To do that, we need to create the `Account` class.

```java
public class Account {
}
```

Just that?! - Yes, that's the idea.
Our class is completely empty, but it's enough for the test to compile.
We have a green light, so we can move on to the third step - refactoring.
Let's assume that the test name could be more descriptive.
We are in the right place to change it.

```java
@Test
void activatedAccountShouldHaveActiveFlagSet() {
    //given
    Account newAccount = new Account();
}
```

We've gone through all 3 points!
Is that the end?
Well, not yet.
Our `Account` class still doesn't truly exist.
So, we're moving on to the next iteration.
Let's make the test result negative again.

```java
@Test
void activatedAccountShouldHaveActiveFlagSet() {
    //given
    Account newAccount = new Account();

    //when
    newAccount.activate();
}
```

The `activate()` method doesn't exist.
We have a red light - let's move on to the next step.

```java
public class Account {
    public void activate() {
    }
}
```

Take note of this example.
In our test, we're simply calling the `activate()` method, we're not verifying its behavior.
Therefore, its implementation isn't necessary for the test to pass.
That's why at this stage, we're only declaring it.
This is a crucial rule when practicing TDD - Add only as much code as it's necessary to pass the test.
We have a green light, there's nothing to refactor, so we move on to the next iteration.

```java
@Test
void activatedAccountShouldHaveActiveFlagSet() {
    //given
    Account newAccount = new Account();

    //when
    newAccount.activate();

    //then
    assertTrue(newAccount.isActive());
}
```

Finally, we added an assertion.
Now, the test result depends on the value returned by `isActive()`.
Let's move on to the next step:

```java
public class Account {
    public void activate() {
    }

    public boolean isActive() {
        
    }
}
```

We've added the `isActive()` method, but the test result is still negative.
Let's implement it:

```java
public class Account {
    private boolean active;

    public void activate() {
    }

    public boolean isActive() {
        return this.active;
    }
}
```

We've added a flag - the field `active`, and made the `isActive()` method return its value.
However, the test still fails because the initial value of `active` is false.
Let's implement the method that activates the account.

```java
public class Account {
    private boolean active;

    public void activate() {
        this.active = true;
    }

    public boolean isActive() {
        return this.active;
    }
}
```

Now our test passes, there's nothing to refactor, and we have a functioning feature.
We've just navigated through creating functionality using the TDD methodology!
I hope you enjoy it as much as I do 🙂
If you're not familiar with or unsure about the "given, when, then" comments, don't worry, as they're not particularly significant in our example.
However, if you're interested in this topic, you might want to read up on BDD - Behavior Driven Development.

## Summary

Thanks to the Test Driven Development methodology:

- We write clean, well-documented code right from the start.
- We create tests during the coding phase, so we don't have to revisit minor functionalities later and try to remember how they functioned.
- We can quickly implement changes and new features without worrying about affecting other parts of the application.
If something is wrong, we immediately know that because the application shouldn't pass the tests after such changes.

This method has another significant advantage - it motivates and encourages to further work.
Why? Well, when working on a larger project, the effects of our work are often visible only after several hours or even days.
In such a working system, our minds rarely receive rewards and may become discouraged.
Our minds love rewards and need them regularly, such as satisfaction or tangible outcomes, to maintain energy for further work.

How does TDD fit into this?
With each iteration, we receive a reward in the form of a green light or rather, a transition from a negative - red test result to a positive - green one.
There are many more of these changes during coding work, delivered regularly.
It's a psychological aspect tied to the way the human mind functions, but trust me, it works.
Yet, it's best to experience it firsthand 😉
