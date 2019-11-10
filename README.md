# Not an Introduction to Python

## whomai

```json
{
  "name": "Abhai Kollara Dilip",
  "company": "Saama Technologies",
  "profession": "Research Engineer",
  "stuff_I_brag_about": "Contributed to Keras long ago"
}
```

## We'll probably discuss

- The dunder methods
- `__slots__`
- Enum
- Decorators
- Context Managers
- Mixins
- Metaclasses
- Some tricks

## The dunder methods
- Think operator overloading if you've learned it in C (not the same thing though)
- `len(x)` = `__len__(x)`
- `x + y` = `x.__add__(y)` (similar for other math ops)
- `x[4]` = `x.__getitem__(4)`
- `x == y` = `x.__eq__(y)`
- `x in seq` = `seq.__contains__(x)`

Lots more. Refer [Python data model docs](https://docs.python.org/3/reference/datamodel.html)

## Enum
- Use it when you want to restrict choices to a limited set of values

```python
from enum import Enum

class CardSuit(Enum):
  Hearts = 1
  Spades = 2
  Clubs = 3
  Diamonds = 4
```

## `__slots__`
- Every Python object has a `__dict__` holding it's data members.
  - Used for dynamic allocation of attributes
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
```
```
>>> hello("Kozhikode")
Executing hello
Hello Kozhikode
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

## Mixins
- Not a language feature, more of a pattern.

According to this [StackOverflow answer](https://stackoverflow.com/a/547714) mixins have to uses

1. You want to provide a lot of optional features for a class.
2. You want to use one particular feature in a lot of different classes.

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


## Quick Bites

### The `lru_cache`
- Store the results of recent function calls
- Can significantly speed up if same arguments are frequently passed to function

```python
from functools import lru_cache

@lru_cache(8) # Remove this to see the difference
def fib(n):
    if n < 2:
        return 1

    return fib(n-1) + fib(n-2)

print(fib(300))
```

### NamedTuples
Use `namedtuple` to make tuples self descriptive

```python
from collections import namedtuple

text = "Welcome to Kozhikode. Nice to meet you all."

def text_stats(text):
  Result = namedtuple("Result", "n_chars n_words n_whitespaces")
  n_chars = len(text)
  n_words = len(text.split())
  n_whitespaces = text.count(" ")
  
  return Result(n_chars, n_words, n_whitespaces)
```
```
>>> print(text_stats(text))
Result(n_chars=43, n_words=8, n_whitespaces=7)
```


## Credits/References
- [James Powell: So you want to be a Python expert? | PyData Seattle 2017](https://www.youtube.com/watch?v=sUmoMSU9_GQ)
- [Transforming Code into Beautiful, Idiomatic Python](https://www.youtube.com/watch?v=OSGv2VnC0go)
- [Python Tips](https://book.pythontips.com/)
- [Python Metaclasses](https://realpython.com/python-metaclasses/)
- Fluent Python book
