# Callable Objects: `__call__` 

## 1. What is a Callable in Python?

In Python, **"callable"** means **"can be called"**, like a function:

```python linenums="1"
def greet():
    print("Hello!")

greet()  # You "call" it
```

But functions aren’t the only things you can call. Any object with a `__call__` method defined can also be called **like a function**.

## 2. Basic Syntax of `__call__`

```python linenums="1"
class MyCallable:
    def __call__(self, *args, **kwargs):
        print("Called with", args, kwargs)

obj = MyCallable()
obj(1, 2, a=3)  # You can call the object like a function
```

## 3.  Why Use `__call__`?

It lets your **objects behave like functions**, which is useful when:  
- You want **stateful functions**  
- You're building **function wrappers or decorators**  
- You're implementing **custom logic with function-like interface**  
- You want to write **fluent APIs**  

## 4. Step-by-Step Tutorial
=== "1. A Simple Callable Class"
    ```python linenums="1"
    class Adder:
        def __init__(self, n):
            self.n = n

        def __call__(self, x):
            return x + self.n

    add5 = Adder(5)
    print(add5(10))  # 15
    ```
    add5(10)` works just like a function, but it’s actually an object that “remembers” `n=5`.  
=== "2. Tracking Calls or State"
    ```python
    class Counter:
        def __init__(self):
            self.count = 0

        def __call__(self):
            self.count += 1
            print(f"Called {self.count} times")

    c = Counter()
    c()
    c()
    ```

    Output:
    ```
    Called 1 times
    Called 2 times
    ```
=== "3. Callable as Decorator Alternative"
    ```python linenums="1"
    class Multiply:
        def __init__(self, factor):
            self.factor = factor

        def __call__(self, func):
            def wrapper(*args, **kwargs):
                return self.factor * func(*args, **kwargs)
            return wrapper

    @Multiply(3)
    def get_number():
        return 10

    print(get_number())  # 30
    ```
    So the `Multiply` object acts like a decorator!

=== "4. Callable Classes in ML / Data Pipelines"
    ```python linenums="1"
    class Normalize:
        def __init__(self, mean, std):
            self.mean = mean
            self.std = std

        def __call__(self, x):
            return (x - self.mean) / self.std

    norm = Normalize(mean=50, std=10)
    print(norm(60))  # 1.0
    ```

## 5. Checking if Something is Callable

```python linenums="1"
print(callable(len))      # True (function)
print(callable(42))       # False (int)
print(callable(Counter))  # True (you can instantiate it)
print(callable(Counter()))  # True (has __call__)
```

## 5. Difference Between `__call__` and `__init__`

- `__init__` initializes the object  
- `__call__` lets you use the object like a function  

```python linenums="1"
obj = MyClass()  # __init__ runs
obj()            # __call__ runs
```

## 6. Advanced: Chaining Calls
```python linenums="1"
class Chain:
    def __init__(self):
        self.values = []

    def __call__(self, val):
        self.values.append(val)
        return self  # Enables chaining

    def show(self):
        print("Chained values:", self.values)

c = Chain()(1)(2)(3)
c.show()
```

## 7. Summary

| Feature | Explanation |
|--------|-------------|
| `__call__` | Makes object callable like a function |
| Stateful | Can remember data (unlike regular function) |
| Used in | Decorators, Pipelines, Functional APIs |
| Chainable | Returns `self` for method chaining |

## 8. When to Use
Use `__call__` when:  
- You want to write classes that **behave like functions**  
- You need **state** to persist between calls  
- You want to build **cleaner and chainable APIs**  
