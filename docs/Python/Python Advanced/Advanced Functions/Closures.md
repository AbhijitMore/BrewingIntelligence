# Python Closures

## 1. What is a Closure?

A **closure** is a function object that retains access to variables from its enclosing scope, even after the outer function has finished executing.

In simple terms, a **closure** allows a nested function to "remember" the environment in which it was created.

## 2. Why Use Closures?

Closures are used for:  
- Data hiding / encapsulation  
- Factory functions  
- Callback functions  
- Avoiding use of global variables  
- Maintaining state without using classes

##  3. Understanding with an Example

### Basic Structure:

!!! note "Code & explanation"
    ```python linenums="1"
    def outer_function(msg):
        def inner_function():
            print(msg)
        return inner_function

    greet = outer_function("Hello, Abhijit!")
    greet()  # Output: Hello, Abhijit!
    ```
    **explanation**    
    - `inner_function` is **defined inside** `outer_function`.  
    - It **remembers** the value of `msg` even though `outer_function` has finished executing.  
    - `greet` becomes a **closure**.  

## 4. Checking if a Function is a Closure

```python
print(greet.__closure__)  # Contains cell objects with enclosed variables
print(greet.__closure__[0].cell_contents)  # Output: Hello, Abhijit!
```

## 5. Another Example: Counter Function

!!! note "Code & explanation"
    ```python linenums="1"
    def make_counter():
        count = 0
        def counter():
            nonlocal count
            count += 1
            return count
        return counter

    counter1 = make_counter()
    print(counter1())  # 1
    print(counter1())  # 2

    counter2 = make_counter()
    print(counter2())  # 1
    ```
    ***What's Going On?***  
    - Each `make_counter()` call creates a **new scope**.  
    - The nested `counter()` remembers `count` through closure.  
    - `nonlocal` allows modifying the enclosing scope’s variable.  

## 6. Common Mistake with Closures in Loops: Late Binding

**Late binding** means that the value of a variable used in a closure is looked up **when the inner function is called**, not when it was defined. This often causes unexpected results when using closures inside loops.
!!! note "Code Example & explanation"
    ```python linenums="1"
    functions = []
    for i in range(3):
        def f():
            return i
        functions.append(f)

    print([func() for func in functions])  # Output: [2, 2, 2] — NOT [0, 1, 2]
    ```
    **Why this happens?**  
    - The function `f()` does not capture the **value** of `i`, it captures a **reference** to the variable `i`.  
    - By the time `f()` is called (after the loop finishes), `i` has the final value from the loop: `2`.  
    - So all closures return the same value: `2`.  

**Solution: Use Default Arguments to Simulate Early Binding**  

A common and effective way to fix the late binding issue is to use **default arguments**. This binds the current value of the variable to the function at the time it is defined.

!!! note "Code Fix & explanation"
    ```python linenums="1"
    functions = []
    for i in range(3):
        def f(i=i):  # i is captured by value at definition time
            return i
        functions.append(f)

    print([func() for func in functions])  # Output: [0, 1, 2]
    ```
    **Why this works**
    - The function’s default argument `i=i` captures the **current value** of `i` when the function is defined.  
    - This simulates **early binding**, giving each function its own copy of the loop variable.  
    - Now each closure works as expected.  


## 7. Real-World Use Case: Logger Factory

Imagine you are building a large application where different parts of your program need to log messages with different severity levels (INFO, DEBUG, ERROR, etc.).

Instead of writing a full class for logging, you can use a closure to create lightweight, customized loggers.

### How it works:

- `make_logger(level)` creates a logger that **remembers** the `level` it was created with.
- The returned `log` function formats the message with the correct level.

!!! note "Code & Exaplaination"
    ```python linenums="1"
    def make_logger(level):
        def log(message):
            print(f"[{level}] {message}")
        return log

    # Create specialized loggers
    info_logger = make_logger("INFO")
    error_logger = make_logger("ERROR")

    # Use them
    info_logger("System is running smoothly.")    # [INFO] System is running smoothly.
    error_logger("System failure detected!")      # [ERROR] System failure detected!
    ```

