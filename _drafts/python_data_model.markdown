---
layout: post
title:  "Data Model in Python"
categories: Programming
tags: [Python]
excerpt: ""
---
### [data model](https://docs.python.org/3/reference/datamodel.html)
#### 1. Objects, Values and Types 
- object
  - identity
    - unique and never changed once created
    - use the `is` operator to compare
    - `id` returns the identity 
      - in CPython, `id` returns the memory address where it is stored
- type
    - the `type` function
- value
    - mutable: list, dictionary
    - immutable: number, string, tuple
    - **Note:**
      - "The value of an immutable container object that contains a reference to a mutable object can change when the latter’s value is changed; however the container is still considered immutable, because the collection of objects it contains cannot be change"
      ```python
      t = ([1, 2], [3, 4])
      t[0][0] = 0          # ([0, 2], [3, 4])
      ```
      - a container is an object that contains references to other objects such as tuple, list and dictionary
- garbage collection
  - never explicitly destroyed
  - in CPython, reference-counting with an optional delayed detection of cyclically linked garbage
  - `try...catch`, debugging, and tracing falicities may keep objects alive which are collectable
  - the `gc` module
  - explicitly call `close` methods 
  - use the statements `try...finally` and `with`
- the different behaviors of different types/mutability
  ```python
  a = 1
  b = 1 # a and b may refer to the same object (depends on implementation, numbers are immutable)
  c = []
  d = [] # c and d refers to the different objects definitely as a list is mutable
  e = f = [] # this is the syntax to asign the same object to both e and f
  ```

#### 2. The standard type hierarchy
- None
  - to signify absense of a value
- NotImplemented
- Ellipsis
- numbers.Number
  - numbers.Integral
    - Integers (int)
    - Boolean (bool)
  - numbers.Real (float)
  - numbers.Complex (complex)
- Sequences
  - Immutable sequences
    - Strings
    - Tuples
    - Bytes
  - Mutable sequences
    - Lists
    - Byte Arrays
- Set types
  - Sets
  - Frozen sets
- Mapping
  - Dictionaries
- Callable types
  - User-defined functions
  - Instance methods
  - Generator functions
  - Coroutine functions
  - Asynchronous generator functions
  - Built-in functions
  - Built-in methods
  - Classes
  - Class Instances
- Modules
- Custom classes
- Class instances
- I/O objects (file objects)
- Internal types
  - Code objects
  - Frame objects
  - Traceback objects
  - Slice objects
  - Static method objects
  - Class method objects

