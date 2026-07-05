# Python A to Z — The Complete Master Guide

A structured path from absolute basics to advanced, production-level Python. Read top to bottom the first time; use it as a reference afterward.

---

## Table of Contents
1. Setup & Running Python
2. Syntax, Variables, Data Types
3. Operators
4. Control Flow
5. Strings Deep Dive
6. Data Structures (list, tuple, dict, set)
7. Functions
8. Comprehensions
9. Object-Oriented Programming
10. Error Handling
11. Modules & Packages
12. File Handling
13. Iterators & Generators
14. Decorators
15. Context Managers
16. Functional Programming Tools
17. Type Hints
18. Regular Expressions
19. Advanced OOP (metaclasses, properties, ABCs, slots)
20. Concurrency: Threading, Multiprocessing, Asyncio
21. Standard Library Highlights
22. Testing
23. Virtual Environments & Packaging
24. Memory Management & Performance
25. Best Practices & Idiomatic Python
26. Deeper Language Internals
27. Practical Tooling for Real Projects
28. Where to Go Next

---

## 1. Setup & Running Python

- Install from python.org or use a version manager (`pyenv`).
- Check version: `python3 --version`
- Run a script: `python3 script.py`
- Interactive REPL: `python3`
- Better REPL: `ipython`

```bash
python3 -m venv venv          # create virtual environment
source venv/bin/activate      # activate (Linux/Mac)
venv\Scripts\activate         # activate (Windows)
pip install package_name
```

---

## 2. Syntax, Variables, Data Types

Python uses **indentation** (not braces) to define blocks. Standard is 4 spaces.

```python
x = 10              # int
y = 3.14            # float
name = "Alice"      # str
is_valid = True     # bool
z = None            # NoneType
```

### Core built-in types
| Type | Example | Mutable? |
|---|---|---|
| int | `5` | No |
| float | `5.0` | No |
| str | `"hi"` | No |
| bool | `True` | No |
| list | `[1,2,3]` | Yes |
| tuple | `(1,2,3)` | No |
| dict | `{"a":1}` | Yes |
| set | `{1,2,3}` | Yes |

### Dynamic typing
Variables aren't declared with a type — the object has a type, the name is just a label.

```python
x = 5
x = "now I'm a string"   # totally legal
```

### Type checking & conversion
```python
type(x)              # <class 'str'>
isinstance(x, str)    # True
int("42")             # 42
str(42)               # "42"
float("3.14")         # 3.14
```

### Multiple assignment
```python
a, b, c = 1, 2, 3
a, b = b, a           # swap without a temp variable
```

---

## 3. Operators

```python
# Arithmetic
+ - * / // % **        # // floor div, ** power

# Comparison
== != > < >= <=

# Logical
and or not

# Identity vs equality
a == b   # same value?
a is b   # same object in memory?

# Membership
"a" in "cat"        # True
3 not in [1,2,4]    # True

# Walrus operator (assign inside expression) - Python 3.8+
if (n := len([1,2,3])) > 2:
    print(n)         # 3
```

---

## 4. Control Flow

```python
if x > 10:
    print("big")
elif x == 10:
    print("exact")
else:
    print("small")

for i in range(5):        # 0,1,2,3,4
    print(i)

for item in ["a", "b"]:
    print(item)

i = 0
while i < 5:
    i += 1

for i in range(10):
    if i == 5:
        break
    if i % 2 == 0:
        continue
    print(i)

# for/else — else runs only if loop completes without break
for i in range(3):
    print(i)
else:
    print("done without break")

# match statement (Python 3.10+, like switch)
match command:
    case "start":
        print("starting")
    case "stop" | "halt":
        print("stopping")
    case _:
        print("unknown")
```

---

## 5. Strings Deep Dive

