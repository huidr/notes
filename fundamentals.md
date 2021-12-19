
# Fundamentals

```python
  # multiple assignment
  a, b = 0, 1 
  ```
  
In multiple assignment, the expressions on the right-hand side are all evaluated ﬁrst before any of the assignments take place. The right-hand side expressions are evaluated from the left to the right.

## Arithmetic

In interactive mode, the last printed expression is assigned to the variable _

```python
  a + b; 
  a - b;
  a * b;
  a / b; # returns a float
  a // b; # floor division
  a % b;
  a ** b; # a to the power b
  ```
  
## Strings and lists

```python
	# raw string
  print(r'\usr\bin\name') # characters prefaced by \ will NOT be interpreted as special characters
  
  # f-strings, formatstrings
  x = 5
  print(f'this is {x*x}') # f-string: {x*x} becomes 25
  print('this is {}'.format(x*x)) # formatstrings
  ```
  
Operations on strings, lists, etc.
  
```python
  # concatenation
  name = 'Computer' + 'programming'
  name = 'Computer' 'programming' # useful for long strings, works only for string literals
  
  # slicing [start:stop:step]
  subname = name[2:8] # step = 1 (by default)
  subname = name[-1] # the last entry
  subname = name[-1::-1] # reverse string
  ```

List methods. Some of these may not work for strings because strings are immutable.

```python
  lst.append()
  ```
  
String literals can span multiple lines. One way is using triple-quotes: """...""" or '''...'''. End of lines are
automatically included in the string, but it’s possible to prevent this by adding a \ at the end of the line.
 
### Basics

```python
  sys.argv // a list that stores script name and additional arguments
  sys.argv[0] // script name
  sys.argv[1] // first argument, if it exists
  ```
  
## Control Flow
 
```if```, ```elif```, ```else``` &mdash; elif and else are optional, elif can be one or more.
 
```python
  if (condition):
  	do_this
  elif (condition):
    do_this
  else:
    do_this
  ```

```for``` &mdash; iterates over an iterable (like list, string, dictionary, etc). Code that modiﬁes a collection while iterating over that same collection can be tricky to get right. Instead, it is usually more straight-forward to loop over a copy of the collection or to create a new collection.

```python
  # iterate over a copy
  for user, status in users.copy().items():
    if status == 'inactive':
      del users[user]
      
  # create a new collection
  active_users = {}
  for user, status in users.items():
    if status == 'active':
      active_users[user] = status
  ```
  
```range()``` &mdash; accepts arguments as (start:stop:step), generates arithmetic progressions.

```python
  # handy in looping, default values: start = 0, step = 1
  for i in range(10):
    do_this
    
  # turn it into a list
  nums = list(range(40, 20, -2))
  
  # sum it up because it is an iterable
  sum(range(10))
  ```

```break```, ```continue```, else-in-loop (for-else, while-else)

```python
  for i in range(10):
    for j in range(5):
      if (condition):
        break # terminate the innermost loop containing this statement
      if (condition):
        continue # skip to the next iteration of the innermost loop containing this statement
    else:      
      do_this # execute this if the loop was not terminated by break
  ```

```pass``` &mdash; handy when statement is required syntactically but the program requires no action.

```python
  # commonly used for creating minimal classes
  class EmptyClass:
    pass
  
  # also used as a place-holder
  def func(*args):
    pass
  ```

## Functions

```python
  def myfunction(*args):
    ''' docstring ''' # can be fetched by myfunction.__doc__
    do_this
  ```
    
1. Global variables and variables of enclosing functions cannot be directly assigned a value within a function (unless, for global variables, named in a global statement, or, for variables of enclosing functions, named in a nonlocal statement), although they may be referenced.
2. Functions always return a value. It returns ```None``` if there is no ```return``` statement.

### Default arguments

```python
  def myfunction(name, type='language', id=0):
    do_this
  
  # different ways to call the above function
  myfunction('Python')
  myfunction('van Rossum', 'programmer')
  myfunction('Aventador', 'car', 10)
  ```
The default value is evaluated only once even when the function is called multiple times. This makes a diﬀerence when the default is a mutable object such as a list, dictionary, etc. See the following.

```python
  def f(a, L=[]):
    L.append(a)
    return L
  
  print(f(1)) # [1]
  print(f(2)) # [1, 2]
  print(f(3)) # [1, 2, 3]
  ```


### Keyword arguments

Keyword arguments must follow positional arguments (if any) in function call arguments list.

