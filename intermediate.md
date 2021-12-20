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

### Decorators


   
  