```python
s = "Hello, World!"

s.lower(); s.upper(); s.strip(); s.split(",")
s.replace("Hello", "Hi")
s.startswith("Hello"); s.endswith("!")
s.find("World")          # index or -1
len(s)
s[0]; s[-1]; s[0:5]; s[::-1]      # slicing, reverse

# f-strings (preferred, Python 3.6+)
name, age = "Bob", 30
print(f"{name} is {age} years old")
print(f"{3.14159:.2f}")           # 3.14 — format spec
print(f"{age=}")                  # age=30 (debug shorthand)

# Multi-line strings
text = """
Line one
Line two
"""

# join
",".join(["a", "b", "c"])   # "a,b,c"
```

Strings are **immutable** — every "modification" creates a new string.

---

## 6. Data Structures

### Lists — ordered, mutable
```python
nums = [1, 2, 3]
nums.append(4)
nums.insert(0, 0)
nums.remove(2)
nums.pop()             # removes & returns last
nums.sort()
nums.sort(reverse=True)
sorted(nums)            # returns new list, doesn't mutate
nums[1:3]               # slicing
nums + [5, 6]           # concatenation
```

### Tuples — ordered, immutable
```python
point = (3, 4)
x, y = point            # unpacking
```
Use tuples for fixed collections (coordinates, records) — they're hashable so can be dict keys.

### Dictionaries — key/value, ordered (3.7+), mutable
```python
person = {"name": "Alice", "age": 30}
person["age"]
person.get("email", "N/A")     # safe access with default
person["email"] = "a@x.com"
person.keys(); person.values(); person.items()
for k, v in person.items():
    print(k, v)

# Merge dicts (3.9+)
d3 = {**person, "city": "NYC"}
d3 = person | {"city": "NYC"}
```

### Sets — unordered, unique elements
```python
s1 = {1, 2, 3}
s1.add(4)
s2 = {3, 4, 5}
s1 | s2      # union
s1 & s2      # intersection
s1 - s2      # difference
s1 ^ s2      # symmetric difference
```

### `collections` module (very useful)
```python
from collections import defaultdict, Counter, namedtuple, deque

dd = defaultdict(list)
dd["missing"].append(1)          # no KeyError

Counter("mississippi")            # Counter({'i':4,'s':4,'p':2,'m':1})

Point = namedtuple("Point", ["x", "y"])
p = Point(1, 2)

dq = deque([1,2,3])
dq.appendleft(0)                  # O(1) at both ends
```

---

## 7. Functions

```python
def greet(name, greeting="Hello"):     # default argument
    return f"{greeting}, {name}!"

greet("Alice")
greet("Alice", greeting="Hi")
greet(name="Alice")

def add(*args):                # variable positional args -> tuple
    return sum(args)

def config(**kwargs):          # variable keyword args -> dict
    for k, v in kwargs.items():
        print(k, v)

def full(a, b, *args, c=10, **kwargs):
    pass

# Lambda — anonymous one-line function
square = lambda x: x * x

# Scope
x = "global"
def outer():
    y = "enclosing"
    def inner():
        nonlocal y     # modify enclosing scope
        y = "modified"
    inner()
    return y

def modify_global():
    global x
    x = "modified"
```

### Recursion
```python
def factorial(n):
    return 1 if n <= 1 else n * factorial(n - 1)
```

---

## 8. Comprehensions

```python
squares = [x**2 for x in range(10)]
evens = [x for x in range(20) if x % 2 == 0]
matrix_flat = [x for row in matrix for x in row]     # nested

square_dict = {x: x**2 for x in range(5)}
unique_set = {x % 3 for x in range(10)}

gen = (x**2 for x in range(10))    # generator expression — lazy, memory-efficient
```

---

## 9. Object-Oriented Programming

```python
class Animal:
    species_count = 0                 # class variable, shared

    def __init__(self, name, sound):  # constructor
        self.name = name              # instance variable
        self.sound = sound
        Animal.species_count += 1

    def make_sound(self):
        return f"{self.name} says {self.sound}"

    def __str__(self):                # readable string (print())
        return f"Animal({self.name})"

    def __repr__(self):               # unambiguous string (debugging)
        return f"Animal(name={self.name!r}, sound={self.sound!r})"

a = Animal("Dog", "Woof")
print(a.make_sound())
```

