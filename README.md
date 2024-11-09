### Decorators in Python

A **decorator** is a function that allows you to modify or extend the behavior of another function or method without permanently modifying its source code. Decorators are commonly used to add functionality like logging, access control, memoization, and more, in a clean and reusable way.

Decorators in Python are applied using the `@` syntax just before the function definition.

#### Basic Structure of a Decorator

Here's how a basic decorator works:



1. A function is defined that takes another function as an argument.
2. The decorator function wraps the original function with additional behavior.
3. The decorator returns a new function that calls the original function.

#### Example:

```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

# Applying the decorator to a function
@my_decorator
def say_hello():
    print("Hello!")

# Calling the decorated function
say_hello()
```

**Output:**
```
Something is happening before the function is called.
Hello!
Something is happening after the function is called.
```

In this example, `my_decorator` takes `say_hello` as an argument, wraps it in the `wrapper` function, and adds extra behavior before and after calling the original function.

#### Decorators with Arguments

Decorators can also take arguments. If the function you're decorating takes arguments, you'll need to pass those arguments to the decorated function as well.

Example with arguments:

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before function call")
        result = func(*args, **kwargs)
        print("After function call")
        return result
    return wrapper

@my_decorator
def add(a, b):
    return a + b

print(add(2, 3))
```

**Output:**
```
Before function call
After function call
5
```

### Generators in Python

A **generator** is a type of iterable, like a list or a tuple, but instead of returning all the values at once, it generates values on the fly, one at a time, as they are needed. This is particularly useful when working with large datasets or streams of data where you don't want to load everything into memory at once.

A generator is defined using a function that contains one or more `yield` statements. The `yield` keyword is used to produce a value and pause the function's execution, allowing it to be resumed later.

#### Basic Structure of a Generator:

1. Use the `yield` keyword inside the function to generate values.
2. Calling a generator function returns a **generator object**, which can be iterated over using `next()` or a loop.

#### Example:

```python
def count_up_to(n):
    count = 1
    while count <= n:
        yield count
        count += 1

# Create a generator object
counter = count_up_to(5)

# Iterate through the generator
for num in counter:
    print(num)
```

**Output:**
```
1
2
3
4
5
```

In this example, the `count_up_to` function is a generator that yields numbers from 1 to `n` one by one. Each time `yield` is called, the function's state is saved, and execution is paused until the next value is requested.

#### Using `next()` with Generators

You can also manually retrieve the next value from a generator using the `next()` function.

```python
gen = count_up_to(3)

print(next(gen))  # 1
print(next(gen))  # 2
print(next(gen))  # 3
print(next(gen))  # Raises StopIteration
```

#### Advantages of Generators:

1. **Memory Efficiency**: Generators only produce one item at a time, so they are much more memory-efficient than lists, especially for large datasets.
2. **Lazy Evaluation**: Values are computed only when requested, which can save time and resources in certain situations.
3. **Stateful Iteration**: Generators maintain their state between `yield` calls, which is useful for processes like reading from a file or generating sequences on demand.

#### Example: Generator for Fibonacci Sequence

```python
def fibonacci(n):
    a, b = 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b

# Get the first 5 Fibonacci numbers
for number in fibonacci(5):
    print(number)
```

**Output:**
```
0
1
1
2
3
```

Here, `fibonacci` is a generator that yields the first `n` numbers in the Fibonacci sequence.

### Combining Decorators and Generators

You can combine both decorators and generators in Python. For example, you could write a decorator that wraps around a generator function to modify or log its behavior.

```python
def log_generator(func):
    def wrapper(*args, **kwargs):
        print(f"Starting generator {func.__name__}")
        gen = func(*args, **kwargs)
        for value in gen:
            print(f"Yielded value: {value}")
            yield value
        print(f"Generator {func.__name__} finished.")
    return wrapper

@log_generator
def my_gen(n):
    for i in range(n):
        yield i

# Call the decorated generator
for item in my_gen(3):
    print(f"Processed: {item}")
```

**Output:**
```
Starting generator my_gen
Yielded value: 0
Processed: 0
Yielded value: 1
Processed: 1
Yielded value: 2
Processed: 2
Generator my_gen finished.
```

In this example, the `log_generator` decorator adds logging around the `my_gen` generator, showing when values are yielded.

### Summary:

- **Decorators**: Functions that modify the behavior of other functions or methods, often used for cross-cutting concerns like logging, authentication, etc.
- **Generators**: Functions that use the `yield` keyword to generate values lazily, making them memory-efficient and suitable for working with large datasets.
- **Combining**: You can apply decorators to generator functions to enhance their behavior while maintaining the advantages of generators.

These tools are powerful and widely used in Python to create clean, efficient, and reusable code.
