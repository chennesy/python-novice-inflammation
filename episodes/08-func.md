---
title: Creating Functions
teaching: 30
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Define a function that takes parameters.
- Return a value from a function.
- Test and debug a function.
- Set default values for function parameters.
- Explain why we should divide programs into small, single-purpose functions.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I define new functions?
- What's the difference between defining and calling a function?
- What happens when I call a function?

::::::::::::::::::::::::::::::::::::::::::::::::::

At this point, we've seen that code can have Python make decisions about what it sees in our data. What if we want to convert some of our data, like taking a temperature in Fahrenheit and converting it to Celsius. We could write something like this for converting a single number

```python
fahrenheit_val = 99
celsius_val = ((fahrenheit_val - 32) * (5/9))
```

We could change the value of fahrenheit_val and re-run the code over, but we would be in trouble as soon as we had to do this more than a couple times.
Cutting and pasting it is going to make our code get very long and very repetitive,
very quickly.
We'd like a way to package our code so that it is easier to reuse,
a shorthand way of re-executing longer pieces of code. In Python we can use 'functions'.
Let's start by defining a function `fahr_to_celsius` that converts temperatures
from Fahrenheit to Celsius:

```python
    
def fahr_to_celsius(temp):
    return ((temp - 32) * (5/9))
```

![](fig/python-function.svg){alt='Labeled parts of a Python function definition'}