### Inheritance
```python
class Dog(Animal):
    def __init__(self, name):
        super().__init__(name, "Woof")   # call parent constructor

    def make_sound(self):                # override (polymorphism)
        return f"{self.name} barks!"

class Cat(Animal):
    def make_sound(self):
        return f"{self.name} meows!"

for pet in [Dog("Rex"), Cat("Tom")]:
    print(pet.make_sound())          # different behavior, same call
```

### Encapsulation
```python
class Account:
    def __init__(self, balance):
        self._balance = balance       # convention: "protected"
        self.__secret = "hidden"      # name-mangled: "private"

    @property
    def balance(self):                # getter
        return self._balance

    @balance.setter
    def balance(self, value):         # setter with validation
        if value < 0:
            raise ValueError("Balance can't be negative")
        self._balance = value
```

### Common magic (dunder) methods
```python
class Vector:
    def __init__(self, x, y): self.x, self.y = x, y
    def __add__(self, other): return Vector(self.x+other.x, self.y+other.y)
    def __eq__(self, other): return self.x == other.x and self.y == other.y
    def __len__(self): return 2
    def __getitem__(self, i): return (self.x, self.y)[i]
```

---

## 10. Error Handling

```python
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")
except (TypeError, ValueError) as e:
    print("Multiple types caught")
else:
    print("Runs if no exception")      # only if try succeeded
finally:
    print("Always runs")               # cleanup

# Raising your own
def withdraw(balance, amount):
    if amount > balance:
        raise ValueError("Insufficient funds")

# Custom exceptions
class InsufficientFundsError(Exception):
    pass

# Chaining exceptions
try:
    1/0
except ZeroDivisionError as e:
    raise RuntimeError("Calculation failed") from e
```

---

## 11. Modules & Packages

```python
# mymodule.py
def helper(): pass

# main.py
import mymodule
from mymodule import helper
from mymodule import helper as h
import mymodule as mm

if __name__ == "__main__":     # runs only when executed directly, not when imported
    main()
```

A **package** is a folder with `__init__.py` containing modules:
```
mypackage/
    __init__.py
    module1.py
    module2.py
```

---

## 12. File Handling

```python
with open("file.txt", "r") as f:   # context manager auto-closes file
    content = f.read()
    lines = f.readlines()

with open("out.txt", "w") as f:
    f.write("hello\n")

with open("out.txt", "a") as f:    # append
    f.write("more\n")

import json
with open("data.json") as f:
    data = json.load(f)
json.dumps(data, indent=2)

import csv
with open("data.csv") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row)
```

---

## 13. Iterators & Generators

An **iterable** has `__iter__`; an **iterator** has `__next__`.

```python
class Counter:
    def __init__(self, limit): self.limit, self.n = limit, 0
    def __iter__(self): return self
    def __next__(self):
        if self.n >= self.limit: raise StopIteration
        self.n += 1
        return self.n

# Generators — simpler way to write iterators
def count_up_to(limit):
    n = 1
    while n <= limit:
        yield n              # pauses here, resumes on next()
        n += 1

for num in count_up_to(5):
    print(num)

# yield from — delegate to a sub-generator
def chain(*iterables):
    for it in iterables:
        yield from it
```
Generators are **lazy** — values are computed on demand, saving memory for large sequences.

---

## 14. Decorators

A decorator wraps a function to extend behavior without modifying it.

```python
import functools
import time

def timer(func):
    @functools.wraps(func)          # preserves original function's name/docstring
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        print(f"{func.__name__} took {time.time()-start:.4f}s")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)

# Decorator with arguments
def repeat(times):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def greet(): print("Hi")

# Built-in decorators
class MyClass:
    @staticmethod
    def util(): pass            # no self/cls

    @classmethod
    def factory(cls): return cls()   # receives class, not instance

    @property
    def value(self): return self._value
```