```python
  # the above function can be called as
  myfunction(type='programmer', id=24, name='Knuth')
  myfunction('van Rossum', id=10, type='programmer')
  ```
  
A parameter of the form ```*args``` receives a tuple while ```**kwargs``` receives a dictionary. Note that if both exist, ```**kwargs``` should follow ```*args```. Also, keywords become the keys of the dictionary.

```python
  def insert_data(name, *marks, **grades):
    print(name)
    print(marks)
    print(grades)
    
  insert_data('John',
	83, 77, 80,
 	algorithms='A', topology='B', electrodynamics='A')
  
  # this becomes
  # name = 'John'
  # marks = (83, 77, 80)
  # grades = {'algorithms':'A', 'topology':'B', 'electrodynamics':'A'}
  ```

### Parameter list

One can specify only positional arguments, standard (either positional or keyword) and only keyword argumements in the parameter list. The general form of the parameter list look something like this:

```
  def f(pos_only, /, pos_or_kw, *, kw_only)
  ```
  
pos_only arguments cannot receive keyword arguments and so on. If there is no ```/``` and ```*```, the arguments are standard (either positional or keyword).

```python
  def get_info(name, /, age, *, weight):
    print(name, age, height, sep=' ')
  
  # possible function calls
  get_info('Tom', 24, weight=76)
  get_info('Tom', age=24, weight=76)

  # some ways to write parameters list
  def get_info(name, /) # receive pos_only arguments
  def get_info(*, weight) # receive kw_only arguments
  ```

Dictionaries can deliver keyword arguments with the ```**```-operator.

```python
  def f(name, weight=50, beverage='beer'):
    print(f'{name} weighs {weight} kilos and drinks {beverage}.')
  
  d = {'name':'Robert', 'weight':58, 'beverage':'vodka'}
  
  # then call the function this way
  f(**d)
  ```
### Lambdas

Lambda functions can take any number of arguments but can only have one expression.

```python
  f = lambda x, y : x ** y
  
  print(f(2,4)) # 2 to the power 4
  ```
Lambdas are better used inside function definitions when an anonymous function is required for a short period of time.

## Data Structures

### Lists

The following are some common list methods.

```python
  list.append(x)
  list.extend(iterable)
  list.insert(index, x)
  list.remove(x) # removes first occurrence of x, returns ValueError if not found
  list.pop([index]) # returns the popped off item
  list.clear()
  list.index(x [, start[, stop]]) # finds the index of x searching in list[start:stop]
  list.count(x)
  list.sort(*, key=None, reverse=False)
  list.reverse()
  list.copy() # returns a copy of the list
  ```

Lists can be used as **stacks** &mdash; use ```list.append()``` and ```list.pop()``` to add/remove items at the top of the stack. Although lists can be used as **queues** as well, it is not recommended as performing inserts/pops from the beginning of a list is slow (as it involves shifting of all other elements by one) &mdash for this purpose, use ```collections.deque``` which was designed to have fast appends/pops from both ends.

```python
  from collections import deque
  queue = deque(['Newton', 'Maxwell', 'Einstein'])
  queue.append('Dirac')
  queue.popleft()
  
  # can be used as stack as well
  queue.pop()
  ```

List comprehensions provide a concise way to create lists and other data structures.

```python
  pairs = [(x, y) for x in [2, 3, 7] for y in [1, 3, 4] if x != y]
  # [(2, 1), (2, 3), (2, 4), (3, 1), (3, 4), (7, 1), (7, 3), (7, 4)]
  
  a = ['  time', 'money  ', ' skill ']
  b = [x.strip() for x in old_names]
  # b = ['time', 'money', 'skill']
  
  vec = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
  flattened_list = [item for elem in vec item in elem]
  # flattened_list = [1, 2, 3, 4, 5, 6, 7, 8, 9]
  ```
  
List comprehensions can be nested.

```python
  matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
  transpose = [[row[i] for row in matrix] for i in range(3)]
  ```
  
```del``` can be used to remove items from a list given its index instead of its value.

```python
  a = ['Tom', 'Huck', 'Mark']
  del a[2]
  del a[1:] # use slicing
  del a # delete entire variables
  ```
  
### Tuples

Tuples are like lists but immutatable.

```python
  # tuple packing
  t = 2, 4, 6
  
  # sequence unpacking
  a, b, c = t
  
  # nesting
  u = t, ('Python', 'C++')
  
  # can contain mutable objects
  s = ([1, 2], [3, 4])
  
  # empty tuple
  s = ()
  
  # one-element tuple
  s = 1,
  ```

