# Python Decorators
## 1. What is a Decorator?

A **decorator** in Python is a function that takes another function (or method) as an argument, adds some functionality, and returns a new function that typically calls the original one. Decorators are often used to modify or enhance the behavior of functions or methods without permanently modifying them.

!!! note "Code Example"
    ```python linenums="1"
    def decorator_function(original_function):
        def wrapper_function():
            print("Wrapper executed before", original_function.__name__)
            return original_function()
        return wrapper_function
    ```
    **explanation**  
    - `original_function` is the function being decorated.  
    - `wrapper_function` is the new function that wraps around the original one.  
    - It adds behavior (e.g., print statement) before calling the original function.  

## 2. Functions are First-Class Citizens
In Python, **functions are first-class citizens**, which means they can be:  
- Assigned to variables  
- Passed as arguments to other functions  
- Returned from other functions  

!!! note "Code Example"
    ```python linenums="1"
    def greet():
        return "Hello!"

    hello = greet
    print(hello())  # Output: Hello!
    ```
    Here, `greet` is assigned to `hello`, which is then called like a function.  
    This flexibility is the foundation for decorators.

## 3. Nested Functions and Closures

Python allows defining functions inside other functions. These are called **nested functions**. When the inner function captures variables from the outer function’s scope, it's called a **closure**.  
!!! note "Code Example"
    ```python linenums="1"
    def outer():
        msg = "Hi"
        def inner():
            print(msg)
        return inner

    my_func = outer()
    my_func()  # Output: Hi
    ```
    **Explanation:**  
    - `outer` defines `msg` and returns `inner`.  
    - `inner` remembers the value of `msg` even after `outer` has finished.  
    - This is a closure — an important concept for decorators.  

## 4. Creating a Simple Decorator

Let’s create a decorator manually and see how it wraps a function.
!!! note "Code Example"
    ```python linenums="1"
    def simple_decorator(func):
        def wrapper():
            print("Before function call")
            func()
            print("After function call")
        return wrapper

    def say_hello():
        print("Hello!")

    decorated = simple_decorator(say_hello)
    decorated()
    ```
    **Explanation:**  
    - `simple_decorator` takes `say_hello` as input.  
    - It wraps it with `wrapper`, adding print statements before and after.  
    - When `decorated()` is called, it runs the wrapper logic.  

## 5. Using `@` Syntax (Pythonic View)
Python provides a shorthand `@decorator_name` to apply a decorator to a function.

!!! note "Code Exmample"
    ```python linenums="1"
    @simple_decorator
    def say_hi():
        print("Hi!")

    say_hi()
    ```
    This is equivalent to `say_hi = simple_decorator(say_hi)`.  
    It’s cleaner and more readable.  

## 6. Decorators with Arguments
To handle functions with arguments, use `*args` and `**kwargs` in the wrapper.

!!! note "Code Example"
    ```python linenums="1"
    def decorator_with_args(func):
        def wrapper(*args, **kwargs):
            print(f"Called with args: {args}, kwargs: {kwargs}")
            return func(*args, **kwargs)
        return wrapper

    @decorator_with_args
    def greet(name):
        print(f"Hello, {name}!")

    greet("Abhijit")
    ```
    **Explanation:**  
    - `*args` captures positional arguments.  
    - `**kwargs` captures keyword arguments.  
    - This makes the decorator flexible to decorate any function. 

## 7. Chaining Multiple Decorators

You can apply multiple decorators to one function. They are executed from bottom to top.

!!! note "Code Example"
    ```python linenums="1"
    def bold(func):
        def wrapper():
            return "<b>" + func() + "</b>"
        return wrapper

    def italic(func):
        def wrapper():
            return "<i>" + func() + "</i>"
        return wrapper

    @bold
    @italic
    def say_text():
        return "Hello!"

    print(say_text())  # Output: <b><i>Hello!</i></b>
    ```
    **Order:**  
    - `italic` is applied first.  
    - Then `bold` wraps that result.  

## 8. `functools.wraps` – Preserving Metadata

When decorating, the original function’s name and docstring are lost. Use `functools.wraps` to preserve them.

!!! note "Code Example"
    ```python linenums="1"
    from functools import wraps

    def logging_decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            print(f"Calling {func.__name__}")
            return func(*args, **kwargs)
        return wrapper

    @logging_decorator
    def example():
        """This is a docstring."""
        print("Running example.")

    print(example.__name__)  # Output: example
    print(example.__doc__)   # Output: This is a docstring.
    ```

## 9. Class-Based Decorators (Advanced)

Instead of functions, you can use classes to define decorators. This is useful when you need to maintain state.
!!! note "Code Example"
    ```python linenums="1"
    class CountCalls:
        def __init__(self, func):
            self.func = func
            self.count = 0

        def __call__(self, *args, **kwargs):
            self.count += 1
            print(f"Call {self.count} of {self.func.__name__}")
            return self.func(*args, **kwargs)

    @CountCalls
    def say_hello():
        print("Hello!")

    say_hello()
    say_hello()
    ```
    **Explanation:**  
    - The `__call__` method makes an instance callable like a function.  
    - State (`self.count`) is maintained across calls.  

## 10. Summary

| Concept             | Description                                |
|---------------------|--------------------------------------------|
| Decorator           | Function that wraps another function       |
| `@decorator`        | Syntactic sugar for applying decorators    |
| `*args, **kwargs`   | Allow decorators to handle any arguments   |
| `functools.wraps`   | Preserves metadata of original function    |
| Real-world uses     | Logging, timing, access control, caching   |

!!! note "Real World Use Cases"
    === "1. Logging Function Calls"
        ```python linenums="1"
        def log(func):
            @wraps(func)
            def wrapper(*args, **kwargs):
                print(f"{func.__name__} called with {args}")
                return func(*args, **kwargs)
            return wrapper
        ```    
    === "2. Access Control / Authorization"
        ```python linenums="1"
        def require_auth(func):
            @wraps(func)
            def wrapper(user):
                if not user.get("is_admin"):
                    print("Access denied!")
                    return
                return func(user)
            return wrapper

        @require_auth
        def view_dashboard(user):
            print("Welcome Admin!")

        view_dashboard({"username": "john", "is_admin": False})
        ```

    === "3. Measuring Execution Time"
        ```python linenums="1"
        import time

        def timer(func):
            @wraps(func)
            def wrapper(*args, **kwargs):
                start = time.time()
                result = func(*args, **kwargs)
                print(f"{func.__name__} took {time.time() - start:.4f}s")
                return result
            return wrapper
        ```