The function definition opens with the keyword `def` followed by the
name of the function (`fahr_to_celsius`) and a parenthesized list of parameter names (`temp`). The
[body](../learners/reference.md#body) of the function --- the
statements that are executed when it runs --- is indented below the
definition line.  The body concludes with a `return` keyword followed by the return value.

When we call the function,
the values we pass to it are assigned to those variables
so that we can use them inside the function.
Inside the function,
we use a [return statement](../learners/reference.md#return-statement) to send a result
back to whoever asked for it.

Let's try running our function.

```python
fahr_to_celsius(32)
```

This command should call our function, using "32" as the input and return the function value.

In fact, calling our own function is no different from calling any other function:

```python
print('freezing point of water:', fahr_to_celsius(32), 'C')
print('boiling point of water:', fahr_to_celsius(212), 'C')
```

```output
freezing point of water: 0.0 C
boiling point of water: 100.0 C
```

We've successfully called the function that we defined,
and we have access to the value that we returned.

## Composing Functions

Now that we've seen how to turn Fahrenheit into Celsius,
we can also write the function to turn Celsius into Kelvin:

```python
def celsius_to_kelvin(temp_c):
    return temp_c + 273.15

print('freezing point of water in Kelvin:', celsius_to_kelvin(0.))
```

```output
freezing point of water in Kelvin: 273.15
```

What about converting Fahrenheit to Kelvin?
We could write out the formula,
but we don't need to.
Instead,
we can [compose](../learners/reference.md#compose) the two functions we have already created:

```python
def fahr_to_kelvin(temp_f):
    temp_c = fahr_to_celsius(temp_f)
    temp_k = celsius_to_kelvin(temp_c)
    return temp_k

print('boiling point of water in Kelvin:', fahr_to_kelvin(212.0))
```

```output
boiling point of water in Kelvin: 373.15
```
Note that in this case we're creating new variables inside of the function, and choosing to return one of them. 

This is our first taste of how larger programs are built:
we define basic operations,
then combine them in ever-larger chunks to get the effect we want.
Real-life functions will usually be larger than the ones shown here --- typically half a dozen
to a few dozen lines --- but they shouldn't ever be much longer than that,
or the next person who reads it won't be able to understand what's going on.


## Variable Scope

In composing our temperature conversion functions, we created variables inside of those functions,
`temp`, `temp_c`, `temp_f`, and `temp_k`.
We refer to these variables as [local variables](../learners/reference.md#local-variable)
because they no longer exist once the function is done executing.
If we try to access their values outside of the function, we will encounter an error:

```python
print('Again, temperature in Kelvin was:', temp_k)
```

```error
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-1-eed2471d229b> in <module>
----> 1 print('Again, temperature in Kelvin was:', temp_k)

NameError: name 'temp_k' is not defined
```

If you want to reuse the temperature in Kelvin after you have calculated it with `fahr_to_kelvin`,
you can store the result of the function call in a variable:

```python
temp_kelvin = fahr_to_kelvin(212.0)
print('temperature in Kelvin was:', temp_kelvin)
```

```output
temperature in Kelvin was: 373.15
```

The variable `temp_kelvin`, being defined outside any function,
is said to be [global](../learners/reference.md#global-variable).

Inside a function, one can read the value of such global variables:

```python
def print_temperatures():
    print('temperature in Fahrenheit was:', temp_fahr)
    print('temperature in Kelvin was:', temp_kelvin)

temp_fahr = 212.0
temp_kelvin = fahr_to_kelvin(temp_fahr)

print_temperatures()
```

```output
temperature in Fahrenheit was: 212.0
temperature in Kelvin was: 373.15
```

You can also return more than one value in your function's return statement:
```python
def fahr_to_kelvin(temp_f):
    temp_c = fahr_to_celsius(temp_f)
    temp_k = celsius_to_kelvin(temp_c)
    return temp_c, temp_k

temp_c, temp_k = fahr_to_kelvin(212.0)
print(temp_c, temp_k)
```

```output
100.0 373.15
```
:::::::::::::::::::::::::::::::::::::::  challenge

## Combining Strings

"Adding" two strings produces their concatenation:
`'a' + 'b'` is `'ab'`.
Write a function called `fence` that takes two parameters called `original` and `wrapper`
and returns a new string that has the wrapper character at the beginning and end of the original.
A call to your function should look like this:

```python
print(fence('name', '*'))
```

```output
*name*
```

:::::::::::::::  solution

## Solution

```python
def fence(original, wrapper):
    return wrapper + original + wrapper
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Return versus print

Note that `return` and `print` are not interchangeable.
`print` is a Python function that *prints* data to the screen.
It enables us, *users*, see the data.
`return` statement, on the other hand, makes data visible to the program.
Let's have a look at the following function:

```python
def add(a, b):
    print(a + b)
```

**Question**: What will we see if we execute the following commands?

```python
A = add(7, 3)
print(A)
```

:::::::::::::::  solution

## Solution

Python will first execute the function `add` with `a = 7` and `b = 3`,
and, therefore, print `10`. However, because function `add` does not have a
line that starts with `return` (no `return` "statement"), it will, by default, return
nothing which, in Python world, is called `None`. Therefore, `A` will be assigned to `None`
and the last line (`print(A)`) will print `None`. As a result, we will see:

```output
10
None
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::
## Tidying up

Now that we know how to wrap bits of code up in functions,
we can make our inflammation analysis easier to read and easier to reuse.
First, let's make a `visualize` function that generates our plots:

```python
import matplotlib.pyplot as plt
import numpy

def visualize(filename):

    data = numpy.loadtxt(fname=filename, delimiter=',')

    fig = plt.figure(figsize=(10.0, 3.0))

    axes1 = fig.add_subplot(1, 3, 1)
    axes2 = fig.add_subplot(1, 3, 2)
    axes3 = fig.add_subplot(1, 3, 3)

    axes1.set_ylabel('average')
    axes1.plot(numpy.mean(data, axis=0))

    axes2.set_ylabel('max')
    axes2.plot(numpy.amax(data, axis=0))

    axes3.set_ylabel('min')
    axes3.plot(numpy.amin(data, axis=0))

    fig.tight_layout()
    plt.show()
```

and another function called `detect_problems` that checks for those systematics
we noticed:

```python
def detect_problems(filename):

    data = numpy.loadtxt(fname=filename, delimiter=',')

    if numpy.amax(data, axis=0)[0] == 0 and numpy.amax(data, axis=0)[20] == 20:
        print('Suspicious looking maxima!')
    elif numpy.sum(numpy.amin(data, axis=0)) == 0:
        print('Minima add up to zero!')
    else:
        print('Seems OK!')
```

Wait! Didn't we forget to specify what both of these functions should return? Well, we didn't.
In Python, functions are not required to include a `return` statement and can be used for
the sole purpose of grouping together pieces of code that conceptually do one thing. In such cases,
function names usually describe what they do, *e.g.* `visualize`, `detect_problems`.

Notice that rather than jumbling this code together in one giant `for` loop,
we can now read and reuse both ideas separately.
We can reproduce the previous analysis with a much simpler `for` loop:

```python
import glob
filenames = sorted(glob.glob('inflammation*.csv'))

for filename in filenames[:3]:
    print(filename)
    visualize(filename)
    detect_problems(filename)
```

By giving our functions human-readable names,
we can more easily read and understand what is happening in the `for` loop.
Even better, if at some later date we want to use either of those pieces of code again,
we can do so in a single line.

## Documenting

we should write some [documentation](../learners/reference.md#documentation) for our function
to remind ourselves later what it's for and how to use it.

The usual way to put documentation in software is
to add [comments](../learners/reference.md#comment) is usingthe #.


There's a better way, though.
If the first thing in a function is a string that isn't assigned to a variable,
that string is attached to the function as its documentation:

```python
def visualize(filename):
    """Generate three subplots with average, 
    max, and min values from inflammation 
    CSV files."""
    
```

This is better because we can now ask Python's built-in help system to show us
the documentation for the function:

```python
help(visualize)
```

A string like this is called a [docstring](../learners/reference.md#docstring).
We don't need to use triple quotes when we write one,
but if we do,
we can break the string across multiple lines:


## Defining Defaults

We have passed parameters to functions in two ways:
directly, as in `type(data)`,
and by name, as in `numpy.loadtxt(fname='something.csv', delimiter=',')`.
In fact,
we can pass the filename to `loadtxt` without the `fname=`:

```python
numpy.loadtxt('inflammation-01.csv', delimiter=',')
```

```output
array([[ 0.,  0.,  1., ...,  3.,  0.,  0.],
       [ 0.,  1.,  2., ...,  1.,  0.,  1.],
       [ 0.,  1.,  1., ...,  2.,  1.,  1.],
       ...,
       [ 0.,  1.,  1., ...,  1.,  1.,  1.],
       [ 0.,  0.,  0., ...,  0.,  2.,  0.],
       [ 0.,  0.,  1., ...,  1.,  1.,  0.]])
```

but we still need to say `delimiter=`:

```python
numpy.loadtxt('inflammation-01.csv', ',')
```

```error
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/Users/username/anaconda3/lib/python3.6/site-packages/numpy/lib/npyio.py", line 1041, in loa
dtxt
    dtype = np.dtype(dtype)
  File "/Users/username/anaconda3/lib/python3.6/site-packages/numpy/core/_internal.py", line 199, in
_commastring
    newitem = (dtype, eval(repeats))
  File "<string>", line 1
    ,
    ^
SyntaxError: unexpected EOF while parsing
```

To understand what's going on,
and make one our own functions easier to use,
let's re-define our `fahr_to_kelvin` function to use a default f value:

```python
def fahr_to_kelvin(temp_f=32):
    temp_c = fahr_to_celsius(temp_f)
    temp_k = celsius_to_kelvin(temp_c)
    return temp_c, temp_k
fahr_to_kelvin()
```

The key change is that the second parameter is now written `temp_f=32`
instead of just `temp_f`.

This is handy:
if we usually want a function to work one way,
but occasionally need it to do something else,
we can allow people to pass a parameter when they need to
but provide a default to make the normal case easier.
The example below shows how Python matches values to parameters:

```python
def display(a=1, b=2, c=3):
    print('a:', a, 'b:', b, 'c:', c)

print('no parameters:')
display()
print('one parameter:')
display(55)
print('two parameters:')
display(55, 66)
```

```output
no parameters:
a: 1 b: 2 c: 3
one parameter:
a: 55 b: 2 c: 3
two parameters:
a: 55 b: 66 c: 3
```

As this example shows,
parameters are matched up from left to right,
and any that haven't been given a value explicitly get their default value.
We can override this behavior by naming the value as we pass it in:

```python
print('only setting the value of c')
display(c=77)
```

```output
only setting the value of c
a: 1 b: 2 c: 77
```

With that in hand,
let's look at the help for `numpy.loadtxt`:

```python
help(numpy.loadtxt)
```

```output
Help on function loadtxt in module numpy.lib.npyio:

loadtxt(fname, dtype=<class 'float'>, comments='#', delimiter=None, converters=None, skiprows=0, use
cols=None, unpack=False, ndmin=0, encoding='bytes')
    Load data from a text file.

    Each row in the text file must have the same number of values.

    Parameters
    ----------
...
```

There's a lot of information here,
but the most important part is the first couple of lines:

```output
loadtxt(fname, dtype=<class 'float'>, comments='#', delimiter=None, converters=None, skiprows=0, use
cols=None, unpack=False, ndmin=0, encoding='bytes')
```

This tells us that `loadtxt` has one parameter called `fname` that doesn't have a default value,
and eight others that do.
If we call the function like this:

```python
numpy.loadtxt('inflammation-01.csv', ',')
```

then the filename is assigned to `fname` (which is what we want),
but the delimiter string `','` is assigned to `dtype` rather than `delimiter`,
because `dtype` is the second parameter in the list. However `','` isn't a known `dtype` so
our code produced an error message when we tried to run it.
When we call `loadtxt` we don't have to provide `fname=` for the filename because it's the
first item in the list, but if we want the `','` to be assigned to the variable `delimiter`,
we *do* have to provide `delimiter=` for the second parameter since `delimiter` is not
the second parameter in the list.

## Readable functions

Consider these two functions:

```python
def s(p):
    a = 0
    for v in p:
        a += v
    m = a / len(p)
    d = 0
    for v in p:
        d += (v - m) * (v - m)
    return numpy.sqrt(d / (len(p) - 1))

def std_dev(sample):
    sample_sum = 0
    for value in sample:
        sample_sum += value

    sample_mean = sample_sum / len(sample)

    sum_squared_devs = 0
    for value in sample:
        sum_squared_devs += (value - sample_mean) * (value - sample_mean)

    return numpy.sqrt(sum_squared_devs / (len(sample) - 1))
```

The functions `s` and `std_dev` are computationally equivalent (they
both calculate the sample standard deviation), but to a human reader,
they look very different. You probably found `std_dev` much easier to
read and understand than `s`.

As this example illustrates, both documentation and a programmer's
*coding style* combine to determine how easy it is for others to read
and understand the programmer's code. Choosing meaningful variable
names and using blank spaces to break the code into logical "chunks"
are helpful techniques for producing *readable code*. This is useful
not only for sharing code with others, but also for the original
programmer. If you need to revisit code that you wrote months ago and
haven't thought about since then, you will appreciate the value of
readable code!


:::::::::::::::::::::::::::::::::::::::  challenge

## Mixing Default and Non-Default Parameters

Given the following code:

```python
def numbers(one, two=2, three, four=4):
    n = str(one) + str(two) + str(three) + str(four)
    return n

print(numbers(1, three=3))
```

what do you expect will be printed?  What is actually printed?
What rule do you think Python is following?

1. `1234`
2. `one2three4`
3. `1239`
4. `SyntaxError`

Given that, what does the following piece of code display when run?

```python
def func(a, b=3, c=6):
    print('a: ', a, 'b: ', b, 'c:', c)

func(-1, 2)
```

1. `a: b: 3 c: 6`
2. `a: -1 b: 3 c: 6`
3. `a: -1 b: 2 c: 6`
4. `a: b: -1 c: 2`

:::::::::::::::  solution

## Solution

Attempting to define the `numbers` function results in `4. SyntaxError`.
The defined parameters `two` and `four` are given default values. Because
`one` and `three` are not given default values, they are required to be
included as arguments when the function is called and must be placed
before any parameters that have default values in the function definition.

The given call to `func` displays `a: -1 b: 2 c: 6`. -1 is assigned to
the first parameter `a`, 2 is assigned to the next parameter `b`, and `c` is
not passed a value, so it uses its default value 6.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Readable Code

Revise a function you wrote for one of the previous exercises to try to make
the code more readable. Then, collaborate with one of your neighbors
to critique each other's functions and discuss how your function implementations
could be further improved to make them more readable.


::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- Define a function using `def function_name(parameter)`.
- The body of a function must be indented.
- Call a function using `function_name(value)`.
- Numbers are stored as integers or floating-point numbers.
- Variables defined within a function can only be seen and used within the body of the function.
- Variables created outside of any function are called global variables.
- Within a function, we can access global variables.
- Variables created within a function override global variables if their names match.
- Use `help(thing)` to view help for something.
- Put docstrings in functions to provide help for that function.
- Specify default values for parameters when defining a function using `name=value` in the parameter list.
- Parameters can be passed by matching based on name, by position, or by omitting them (in which case the default value is used).
- Put code whose parameters change frequently in a function, then call it with different parameter values to customize its behavior.

::::::::::::::::::::::::::::::::::::::::::::::::::


