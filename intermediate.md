# Intermediate

## More on functions

### Closure

Nested functions can access variables of the enclosing scope. The criteria for a closure are:

1. Have a nested function.
2. The nested function references a variable defined in the enclosing function.
3. The enclosing function returns the nested function.

```python
def general_multiplier(n):
  def multiplier(x):
    return x*n
  return multiplier

times_2 = general_multiplier(2)
times_3 = general_multiplier(3)

times_2(10) # 20
times_3(10) # 30

del general_multiplier

# still works after the function has been removed
times_2(8) # 16
times_3(8) # 24
```

The value in the enclosing scope is remembered even when the variable goes out of scope or the function itself is removed from the current namespace.

When to use closure?

1. If there are few methods of a class, it might be a better idea to use a closure instead of that class.
2. To avoid the use of global variables and provide some form of data hiding.

```python
class A:
  def __init__(self):
    self.data = []
  
  # make its instances callable
  def __call__(self, a):
    self.data.append(a)
    _sum = sum(self.data)
    return _sum
  
a = A()

# call the instance
a(1) # 1
a(2) # 3
a(3) # 6
a(4) # 10

# implement the same class as closure
def f():
  data = [] # act like attributes
  
  def sum(a):
    data.append(a)
    _sum = sum(data)
    return _sum
  return sum

t = f()
t(1) # 1
t(2) # 3
t(3) # 6
t(4) # 10
```

When to not use closure?

1. When the number of attributes and methods grow larger, use a class and not a closure.

```python
# x, y, z are nonlocal variables of g
def f():
  x = 2
  y = 4
  z = 6
  def g(a, b, c):
    return x*a, y*b, z*c
  return g

s = f()

# check if s is a closure function
s.__closure__ # returns a tuple of cell objects

# the cell object has the attribute cell_contents which stores the closed value
# this will return the values of the nonlocal varibles which are 2, 4, 6
s.__closure__[0].cell_contents # 2
s.__closure__[1].cell_contents # 4
s.__closure__[2].cell_contents # 6
```

Use the ```nonlocal``` keyword to modify a variable in the outer function scope.

```python
def outer():
  x = 8
  def inner():
    nonlocal x # x must already be defined in the outer scope
    x = 2
  print("x =", x)
  inner()
  print("x =", x)

outer()
# prints
# x = 8
# x = 2
```

### Higher-order functions

Almost everything in Python is an object, i.e., it is an subclass or an instance of a subclass of the ```Object``` class. Hence, functions are also objects and they can be passed as arguments to other functions or returned from other functions.

```python
def f(text):
  return text.upper()
  
def g(text):
  return text.lower()

# function as a parameter
def h(func, text):
  s = func(text)
  print(s)
  
# function as arguments
h(f, 'Python') # PYTHON
h(g, 'JavaScript') # javascript
```

The following are also possible.

1. Assign the function to a variable.
2. Use ```del``` to delete functions.
3. Functions can be stored in lists, etc.

```python
def f(n):
  i = 2
  a = []
  def g(x):
    return x*i
  while (i<=n):
  
    # store functins in a list
    a.append(g)
    i += 1
    
  # return a list of functions
  return a

# assign the function to a variable
h = f

# h and f are two references to the function
# let's delete f and work with h
del f

# unpack the functions
x, y, z = h(4)

# all are multipliers of 5
x(5) # 25
y(7) # 35
z(9) # 45
```

### Decorators

Decorators are the most common use of higher-order functions. They take in functions (as arguments), add functionality (decorate) and return it.

```python
def decorate(f):
  def inner():
    print("Just got decorated.")
    f()
  return inner

def g():
  print("I am just a function")

# send g to decorate()
# h is also a closure function
h = decorate(g)

g()
# I am just a function

h()
# Just got decorated
# I am just a function
```

In the above, one could have written:

```python
g = decorate(g)
```

The function g just got upgraded (or decorated). This is exactly what a decorator is. Python has a special syntax for this.

```python
@decorate
def g():
  print('I am just a function')

# above code is equivalent to the following
def g():
  print('I am just a function')
g = decorate(g)
```

Parametrized functions can be decorated like the following:

```python
def smart_divider(f):
  def inner(a, b):
    if b == 0:
      print("Can't divide by zero.")
      return
    return f(a, b)
    
  return inner
  
@smart_divider
def divider(x, y):
  return x/y
  
divider(4, 2) # 2.0

divider(4, 0)
# Can't divide by zero.
```

Parameters of the nested inner() function inside the decorator is the same as the parameters of functions it decorates. A general decorator can be made using ```*args``` (which accepts tuple of positional arguments) and ```**kwargs``` (which accepts dictionary of keyword arguments).

```python
def decorate(f):
  def inner(*args, **kwargs):
    print('I can decorate any function')
    return f(*args, **kwargs)
    
  return inner
```

Decorators can also be chained. One can decorate functions with same or different decorators many times.

```python
def first_decorator(f):
  def inner(*args, **kwargs):
    print('I am first decorator')
    return f(*args, **kwargs)
    
  return inner
  
def second_decorator(f):
  def inner(*args, **kwargs):
    print('I am second decorator')
    return f(*args, **kwargs)
    
  return inner

@second_decorator
@first_decorator
def adder(*args):
  return sum(args)

adder(1, 2, 3)
# I am second decorator
# I am first decorator
# 6

# an equivalent way of decorating
adder = second_decorator(first_decorator(adder))
```





