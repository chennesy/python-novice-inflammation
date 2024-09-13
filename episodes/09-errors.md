---
title: Errors and Exceptions
teaching: 30
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- To be able to read a traceback, and determine where the error took place and what type it is.
- To be able to describe the types of situations in which syntax errors, indentation errors, name errors, index errors, and missing file errors occur.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How does Python report errors?
- How can I handle errors in Python programs?

::::::::::::::::::::::::::::::::::::::::::::::::::

Every programmer encounters errors,
both those who are just beginning,
and those who have been programming for years.
Encountering errors and exceptions can be very frustrating at times,
and can make coding feel like a hopeless endeavour.
However,
understanding what the different types of errors are
and when you are likely to encounter them can help a lot.
Once you know *why* you get certain types of errors,
they become much easier to fix.

Errors in Python have a very specific form,
called a [traceback](../learners/reference.md#traceback).
Let's examine one:

```python
# This code has an intentional error. You can type it directly or
# use it for reference to understand the error message below.

def add_list(input_list):
    print(input_list[4])
    return sum(input_list)
add_list([5,12, 99])    
```

```error
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
Cell In[17], line 4
      2     print(input_list[4])
      3     return sum(input_list)
----> 4 add_list([5,12, 99])       

Cell In[17], line 2, in add_list(input_list)
      1 def add_list(input_list):
----> 2     print(input_list[4])
      3     return sum(input_list)

IndexError: list index out of range

```

- Tracebacks
- Look at documentation
- Google the error


## Syntax Errors

When you forget a colon at the end of a line,
accidentally add one space too many when indenting under an `if` statement,
or forget a parenthesis,
you will encounter a [syntax error](../learners/reference.md#syntax-error).
This means that Python couldn't figure out how to read your program.
This is similar to forgetting punctuation in English:
for example,
this text is difficult to read there is no punctuation there is also no capitalization
why is this hard because you have to figure out where each sentence ends
you also have to figure out where each sentence begins
to some extent it might be ambiguous if there should be a sentence break or not

People can typically figure out what is meant by text with no punctuation,
but people are much smarter than computers.
If Python doesn't know how to read the program,
it will give up and inform you with an error.
For example:

```python
def some_function()
    msg = 'hello, world!'
    print(msg)
     return msg
```

```error
  File "<ipython-input-3-6bb841ea1423>", line 1
    def some_function()
                       ^
SyntaxError: invalid syntax
```

Here, Python tells us that there is a `SyntaxError` on line 1,
and even puts a little arrow in the place where there is an issue.
In this case the problem is that the function definition is missing a colon at the end.

Actually, the function above has *two* issues with syntax.
If we fix the problem with the colon,
we see that there is *also* an `IndentationError`,
which means that the lines in the function definition do not all have the same indentation:

```python
def some_function():
    msg = 'hello, world!'
    print(msg)
     return msg
```

```error
  File "<ipython-input-4-ae290e7659cb>", line 4
    return msg
    ^
IndentationError: unexpected indent
```

Both `SyntaxError` and `IndentationError` indicate a problem with the syntax of your program,
but an `IndentationError` is more specific:
it *always* means that there is a problem with how your code is indented.

:::::::::::::::::::::::::::::::::::::::::  callout

## Tabs and Spaces

Some indentation errors are harder to spot than others.
In particular, mixing spaces and tabs can be difficult to spot
because they are both [whitespace](../learners/reference.md#whitespace).
In the example below, the first two lines in the body of the function
`some_function` are indented with tabs, while the third line — with spaces.
If you're working in a Jupyter notebook, be sure to copy and paste this example
rather than trying to type it in manually because Jupyter automatically replaces
tabs with spaces.

```python
def some_function():
	msg = 'hello, world!'
	print(msg)
        return msg
```

Visually it is impossible to spot the error.
Fortunately, Python does not allow you to mix tabs and spaces.

```error
  File "<ipython-input-5-653b36fbcd41>", line 4
    return msg
              ^
TabError: inconsistent use of tabs and spaces in indentation
```

::::::::::::::::::::::::::::::::::::::::::::::::::

## Variable Name Errors

Another very common type of error is called a `NameError`,
and occurs when you try to use a variable that does not exist.
For example:

```python
print(a)
```

```error
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-7-9d7b17ad5387> in <module>()
----> 1 print(a)

NameError: name 'a' is not defined
```

Variable name errors come with some of the most informative error messages,
which are usually of the form "name 'the\_variable\_name' is not defined".

Why does this error message occur?
That's a harder question to answer,
because it depends on what your code is supposed to do.
However,
there are a few very common reasons why you might have an undefined variable.
The first is that you meant to use a
[string](../learners/reference.md#string), but forgot to put quotes around it:

```python
print(hello)
```

```error
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-8-9553ee03b645> in <module>()
----> 1 print(hello)

NameError: name 'hello' is not defined
```

The second reason is that you might be trying to use a variable that does not yet exist.
In the following example,
`count` should have been defined (e.g., with `count = 0`) before the for loop:

```python
for number in range(10):
    count = count + number
print('The count is:', count)
```

```error
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-9-dd6a12d7ca5c> in <module>()
      1 for number in range(10):
----> 2     count = count + number
      3 print('The count is:', count)

NameError: name 'count' is not defined
```

## File Errors

The last type of error we'll cover today
are those associated with reading and writing files: `FileNotFoundError`.
If you try to read a file that does not exist,
you will receive a `FileNotFoundError` telling you so.
If you attempt to write to a file that was opened read-only, Python 3
returns an `UnsupportedOperationError`.
More generally, problems with input and output manifest as
`OSError`s, which may show up as a more specific subclass; you can see
[the list in the Python docs](https://docs.python.org/3/library/exceptions.html#os-exceptions).
They all have a unique UNIX `errno`, which is you can see in the error message.

```python
file_handle = open('myfile.txt', 'r')
```

```error
---------------------------------------------------------------------------
FileNotFoundError                         Traceback (most recent call last)
<ipython-input-14-f6e1ac4aee96> in <module>()
----> 1 file_handle = open('myfile.txt', 'r')

FileNotFoundError: [Errno 2] No such file or directory: 'myfile.txt'
```

One reason for receiving this error is that you specified an incorrect path to the file.
For example,
if I am currently in a folder called `myproject`,
and I have a file in `myproject/writing/myfile.txt`,
but I try to open `myfile.txt`,
this will fail.
The correct path would be `writing/myfile.txt`.
It is also possible that the file name or its path contains a typo.


:::::::::::::::::::::::::::::::::::::::: keypoints

- Tracebacks can look intimidating, but they give us a lot of useful information about what went wrong in our program, including where the error occurred and what type of error it was.
- An error having to do with the 'grammar' or syntax of the program is called a `SyntaxError`. If the issue has to do with how the code is indented, then it will be called an `IndentationError`.
- A `NameError` will occur when trying to use a variable that does not exist. Possible causes are that a variable definition is missing, a variable reference differs from its definition in spelling or capitalization, or the code contains a string that is missing quotes around it.
- Containers like lists and strings will generate errors if you try to access items in them that do not exist. This type of error is called an `IndexError`.
- Trying to read a file that does not exist will give you an `FileNotFoundError`. Trying to read a file that is open for writing, or writing to a file that is open for reading, will give you an `IOError`.

::::::::::::::::::::::::::::::::::::::::::::::::::


