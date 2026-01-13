---
title: Introduction
teaching: 5
exercises: 0
questions:
- "Whats is Pytest?"
- "Why test?"
objectives:
- "Understand the place of testing in a scientific workflow."
- "Understand that testing has many forms."
keypoints:
- "Tests check whether the observed result, from running the code, is what was expected ahead of time."
- "Tests should ideally be written before the code they are testing is written, however some tests must be written after the code is written."
- "Assertions and exceptions are like alarm systems embedded in the software, guarding against exceptional bahavior."
- "Unit tests try to test the smallest pieces of code possible, usually functions and methods."
- "Integration tests make sure that code units work together properly."
- "Regression tests ensure that everything works the same today as it did yesterday."
---

# What is Pytest?

<center><img src="https://realpython.com/cdn-cgi/image/width=1920,format=auto/https://files.realpython.com/media/Intermediate-Advanced-PyTest-Features_Watermarked.43fb169e7121.jpg" alt="Pytest" style="width:576px;height:324px;"><p><a href="https://realpython.com/pytest-python-testing/">Via Real Python</a></p></center>

Testing the code is a crucial task that confirms that the code is working correctly and meets the quality standards for customers and, in this case, for analysis. Automated tests are important to confirm that code is working correctly. This is called **defensive programming** and the most common way to do it is to add alarms and tests into our code so that it checks itself.

At its core, Pytest follows the "convention over configuration" principle, which means it provides sensible defaults and encourages a standardized structure for tests. This allows you to focus on writing test logic rather than dealing with complex setup or boilerplate code.

# Why Test?

“Trying to improve the quality of software by doing more testing is like trying to lose weight by weighting yourself more often.” - Steve McConnell

- Testing won’t correct a buggy code
- Testing will tell you were the bugs are…
- … if (and only if) the test cases cover the scenarios that cause the bugs or occur.

Also, automated tests only test a narrow interpretation of quality software development. They do not help test that your software is useful and help solves a users’ problem.

There are many ways to test software, such as:

- Assertions
- Exceptions
- Unit Tests
- Regression Tests
- Integration Tests

*Exceptions and Assertions*: While writing code, `exceptions` and `assertions`
can be added to sound an alarm as runtime problems come up. These kinds of
tests, are embedded in the software iteself and handle, as their name implies,
exceptional cases rather than the norm.

*Unit Tests*: Unit tests investigate the behavior of units of code (such as
functions, classes, or data structures). By validating each software unit
across the valid range of its input and output parameters, tracking down
unexpected behavior that may appear when the units are combined is made vastly
simpler.

*Regression Tests*: Regression tests defend against new bugs, or regressions,
which might appear due to new software and updates.

*Integration Tests*: Integration tests check that various pieces of the
software work together as expected.

## 'pytest' vs 'unittest'?

Python has a built in framework for testing called ``unittest``. It is based on the Java x-unit style framework. Unittest requires developers to create classes derived from the TestCase module and then define the test cases as methods in the class. This means that there is a lot of boilerware code required to execute the tests, in contrast to Pytest, which is more condensed and easier to write.
