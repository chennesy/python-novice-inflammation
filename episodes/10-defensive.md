---
title: Defensive Programming
teaching: 30
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objectives

- Explain what an assertion is.
- Add assertions that check the program's state is correct.
- Correctly add precondition and postcondition assertions to functions.
- Explain what test-driven development is, and use it when creating new functions.
- Explain why variables should be initialized using actual data values rather than arbitrary constants.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I make my programs more reliable?

::::::::::::::::::::::::::::::::::::::::::::::::::

Our previous lessons have introduced the basic tools of programming:
variables and lists,
file I/O,
loops,
conditionals,
and functions.
What they *haven't* done is show us how to tell
whether a program is getting the right answer,
and how to tell if it's *still* getting the right answer
as we make changes to it.

To achieve that,
we need to:

- Write programs that check their own operation.
- Write and run tests for widely-used functions.
- Make sure we know what "correct" actually means.

The good news is,
doing these things will speed up our programming,
not slow it down.
As in real carpentry --- the kind done with lumber --- the time saved
by measuring carefully before cutting a piece of wood
is much greater than the time that measuring takes.

## Assertions

The first step toward getting the right answers from our programs
is to assume that mistakes *will* happen
and to guard against them.
This is called [defensive programming](../learners/reference.md#defensive-programming),
and the most common way to do it is to add
[assertions](../learners/reference.md#assertion) to our code
so that it checks itself as it runs.
An assertion is simply a statement that something must be true at a certain point in a program.
When Python sees one,
it evaluates the assertion's condition.
If it's true,
Python does nothing,
but if it's false,
Python halts the program immediately
and prints the error message if one is provided.
For example,
this piece of code halts as soon as the loop encounters a value that isn't positive:

```python
numbers = [1, 0, -1]
total = 0.0
for num in numbers:
    assert num > 0.0, 'Data should only contain positive values'
    total += num
print('total is:', total)
```

```error
---------------------------------------------------------------------------
AssertionError                            Traceback (most recent call last)
Cell In[18], line 4
      2 total = 0.0
      3 for num in numbers:
----> 4     assert num > 0.0, 'Data should only contain positive values'
      5     total += num
      6 print('total is:', total)

AssertionError: Data should only contain positive values
```

Programs like the Firefox browser are full of assertions:
10-20% of the code they contain
are there to check that the other 80–90% are working correctly.
Broadly speaking,
assertions fall into three categories:

- A [precondition](../learners/reference.md#precondition)
  is something that must be true at the start of a function in order for it to work correctly.

- A [postcondition](../learners/reference.md#postcondition)
  is something that the function guarantees is true when it finishes.

- An [invariant](../learners/reference.md#invariant)
  is something that is always true at a particular point inside a piece of code.


Precondition:

```python
def visualize(filename):
    assert filename.endswith('.csv'), "The file must be a .csv file"

```

```python
visualize("Untitled.ipynb")
```

```error
---------------------------------------------------------------------------
AssertionError                            Traceback (most recent call last)
Cell In[20], line 1
----> 1 visualize("Untitled.ipynb")

Cell In[19], line 2, in visualize(filename)
      1 def visualize(filename):
----> 2     assert filename.endswith('.csv'), "The file must be a .csv file"
      3     data = numpy.loadtxt(fname=filename, delimiter=',')
      5     fig = plt.figure(figsize=(10.0, 3.0))

AssertionError: The file must be a .csv file
```

```python
def grade_on_curve(scores):
    max_score = max(scores)
    curved_scores = []
    for score in scores:
        assert score <= 100, "all scores must be 100 or less"
        curved_scores.append((score / max_score) * 100)
    return curved_scores

# Example usage
test_scores = [90, 75, 80, 89, 106]
curved_scores = grade_on_curve(test_scores)
print(curved_scores) 
```

```error
---------------------------------------------------------------------------
AssertionError                            Traceback (most recent call last)
Cell In[58], line 11
      9 # Example usage
     10 test_scores = [90, 75, 80, 89, 106]
---> 11 curved_scores = grade_on_curve(test_scores)
     12 print(curved_scores) 

Cell In[58], line 5, in grade_on_curve(scores)
      3 curved_scores = []
      4 for score in scores:
----> 5     assert score <= 100, "all scores must be 100 or less"
      6     curved_scores.append((score / max_score) * 100)
      7 return curved_scores

AssertionError: all scores must be 100 or less

```

Postcondition

```python
def grade_on_curve(scores):
    max_score = max(scores)
    curved_scores = []
    for score in scores:
        assert score <= 100, "all scores must be 100 or less"
        curved_scores.append((score / max_score) * 100)
        assert 0 <= (score / max_score) * 100 <= 100, "curved scores must be between 0 and 100"
    return curved_scores

# Example usage
test_scores = [99, 97, 98, 98, -10]
curved_scores = grade_on_curve(test_scores)
print(curved_scores) 
```

```output
---------------------------------------------------------------------------
AssertionError                            Traceback (most recent call last)
Cell In[57], line 12
     10 # Example usage
     11 test_scores = [99, 97, 98, 98, -10]
---> 12 curved_scores = grade_on_curve(test_scores)
     13 print(curved_scores) 

Cell In[57], line 7, in grade_on_curve(scores)
      5     assert score <= 100, "all scores must be 100 or less"
      6     curved_scores.append((score / max_score) * 100)
----> 7     assert 0 <= (score / max_score) * 100 <= 100, "curved scores must be between 0 and 100"
      8 return curved_scores

AssertionError: curved scores must be between 0 and 100
```



:::::::::::::::::::::::::::::::::::::::  challenge

## Pre- and Post-Conditions

Suppose you are writing a function called `average` that calculates
the average of the numbers in a NumPy array.
What pre-conditions and post-conditions would you write for it?
Compare your answer to your neighbor's:
can you think of a function that will pass your tests but not his/hers or vice versa?

:::::::::::::::  solution

## Solution

```python
# a possible pre-condition:
assert len(input_array) > 0, 'Array length must be non-zero'
# a possible post-condition:
assert numpy.amin(input_array) <= average <= numpy.amax(input_array),
'Average should be between min and max of input values (inclusive)'
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Testing Assertions

Given a sequence of a number of cars, the function `get_total_cars` returns
the total number of cars.

```python
get_total_cars([1, 2, 3, 4])
```

```output
10
```

```python
get_total_cars(['a', 'b', 'c'])
```

```output
ValueError: invalid literal for int() with base 10: 'a'
```

Explain in words what the assertions in this function check,
and for each one,
give an example of input that will make that assertion fail.

```python
def get_total_cars(values):
    assert len(values) > 0
    for element in values:
        assert int(element)
    values = [int(element) for element in values]
    total = sum(values)
    assert total > 0
    return total
```

:::::::::::::::  solution

## Solution

- The first assertion checks that the input sequence `values` is not empty.
  An empty sequence such as `[]` will make it fail.
- The second assertion checks that each value in the list can be turned into an integer.
  Input such as `[1, 2, 'c', 3]` will make it fail.
- The third assertion checks that the total of the list is greater than 0.
  Input such as `[-10, 2, 3]` will make it fail.
  
  

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- Program defensively, i.e., assume that errors are going to arise, and write code to detect them when they do.
- Put assertions in programs to check their state as they run, and to help readers understand how those programs are supposed to work.
- Use preconditions to check that the inputs to a function are safe to use.
- Use postconditions to check that the output from a function is safe to use.
- Write tests before writing code in order to help determine exactly what that code is supposed to do.

::::::::::::::::::::::::::::::::::::::::::::::::::


