---
title: Assertions and Exceptions
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
- "Exceptions are effectively specialized runtime tests"
- "Exceptions can be caught and handled with a try-except block"
---

Pytest provides a rich set of built-in assertion statements that you can use to check conditions in your tests. These assertions make it easy to express and verify the expected outcomes of your code. The assert keyword in python has the
following behavior:

```python
>>> assert True == False
```

~~~
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  AssertionError
~~~
{: .output}

```python
>>> assert True == True
```

~~~
~~~
{: .output}

(Nothing happened because it "passed" but mind we are not testing something here really, we are just asserting that True is in fact == True)

Assertions halt code execution instantly if the comparison is false.
It does nothing at all if the comparison is true. These are therefore a very
good tool for guarding the function against foolish (e.g. human) input.

The advantage of assertions is their ease of use. They are rarely more than one
line of code. The disadvantage is that assertions halt execution
indiscriminately and the helpfulness of the resulting error message is usually
quite limited.

Another simple example of assertions:

```python
>>> # file name: test_sample.py
>>> import cmath
>>>
>>> def invariant_mass(energy, momentum):
>>>    return cmath.sqrt(energy**2 - momentum**2)
>>>
>>> def test_answer():
>>>   assert invariant_mass(3, 7) == 1
```

```bash
pytest
```

~~~
collected 1 item

test_sample.py F                                                         [100%]

=================================== FAILURES ===================================
_________________________________ test_answer __________________________________

    def test_answer():
>       assert invariant_mass(3, 7) == 1
E       assert 6.324555320336759j == 1
E        +  where 6.324555320336759j = invariant_mass(3, 7)

test_sample.py:7: AssertionError
=========================== short test summary info ============================
FAILED test_sample.py::test_answer - assert 6.324555320336759j == 1
============================== 1 failed in 0.13s ===============================
~~~
{: .output}


Also, input checking may require decending a rabbit hole of exceptional cases.
What happens when the input provided to the invariant_mass function is a string, rather
than a list of numbers? Or even, if it is a NaN result?

1. Open the terminal
2. Create the following function and name the file ``invariant_mass.py``:

```python
 import cmath

 def invariant_mass(energy, momentum):
    return cmath.sqrt(energy**2 - momentum**2)
```

3. In the function, insert an assertion that checks whether the input is actually a list.

> ## Hint
>
> Hint: Use the [isinstance function](https://docs.python.org/2/library/functions.html#isinstance).
{: .callout}

> ## Input as NaN using numpy
>
> What happens if we use numpy instead of cmath in the code? Why we get a `NaN` as an answer?
>
> > ## Solution
> >
> > In `numpy`, NaN is a float and assigning a NaN sets the imaginary part to zero. From the programmer's point
> > of view, it is a logical behavior but not mathematically (definition of not a number). This can be a
> > used as a marker for invalid data, but in this case it doesn't help.
> {: .solution}
{: .challenge}

> ## Testing Near Equality
>
> Assertions are also helpful for catching abnormal behaviors, such as those
> that arise with floating point arithmetic. Using the assert keyword, how could
> you test whether some value is almost the same as another value?
>
> - Use the `assert` keyword to check whether the number a is greater than 2.
> - Use the `assert` keyword to check that a is equal to 2 within an error of 0.003.
{: .callout}

```python
from mynum import a
# greater than 2 assertion here
# 0.003 assertion here
```

## Comparing Numbers within a Tolerance

Pytest as well as numpy has a built-in class for floating-point comparisons called ``pytest.approx``.

```python
>>> from pytest import approx
>>> 0.003 + 2 == approx(2.003)
True
```

# Exceptions

Exceptions are more sophisticated than assertions. They are the standard error
messaging system (unforseen circumstance) in most modern programming languages. Fundamentally, when an
error is encountered, an informative exception is 'thrown' or 'raised'. Mind that a failed assert,
also raises an exception `AssertionError`, so be very careful.

For example, instead of the assertion in the case before, an exception can be
used.

```python
 import cmath

 def invariant_mass(energy, momentum):
    if (energy**2 < momentum**2).any():
        raise Exception("Energy has to be greater than momentum")

    return cmath.sqrt(energy**2 - momentum**2)
```

Once an exception is raised, it will be passed upward in the program scope.
An exception be used to trigger additional error messages or an alternative
behavior rather than immediately halting code
execution, the exception can be 'caught' upstream with a try-except block.
When wrapped in a try-except block, the exception can be intercepted before it reaches
global scope and halts execution.

To add information or replace the message before it is passed upstream, the try-catch
block can be used to catch-and-reraise the exception:

```python
 import cmath

 def invariant_mass(energy, momentum):
    try:
        if (energy**2 < momentum**2).any():
            raise ValueError("Energy has to be greater than momentum")

    except TypeError:
        raise TypeError("Please insert a number or list")
```

If an alternative behavior is preferred, the exception can be disregarded and a
responsive behavior can be implemented like so:

```python
 import cmath

 def invariant_mass(energy, momentum):
    try:
        if (energy**2 < momentum**2).any():
            raise ValueError("Energy has to be greater than momentum")

        elif (energy < 0).any() or (momentum < 0).any():
            raise ValueError("Energy and momentum must be positive")

        else:
            return cmath.sqrt(energy**2 - momentum**2)

    except TypeError:
        raise TypeError("Please insert a number or list")
```

> ## What else could go wrong?
>
> 1. Think of some other type of exception that could be raised by the try
> block.
> 2. Guard against it by adding an except clause.
> 3. Use the mean function in three different ways, so that you cause each
> exceptional case.
{: .callout}
