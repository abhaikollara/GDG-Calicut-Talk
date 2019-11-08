# Not an Introduction to Python

We'll probably cover
- `__slots__`
- The dunder methods
- Decorators
- Context Managers
- Mixins
- Concurrency
- Metaclasses
- Some tricks

## The dunder methods
- Think operator overloading if you've learned it in C (not the same thing though)
- `len(x)` = `__len__(x)`
- `x + y` = `x.__add__(y)` (similar for other math ops)
- `x[4]` = `x.__contains__(4)`
- `x == y` = `x.__eq__(y)`
- `x in seq` = `seq.__contains__(x)`

Lots more. Refer [Python data model docs](https://docs.python.org/3/reference/datamodel.html)

## `__slots__`
- Every Python object has a `__dict__` holding it's data members.
- But dicts are memory-expensive
- `__slots__` allow you to define the data members before hand
  - But you cannot add new data members dynamically

```python
class CardA:

    def __init__(self, value, suit):
        self.value = value
        self.suit = suit


class CardB:
    __slots__ = ('suit', 'value')

    def __init__(self, suit, value):
        self.value = value
        self.suit = suit
```

## Decorators
- Functions are first-class objects
  - Can be created at runtime
- Functions can return other functions

```python
def create_adder(addend):
  def func(x):
    return x + addend
  
  return func

add_3 = create_adder(3)

print(add_3(7))
```
```
>>> 10
```

- Functions can also take in functions as arguments

**Creating a logger**
```python
def log(func):
    def logged(*args, **kwargs):
        print("Executing", func.__qualname__)
        func(*args, **kwargs)

    return logged
```
```python
def hello(name):
    print("Hello", name)

logged_hello = log(hello)
logged_hello("Kozhikode")
```

The syntactic sugar that makes it all sweet
```python
@log
def hello(name):
    print("Hello", name)

print("Kozhikode")
```

## Context managers

```python
from contextlib import contextmanager

@contextmanager
def open_file(name):
    print("Opening file")
    f = open(name, 'w')
    yield f
    print("Closing file")
    f.close()
```
## Concurrency

## Metaclasses 🚨
- Classes are objects of type `type`

```python
class Pup():
    pass

pup = Pup()
```
```
>>> type(pup)
<class '__main__.Pup'>
>>> type(Pup)
<class 'type'>
```

- This allows us to access classes before they are even instantiated

```python

class GenericPuppy(type):

    def __new__(cls, name, bases, dct):
        obj = super().__new__(cls, name, bases, dct)
        assert hasattr(obj, "bark"), "Pup has no bark. Sad pup"
        return obj

class RetriverPuppy(metaclass=GenericPup):

    def __init__(self):
        pass
    
    ## Comment out this part to see the error
    def bark():
        print("Woof woof !")
```


## Quick Tricks

### The `lru_cache`

### Transposing a list of lists

###

## Credits/References
- [James Powell: So you want to be a Python expert? | PyData Seattle 2017](https://www.youtube.com/watch?v=sUmoMSU9_GQ)
- [Transforming Code into Beautiful, Idiomatic Python](https://www.youtube.com/watch?v=OSGv2VnC0go)
- [Python Tips](https://book.pythontips.com/)
- [Python Metaclasses](https://realpython.com/python-metaclasses/)