---

## 15. Context Managers

```python
class ManagedFile:
    def __init__(self, name): self.name = name
    def __enter__(self):
        self.file = open(self.name, "w")
        return self.file
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.file.close()

with ManagedFile("hello.txt") as f:
    f.write("hi")

# Simpler with contextlib
from contextlib import contextmanager

@contextmanager
def managed_file(name):
    f = open(name, "w")
    try:
        yield f
    finally:
        f.close()
```

---

## 16. Functional Programming Tools

```python
from functools import reduce

list(map(lambda x: x*2, [1,2,3]))          # [2,4,6]
list(filter(lambda x: x % 2 == 0, [1,2,3,4]))  # [2,4]
reduce(lambda a,b: a+b, [1,2,3,4])          # 10

from functools import partial
add5 = partial(lambda a,b: a+b, 5)
add5(10)     # 15

from functools import lru_cache
@lru_cache(maxsize=None)          # memoization
def fib(n):
    return n if n < 2 else fib(n-1) + fib(n-2)
```

---

## 17. Type Hints

```python
def greet(name: str, age: int = 0) -> str:
    return f"{name} is {age}"

from typing import List, Dict, Optional, Union, Callable, Tuple

def process(items: List[int]) -> Dict[str, int]: ...
def find(x: Optional[int] = None) -> Union[int, str]: ...
def apply(fn: Callable[[int], int], val: int) -> int: return fn(val)

# Python 3.9+ can use built-ins directly
def process(items: list[int]) -> dict[str, int]: ...

# 3.10+ union syntax
def find(x: int | None = None) -> int | str: ...
```
Type hints are not enforced at runtime — use `mypy` for static checking.

---

## 18. Regular Expressions

```python
import re

re.match(r"\d+", "123abc")          # matches at start
re.search(r"\d+", "abc123")         # searches anywhere
re.findall(r"\d+", "a1 b22 c333")   # ['1', '22', '333']
re.sub(r"\s+", " ", "too    many   spaces")

pattern = re.compile(r"(\w+)@(\w+)\.com")
m = pattern.search("contact me at bob@example.com")
if m:
    m.group(0)   # full match
    m.group(1)   # first capture group
```

---

## 19. Advanced OOP

### Abstract Base Classes
```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self): pass

class Circle(Shape):
    def __init__(self, r): self.r = r
    def area(self): return 3.14159 * self.r ** 2
# Shape() would raise TypeError — can't instantiate abstract class
```

### `__slots__` — memory optimization
```python
class Point:
    __slots__ = ["x", "y"]     # no __dict__, less memory, faster attribute access
    def __init__(self, x, y): self.x, self.y = x, y
```

### Metaclasses — classes that create classes
```python
class Meta(type):
    def __new__(mcs, name, bases, namespace):
        namespace["greet"] = lambda self: "hi"
        return super().__new__(mcs, name, bases, namespace)

class MyClass(metaclass=Meta):
    pass

MyClass().greet()   # "hi"
```

### Multiple inheritance & MRO
```python
class A: pass
class B: pass
class C(A, B): pass
C.__mro__     # Method Resolution Order — determines lookup order
```

### Dataclasses (Python 3.7+) — reduce boilerplate
```python
from dataclasses import dataclass

@dataclass
class Point:
    x: int
    y: int = 0             # auto-generates __init__, __repr__, __eq__
```

---

## 20. Concurrency

### Threading — good for I/O-bound tasks (GIL limits true CPU parallelism)
```python
import threading

def worker(n):
    print(f"Worker {n}")

threads = [threading.Thread(target=worker, args=(i,)) for i in range(5)]
for t in threads: t.start()
for t in threads: t.join()

lock = threading.Lock()
with lock:
    pass   # critical section
```

### Multiprocessing — true parallelism for CPU-bound tasks
```python
from multiprocessing import Pool

def square(x): return x * x

with Pool(4) as p:
    results = p.map(square, range(10))
```