### Why this is powerful:
- No need for classes or passing `level` every time.  
- Keeps your code DRY (Don't Repeat Yourself).  
- Easy to create multiple, independent loggers.  
- Perfect for microservices, scripts, or utilities needing simple logging.  

## 8. Closures vs Lambdas

Closures can also utilize **lambda functions**. A closure with a lambda can allow us to create concise functions while still capturing variables from the enclosing scope.

!!! note "Code Example and explanation"
    ```python linenums="1"
    def power(n):
        return lambda x: x ** n

    square = power(2)
    cube = power(3)

    print(square(4))  # 16
    print(cube(2))    # 8
    ```
    **Explanation:**  
    - The function `power` returns a lambda function.  
    - The lambda function takes one argument `x` and raises it to the power of `n`.  
    - Even though `power` has finished execution, the `lambda` function remembers the value of `n` (because it is closed over the variable `n`).  

### When to Use Lambdas with Closures
- When you need to create a quick, unnamed function within a larger function.  
- When you want to create functions that retain state from the enclosing scope but in a compact form.  
- When working with higher-order functions like `map`, `filter`, or `reduce`, which expect a function as an argument.  


## 9. Summary

In this section, we'll summarize the key concepts, terms, and common use cases for Python closures.

| Term         | Meaning                                           |
|--------------|---------------------------------------------------|
| **Closure**  | A closure is a function that "remembers" the environment in which it was created. Specifically, it retains access to variables from its enclosing scope even after the outer function has finished executing. |
| **nonlocal** | The `nonlocal` keyword is used to modify a variable in an enclosing scope (but not global). It allows inner functions to modify variables from their enclosing functions, which is crucial for closures. |
| **Free Variables** | These are variables used in a function that are not defined within the function itself. In closures, these free variables are bound to the function’s environment when the closure is created. |

!!! note "Use Cases"
    === "1. State Retention Without Classes"
        Closures allow you to create functions that maintain state between function calls, without using a class or object. For example, a counter function can retain its state using closures.

        ```python linenums="1"
        def make_counter():
            count = 0
            def counter():
                nonlocal count
                count += 1
                return count
            return counter

        counter1 = make_counter()
        print(counter1())  # 1
        print(counter1())  # 2
        ```

    === "2. Factory Functions"
        Closures can be used to generate multiple functions from a single function definition. These are often referred to as factory functions, where the outer function returns a function customized with parameters.

        ```python linenums="1"
        def make_logger(level):
            def log(message):
                print(f"[{level}] {message}")
            return log

        info_logger = make_logger("INFO")
        error_logger = make_logger("ERROR")

        info_logger("This is an info message.")   # [INFO] This is an info message.
        error_logger("This is an error message.") # [ERROR] This is an error message.
        ```

    === "3. Callback Functions"
        Closures can be passed around as callback functions to handle asynchronous tasks, event handling, etc. This is particularly useful when dealing with tasks where you need to retain a context or state when the callback is executed later.

        ```python linenums="1"
        def delayed_print(msg, delay):
            import time
            def inner_print():
                time.sleep(delay)
                print(msg)
            return inner_print

        delayed_hello = delayed_print("Hello after 2 seconds!", 2)
        delayed_hello()  # Prints "Hello after 2 seconds!" after 2 seconds.
        ```

    === "4. Decorators"
        Closures are essential for decorators, which allow you to modify the behavior of a function without changing its code. A decorator is simply a function that wraps another function, and it often relies on closures to preserve state or modify behavior.

        ```python linenums="1"
        def add_logging(func):
            def wrapper(*args, **kwargs):
                print(f"Calling function {func.__name__}")
                return func(*args, **kwargs)
            return wrapper

        @add_logging
        def greet(name):
            print(f"Hello {name}")

        greet("Abhijit")  # Output: Calling function greet
                            #         Hello Abhijit
        ```

    === "5. Encapsulation"
        Closures can be used to encapsulate data and functions. This is especially useful when you want to hide or protect data from being accessed or modified directly from outside the scope. For example, you can create "private" variables that can only be accessed through specific methods.

        ```python linenums="1"
        def counter(start=0):
            count = start
            def get_count():
                return count
            def increment():
                nonlocal count
                count += 1
            return get_count, increment

        get_count, increment = counter(5)
        print(get_count())  # 5
        increment()
        print(get_count())  # 6
        ```

??? note "Practice Exercises"
    === "1. Create a multiplier function"
        **Objective**: Use a closure to generate functions that multiply by a given number.
        ```python linenums="1"
        def multiplier(n):
            def multiply(x):
                return x * n
            return multiply

        double = multiplier(2)
        triple = multiplier(3)
        print(double(5))  # 10
        print(triple(4))  # 12
        ```
        **explanation**  
        - `multiplier(n)` returns the function `multiply`, which remembers the value of `n`.  
            - This is a **classic use of closures as function factories**.  
            - `double` and `triple` are each closures with their own copy of `n`.  
    === "2. Memoization using closures (like a simple cache)"
        **Objective**: Use a closure to cache results of expensive computations.
        ```python linenums="1"
        import time

        def memoize():
            cache = {}
            def get(key, compute):
                if key not in cache:
                    cache[key] = compute()
                return cache[key]
            return get

        def slow_add():
            time.sleep(2)
            return 5 + 5

        memo = memoize()
        # First call – takes time
        print(memo("add10", slow_add))  # Output: 10 (after 2s delay)
        # Second call – instant!
        print(memo("add10", slow_add))  # Output: 10 (cached)
        ```
        **Exaplaination**  
        - The outer function `memoize()` creates a private `cache` dictionary.  
        - The inner function `get()` checks if a result is already cached.  
        - If not, it runs `compute()` and stores the result.  
        - The inner function keeps a reference to `cache` via the closure.  