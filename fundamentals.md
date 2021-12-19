
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
  
## Strings and lists.

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

### Parameters list

One can specify only positional arguments, standard (either positional or keyword) and only keyword argumements in the parameters list. The general form of the parameters list look something like this:

```
  def f(pos_only, /, pos_or_kw, *, kw_only)
  ```
  
pos_only arguments cannot receive keyword arguments and so on. If there is no ```/``` and ```*```, the arguments are standard (either positional or keyword).

```
  python
  def get_info(name, /, age, *, weight):
    print(name, age, height, sep=' ')
  
  # possible function calls
  get_info('Tom', 24, weight=76)
  get_info('Tom', age=24, weight=76)

	# some ways to write parameters list
	def get_info(name, /) # receive pos_only arguments
	def get_info(*, weight) # receive kw_only arguments
	```
