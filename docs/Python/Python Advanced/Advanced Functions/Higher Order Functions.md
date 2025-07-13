# Higher Order Functions

## 1. What Are Higher-Order Functions?
A Higher-Order Function (HOF) is a function that either:  
- Takes one or more functions as arguments  
- Returns a function as its result  
They are a fundamental concept in functional programming and are supported in Python because functions are first-class citizens.  

!!! note "Examples"
    === "Passing Functions as Arguments"
        ```python linenums="1"
        def greet(name):
        return f"Hello, {name}!"

        def process_name(func, name):
            return func(name)

        print(process_name(greet, "Abhijit"))  # Output: Hello, Abhijit!
        ```
        **Exaplaination:**  
        greet is passed to process_name as an argument.  
        process_name calls func(name) → greet("Abhijit").  
    === "Returning a Function"
        ```python linenums="1"
        def outer(msg):
        def inner():
            print(f"Message: {msg}")
        return inner

        say_hello = outer("Hello")
        say_hello()  # Output: Message: Hello
        ```

## 2. Why Use Higher-Order Functions?  
Promotes code reuse and modularity  
Helps implement callbacks, decorators, and functional patterns  
Clean and concise for data processing (e.g., using map, filter, etc.)  

## 3. Common Built-in Higher-Order Functions in Python

=== "1. map(function, iterable)"
    ```python linenums="1"
    nums = [1, 2, 3, 4]
    squared = list(map(lambda x: x**2, nums))
    print(squared)
    # Output:
    # [1, 4, 9, 16]
    ```
=== "2. filter(function, iterable)"
    ```python linenums="1"
    nums = [1, 2, 3, 4, 5]
    even = list(filter(lambda x: x % 2 == 0, nums))
    print(even)
    # Output:
    # [2, 4]
    ```
=== "3. reduce(function, iterable)"
    ```python linenums="1"
    from functools import reduce

    nums = [1, 2, 3, 4]
    total = reduce(lambda x, y: x + y, nums)
    print(total)
    # Output:
    # 10
    ```
=== "4. sorted(iterable, key=func)"
    ```python linenums="1"
    names = ["john", "DOE", "alice"]
    sorted_names = sorted(names, key=lambda s: s.lower())
    print(sorted_names)
    # Output:
    # ['alice', 'DOE', 'john']
    ```
## 4. Summary

| Concept              | Description                                        |
|----------------------|----------------------------------------------------|
| Higher-Order Function| Takes or returns another function                  |
| `map()`              | Applies function to all items in iterable         |
| `filter()`           | Filters items based on a condition                |
| `reduce()`           | Reduces list to a single value                    |
| Closures             | Functions that remember the enclosing environment |
| Decorators           | Functions that modify other functions             |

!!! note "Practice Ideas"
    === "1. Write a `custom_map` function like Python’s built-in `map`."
        ```python linenums="1"
        def custom_map(func, iterable):
            result = []
            for item in iterable:
                result.append(func(item))
            return result

        # Example usage
        nums = [1, 2, 3, 4]
        squared = custom_map(lambda x: x**2, nums)
        print(squared)  # Output: [1, 4, 9, 16]
        ```
        **Explanation:**  
        This custom function replicates Python’s `map`.  
        It takes a function and an iterable, applies the function to each item, and collects the results in a list.   

    === "2. Create a decorator that times how long a function takes"
        ```python linenums="1"
        import time

        def timer(func):
            def wrapper(*args, **kwargs):
                start = time.time()
                result = func(*args, **kwargs)
                end = time.time()
                print(f"Execution time: {end - start:.6f} seconds")
                return result
            return wrapper

        @timer
        def slow_function():
            time.sleep(1)
            print("Finished sleeping")

        slow_function()
        ```

        **Explanation:**  
        This decorator wraps the original function and measures how long it takes to run using `time.time()`.  
         It is useful for performance profiling.  
    === "3. Combine `map`, `filter`, and `reduce` to process data"
        ```python linenums="1"
        from functools import reduce

        nums = [1, 2, 3, 4, 5, 6]

        # Step 1: Filter even numbers
        evens = filter(lambda x: x % 2 == 0, nums)

        # Step 2: Square the filtered numbers
        squares = map(lambda x: x ** 2, evens)

        # Step 3: Sum the squared numbers
        result = reduce(lambda x, y: x + y, squares)

        print(result)  # Output: 56
        ```
        **Explanation:**  
        This chain of higher-order functions first filters even numbers from the list,  
        then squares them, and finally sums them using `reduce`.  
        The result is the sum of squares of even numbers: `2² + 4² + 6² = 4 + 16 + 36 = 56`.  