### Asyncio — cooperative concurrency for I/O
```python
import asyncio

async def fetch_data():
    await asyncio.sleep(1)
    return "data"

async def main():
    result = await fetch_data()
    results = await asyncio.gather(fetch_data(), fetch_data())

asyncio.run(main())
```

---

## 21. Standard Library Highlights

```python
import datetime, os, sys, random, itertools, math, pathlib

datetime.datetime.now()
os.path.join("dir", "file.txt")
pathlib.Path("dir/file.txt").exists()
random.choice([1,2,3]); random.randint(1,10); random.shuffle(mylist)

itertools.combinations([1,2,3], 2)
itertools.permutations([1,2,3])
itertools.chain([1,2], [3,4])
itertools.cycle([1,2,3])

math.sqrt(16); math.floor(3.7); math.ceil(3.2)
```

---

## 22. Testing

```python
# test_math.py
import unittest

def add(a, b): return a + b

class TestMath(unittest.TestCase):
    def test_add(self):
        self.assertEqual(add(2, 3), 5)

if __name__ == "__main__":
    unittest.main()
```

```python
# pytest style (simpler, widely used)
def test_add():
    assert add(2, 3) == 5

# run with: pytest test_math.py
```

---

## 23. Virtual Environments & Packaging

```bash
python3 -m venv venv
source venv/bin/activate
pip install requests
pip freeze > requirements.txt
pip install -r requirements.txt
```

Modern projects use `pyproject.toml` with tools like `poetry` or `uv` for dependency management.

---

## 24. Memory Management & Performance

- Python uses **reference counting** + a **garbage collector** for cyclic references.
- `sys.getsizeof(obj)` shows memory footprint.
- The **GIL** (Global Interpreter Lock) means only one thread executes Python bytecode at a time — use multiprocessing for CPU-bound parallelism.
- Use generators instead of lists for large datasets to save memory.
- `timeit` module for micro-benchmarking:
```python
import timeit
timeit.timeit("sum(range(100))", number=10000)
```

---

## 25. Best Practices & Idiomatic Python

- Follow **PEP 8** style guide (naming, spacing, line length).
- "Ask forgiveness, not permission" (EAFP) — prefer try/except over checking conditions upfront where idiomatic.
- Use `enumerate()` instead of manual index counters.
- Use `zip()` to iterate multiple lists together.
- Prefer f-strings over `%` or `.format()`.
- Write docstrings for functions/classes.
- Use `is` only for `None`, `True`, `False` comparisons — never for value equality.
- Avoid mutable default arguments:
```python
def bad(items=[]): ...       # BUG: shared across calls
def good(items=None):
    items = items or []
```

---

## 26. Deeper Language Internals

### `__new__` vs `__init__`
`__new__` creates the instance (rarely overridden); `__init__` initializes it. Override `__new__` for immutable types or singletons.

```python
class Singleton:
    _instance = None
    def __new__(cls, *args, **kwargs):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
```

### Descriptors — the mechanism behind `@property`
A descriptor is any object defining `__get__`/`__set__`/`__delete__`, used as a class attribute to control access on *any* instance.

```python
class PositiveNumber:
    def __set_name__(self, owner, name):
        self.name = "_" + name
    def __get__(self, obj, objtype=None):
        return getattr(obj, self.name)
    def __set__(self, obj, value):
        if value < 0:
            raise ValueError("must be positive")
        setattr(obj, self.name, value)

class Product:
    price = PositiveNumber()      # reusable validation across any class
    def __init__(self, price):
        self.price = price
```

### `__init_subclass__` — hook into subclass creation
```python
class Plugin:
    registry = []
    def __init_subclass__(cls, **kwargs):
        super().__init_subclass__(**kwargs)
        Plugin.registry.append(cls)     # auto-register every subclass

class MyPlugin(Plugin): pass
Plugin.registry   # [MyPlugin]
```

