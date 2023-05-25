---
title: Assertions and Exeptions
teaching: 10
exercises: 0
questions:
- "How can we compare observed and expected values?"
- "How do I handle unusual behavior while the code runs?"
objectives:
- "Assertions can halt execution if something unexpected happens."
- "Assertions are the building blocks of tests."
- "Learn when to use exceptions and what exceptions are available"
keypoints:
- "The `assert` keyword is used to set an assertion."
- "Assertions halt execution if the argument is false."
- "Assertions do nothing if the argument is true."
- "Exeptions are effectively specialized runtime tests"
- "Exceptions can be caught and handled with a try-except block"
---

Pytest provides a rich set of built-in assertion statements that you can use to check conditions in your tests. These assertions make it easy to express and verify the expected outcomes of your code. The assert keyword in python has the
following behavior:

~~~
>>> assert True == False
~~~
{: .python}
~~~

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  AssertionError
~~~
{: .output}

~~~
>>> assert True == True
~~~
{: .python}

~~~
~~~
{: .output}
(Nothing happened becasue it "passed" but mind we are not testing something here really, we are just asserting that True is in fact == True)

That is, assertions halt code execution instantly if the comparison is false.
It does nothing at all if the comparison is true. These are therefore a very
good tool for guarding the function against foolish (e.g. human) input.

The advantage of assertions is their ease of use. They are rarely more than one
line of code. The disadvantage is that assertions halt execution
indiscriminately and the helpfulness of the resulting error message is usually
quite limited.

Another (very) simple example of assertions:

~~~
>>> # file name: test_sample.py
>>> def mean(x):
>>>    return (67 + 5 + 7 + x)/5
>>>
>>> def test_answer():
>>>   assert mean(3) == 1
~~~
{: .python}

~~~
pytest
~~~
{: .bash}

~~~
collected 1 item                                                               

test_sample.py F                                                         [100%]

=================================== FAILURES ===================================
_________________________________ test_answer __________________________________

    def test_answer():
>   	assert mean(3) == 1
E    assert 16.4 == 1
E     +  where 16.4 = mean(3)

test_sample.py:5: AssertionError
=========================== short test summary info ============================
FAILED test_sample.py::test_answer - assert 16.4 == 1
============================== 1 failed in 0.01s ===============================
~~~
{: .output}


Also, input checking may require decending a rabbit hole of exceptional cases.
What happens when the input provided to the mean function is a string, rather
than a list of numbers?

1. Open the terminal
2. Create the following function and name the file ``test_sample.py``:

~~~
def mean(num_list):
  return sum(num_list)/len(num_list)
~~~
{: .python}

3. In the function, insert an assertion that checks whether the input is actually a list.

> ## Hint
>
> Hint: Use the [isinstance function](https://docs.python.org/2/library/functions.html#isinstance).
{: .callout}
> ## Testing Near Equality
>
> Assertions are also helpful for catching abnormal behaviors, such as those
> that arise with floating point arithmetic. Using the assert keyword, how could
> you test whether some value is almost the same as another value?
>
> - Use the `assert` keyword to check whether the number a is greater than 2.
> - Use the `assert` keyword to check that a is equal to 2 within an error of 0.003.
{: .callout}

~~~
from mynum import a
# greater than 2 assertion here
# 0.003 assertion here
~~~
{: .python}

## Comparing Numbers within a Tolerance

Pytest as well as numpy has a built-in class for floating-point comparisons called ``pytest.approx``.

~~~
>>> from pytest import approx
>>> 0.003 + 2 == approx(2.003)
>>> True
~~~
{: .python}

# Exeptions

Exceptions are more sophisticated than assertions. They are the standard error 
messaging system in most modern programming languages.  Fundamentally, when an 
error is encountered, an informative exception is 'thrown' or 'raised'.

For example, instead of the assertion in the case before, an exception can be
used.

~~~
def mean(num_list):
    if len(num_list) == 0:
      raise Exception("The algebraic mean of an empty list is undefined. "
                      "Please provide a list of numbers")
    else:
      return sum(num_list)/len(num_list)
~~~
{: .python}

Once an exception is raised, it will be passed upward in the program scope.
An exception be used to trigger additional error messages or an alternative
behavior. rather than immediately halting code
execution, the exception can be 'caught' upstream with a try-except block.
When wrapped in a try-except block, the exception can be intercepted before it reaches
global scope and halts execution.

To add information or replace the message before it is passed upstream, the try-catch
block can be used to catch-and-reraise the exception:

~~~
def mean(num_list):
    try:
        return sum(num_list)/len(num_list)
    except ZeroDivisionError as detail :
        msg = "The algebraic mean of an empty list is undefined. Please provide a list of numbers."
        raise ZeroDivisionError(detail.__str__() + "\n" +  msg)
~~~
{: .python}

Alternatively, the exception can simply be handled intelligently. If an
alternative behavior is preferred, the exception can be disregarded and a
responsive behavior can be implemented like so:

~~~
def mean(num_list):
    try:
        return sum(num_list)/len(num_list)
    except ZeroDivisionError :
        return 0
~~~
{: .python}

If a single function might raise more than one type of exception, each can be
caught and handled separately.

~~~
def mean(num_list):
    try:
        return sum(num_list)/len(num_list)
    except ZeroDivisionError :
        return 0
    except TypeError as detail :
        msg = "The algebraic mean of an non-numerical list is undefined.\
               Please provide a list of numbers."
        raise TypeError(detail.__str__() + "\n" +  msg)
~~~
{: .python}

> ## What else could go wrong?
>
> 1. Think of some other type of exception that could be raised by the try 
> block.
> 2. Guard against it by adding an except clause.
> 3. Use the mean function in three different ways, so that you cause each
> exceptional case.
{: .callout}
Exceptions have the advantage of being simple to include and powerfully helpful
to the user. However, not all behaviors can or should be found with runtime
exceptions. Most behaviors should be validated with unit tests.
