---
title: Running Tests with pytest
teaching: 10
exercises: 0
questions:
- "How do I automate my tests?"
objectives:
- "Understand how to run a test suite using the pytest framework"
- "Understand how to read the output of a pytest test suite"
keypoints:
- "The `pytest` command collects and runs tests starting with `Test` or `test_`."
- "`.` means the test passed"
- "`F` means the test failed or erred"
- "`x` is a known failure"
- "`s` is a purposefully skipped test"
---

We created a suite of tests for our mean function, but it was annoying to run
them one at a time. It would be a lot better if there were some way to run them
all at once, just reporting which tests fail and which succeed.

Thankfully, that exists. Recalling our last test, write it in a file called `test_invariant_mass.py`, the command
`pytest` can be run on the terminal or command line from the directory containing the tests (note that you'll have to use `py.test` for older versions of the `pytest` package):

```bash
$ pytest
```

~~~
collected 3 items                                                                          

test_invariant_mass.py F.F                                                           [100%]

========================================= FAILURES =========================================
______________________________________ test_with_list ______________________________________

energy = array([1, 2, 3]), momentum = array([6, 7, 8])

    def invariant_mass(energy, momentum):
        try:
            if (energy < 0).any() or (momentum < 0).any():
                raise ValueError("Energy and momentum must be positive")
    
            else:
>               return cmath.sqrt(energy**2 - momentum**2)
E               TypeError: only length-1 arrays can be converted to Python scalars

sample.py:9: TypeError

During handling of the above exception, another exception occurred:

    def test_with_list():
>       assert invariant_mass(np.array([1, 2, 3]), np.array([6, 7, 8]))

test_invariant_mass.py:5: 
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 

energy = array([1, 2, 3]), momentum = array([6, 7, 8])

    def invariant_mass(energy, momentum):
        try:
            if (energy < 0).any() or (momentum < 0).any():
                raise ValueError("Energy and momentum must be positive")
    
            else:
                return cmath.sqrt(energy**2 - momentum**2)
    
        except TypeError:
>           raise TypeError("Please insert a number or list")
E           TypeError: Please insert a number or list

sample.py:12: TypeError
______________________________________ test_negative _______________________________________

    def test_negative():
>       assert invariant_mass(np.array([-3]), np.array([6]))

test_invariant_mass.py:11: 
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 

energy = array([-3]), momentum = array([6])

    def invariant_mass(energy, momentum):
        try:
            if (energy < 0).any() or (momentum < 0).any():
>               raise ValueError("Energy and momentum must be positive")
E               ValueError: Energy and momentum must be positive

sample.py:6: ValueError
================================= short test summary info ==================================
FAILED test_invariant_mass.py::test_with_list - TypeError: Please insert a number or list
FAILED test_invariant_mass.py::test_negative - ValueError: Energy and momentum must be po...
=============================== 2 failed, 1 passed in 0.72s ================================
~~~
{: .output}

In the above case, the pytest package 'sniffed-out' the tests in the
directory and ran them together to produce a report of the sum of the files and
functions matching the regular expression `[Tt]est[-_]*`.

The major benefit a testing framework provides is exactly that, a utility to find and run the
tests automatically. With pytest, this is the command-line tool called
`pytest`.  When `pytest` is run, it will search all directories below where it was called,
find all of the Python files in these directories whose names
start or end with `test`, import them, and run all of the functions and classes
whose names start with `test` or `Test`.
This automatic registration of test code saves tons of human time and allows us to
focus on what is important: writing more tests.

When you run `pytest`, it will print a dot (`.`) on the screen for every test
that passes,
an `F` for every test that fails or where there was an unexpected error.
In rarer situations you may also see an `s` indicating a
skipped tests (because the test is not applicable on your system) or a `x` for a known
failure (because the developers could not fix it promptly). After the dots, pytest
will print summary information.

> ## Show what tests are executed
>
> Pytest always show a list of passed arguments at the end of the test.
> Fixing the error of the negative output and leaving the error of the array will show the
> following passed test and failed test:
>
> ```bash
> $ pytest
> ```
>
> ~~~
> collected 3 items                                                                          
> 
> test_invariant_mass.py F..                                                           [100%]
>
> ...
> ================================= short test summary info ==================================
> FAILED test_invariant_mass.py::test_with_list - TypeError: Please insert a number or list
> =============================== 1 failed, 2 passed in 0.72s ================================
> ~~~
> {: .output}
>
{: .callout}

As we write more code, we would write more tests, and pytest would produce
more dots.  Each passing test is a small, satisfying reward for having written
quality scientific software. Now that you know how to write tests, let's go
into what can go wrong.