### More dunder/operator overloading
```python
class Money:
    def __init__(self, amount): self.amount = amount
    def __lt__(self, other): return self.amount < other.amount
    def __le__(self, other): return self.amount <= other.amount
    def __hash__(self): return hash(self.amount)      # needed if __eq__ defined + used in sets/dicts
    def __contains__(self, item): return item == self.amount
    def __bool__(self): return self.amount != 0
```

### Enums
```python
from enum import Enum, auto

class Status(Enum):
    PENDING = auto()
    ACTIVE = auto()
    DONE = auto()

Status.ACTIVE.name    # "ACTIVE"
Status.ACTIVE.value   # 2
```

### `functools.singledispatch` — function overloading by type
```python
from functools import singledispatch

@singledispatch
def process(arg):
    print("default", arg)

@process.register
def _(arg: int):
    print("int handler", arg)

@process.register
def _(arg: str):
    print("str handler", arg)
```

### Weak references
```python
import weakref
class Node: pass
n = Node()
ref = weakref.ref(n)     # doesn't keep n alive; ref() returns None once n is garbage collected
```

---

## 27. Practical Tooling for Real Projects

### Logging — never use `print()` in real code
```python
import logging

logging.basicConfig(level=logging.INFO, format="%(asctime)s %(levelname)s %(message)s")
logger = logging.getLogger(__name__)

logger.debug("Detailed diagnostic info")
logger.info("General info")
logger.warning("Something unexpected")
logger.error("A failure occurred")
logger.exception("Logs full traceback")   # use inside except block
```

### Debugging
```python
# Insert a breakpoint anywhere (Python 3.7+)
breakpoint()          # drops into pdb debugger at this line

# pdb commands once stopped: n (next), s (step in), c (continue), p var (print), l (list code)
```

### Environment variables & config
```python
import os
from dotenv import load_dotenv     # pip install python-dotenv

load_dotenv()                      # loads variables from a .env file
api_key = os.getenv("API_KEY", "default_value")
```

### Linting & formatting
```bash
pip install black ruff mypy
black .          # auto-formats code to a consistent style
ruff check .     # fast linter, catches bugs/unused imports/style issues
mypy .           # static type checker, validates your type hints
```

### Packaging your own library
Minimal `pyproject.toml`:
```toml
[project]
name = "mypackage"
version = "0.1.0"
dependencies = ["requests"]

[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"
```
```bash
pip install build twine
python -m build             # creates dist/ with .whl and .tar.gz
twine upload dist/*          # publish to PyPI
```

### Databases
```python
import sqlite3
conn = sqlite3.connect("app.db")
cur = conn.cursor()
cur.execute("CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT)")
cur.execute("INSERT INTO users (name) VALUES (?)", ("Alice",))
conn.commit()
cur.execute("SELECT * FROM users")
cur.fetchall()
```
For real applications, an ORM like **SQLAlchemy** maps Python classes to database tables instead of writing raw SQL.

### HTTP requests & building an API
```python
import requests
r = requests.get("https://api.example.com/data")
r.json()

# Minimal API with FastAPI
from fastapi import FastAPI
app = FastAPI()

@app.get("/hello/{name}")
def hello(name: str):
    return {"message": f"Hello {name}"}
# run with: uvicorn main:app --reload
```

---

## 28. Where to Go Next

- Build small projects (CLI tools, web scraper, REST API with FastAPI/Flask).
- Learn a web framework (Django or FastAPI).
- Learn `numpy`/`pandas` if going into data.
- Read real-world open-source Python code.
- Practice on LeetCode/HackerRank for algorithmic fluency.
- Read *Fluent Python* by Luciano Ramalho once comfortable with the basics — it's the definitive "next level" book.

---

**How to use this guide:** work through sections 1–13 first and write code for each concept yourself — don't just read. Sections 14–20 are where "intermediate" becomes "advanced." Revisit this file as a reference once you start building real projects.