### Sets

Sets are unordered collections and have no duplicate elements. One can perform set operations like union, intersection, etc.

```python
  # initialization
  vowels = set('aeiou')
  letters = {'a', 'b', 'c', 'l', 'm'}
  
  # membership testing
  if 'e' in vowels:
    print("Yes")
 
  # set union
  u = vowels | letters
  
  # set intersection
  u = vowels & letters
  
  # set difference
  u = vowels - letters
  
  # symmetric difference
  u = vowels ^ letters
  
  # set comprehensions
  a = {x for x in 'python' if x not in 'javascript'}
  ```
  
### Dictionaries

A dictionary is just a set of *key: value* pairs where *value* can be almost any Python object while *key* must be immutable (strings, numbers or tuples that do not contain mutable objects directly or indirectly).

```python
  # empty dictionary
  d = {}
  
  # add elements
  d['jack'] = 24
  
  # delete elements
  del d['jack']
  
  # get a list of its keys
  keys = list(d)
  
  # get a sorted list of its keys
  keys = sorted(d)
  
  # membership test
  'jack' in d
  'jack' not in d
  
  # dict comprehensions
  d = {x: x**2 for x in range(10)}
  
  # dict() constructor
  d = dict([(1, 1), (2, 4), (3, 9)])
  
  # if keys are simple strings
  d = dict(jack = 24, jill = 36)
  ```
  
### Looping techniques

When looping through dictionaries, use ```items()``` to retrieve key, value pairs.

```python
  d = dict(one = 1, two = 2, three = 3)
  for key, value in d.items():
    print(key, value)
  ```
  
When looping through sequences, use ```enumerate()``` to retrieve index, value pairs.

```python
  lst = ['time', 'money', 'skill']
  for index, value in enumerate(lst):
    print(index, value)
  ```
 
When looping over two or more sequences simultaneously, use ```zip()``` to pair the entries. If the sequences are of different length, it loops until the smallest sequence is looped over.

```python
  sqs = [1, 4, 9, 16, 25]
  cbs = [1, 8, 27, 64, 125]
  for s, c in zip(sqs, cbs):
    print(s, c)
  ```
To loop over a sequence in reverse, ﬁrst specify the sequence in a forward direction and then call ```reversed()```.

```python
  for i in reversed(range(10)):
    print(i)
  ```
  
To loop over a sequence in sorted order, use the ```sorted()``` function which returns a new sorted list while leaving the source unaltered.

```python
  seq = [x in x in range(10, 0, -1)]
  for i in sorted(seq):
    print(i)
  ```
  
To loop over unique elements of a sequence in sorted order, you ```sorted(set(sequence))```.

### Comparisons

Comparisons between sequences of the same type are lexicographical. Also, comparisons can be chained.

```python
  [1, 2, 3] < [1, 2, 4]
  'C' < 'Java' < 'Python'
  (1, 2, 3, 4) < (1, 3)
  (1, 1) < (1, 2) == (1.00, 2.00)
  ```

The conditions used in ```while``` and ```if``` statements can contain any operators, not just comparisons.

## Modules

### Importing

The following are different ways of importing modules.

```python
  import sys
  from math import e, pi
  from os import *
  import matplotlib.pyplot as plt
  from numpy import array as arr
  ```

Defined in ```sys``` module, the variable ```sys.path``` is a list of strings that determines the interpreter’s search path for modules. It can modified as follows:

```python
  sys.path.append('/home/user/python/modules')
  ```

Use ```dir()``` to find out all names which a module defines. Without arguments, it displaces the names you have defined currently.

```python
  dir(sys)
  
  # find out built-in functions and variables
  dir(builtins) 
  ```

### ```main()``` function

To add versatility to a python file, one can add the ```main()``` function so that it can be used both as a script or a module.

```python
  def main():
    do_this
  
  if __name__="__main__":
    main()
  ```

### Packages

A package is a collection of modules. The modules may be put in hierarchical directory system. To make the interpreter treat a directory of modules as if it were a package in itself, put ```__init__.py``` in the directory even if it is an empty file.

When using ```from package import item```, the item can be either a submodule (or subpackage) of the package or some other name defined in the package. Contrary to this, when using ```import item.subitem.subsubitem```, each item except the last must be a package and the last item must be a module or package but can't be a class or function or variable defined in the previous item.




