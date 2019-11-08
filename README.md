# Not an Introduction to Python
## Beyond `print("Hello World")`

We'll probably cover
- Dunder magic
  - `__slots__`
  - Other dunder methods
- Decorators
- Context Managers
- Mixins
- Concurrency
- Metaclasses
- Some tricks

## `__slots__`

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

## Concurrency

## Metaclasses 🚨

## Quick Tricks

### The `lru_cache`

### Transposing a list of lists

###

## Credits/References
- [Transforming Code into Beautiful, Idiomatic Python](https://www.youtube.com/watch?v=OSGv2VnC0go)
