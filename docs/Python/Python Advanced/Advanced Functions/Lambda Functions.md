# Lambda Functions

## 1. What is a Lambda Function?

A **lambda function** in Python is a small anonymous function defined using the `lambda` keyword. Unlike regular functions defined with `def`, a lambda function is created on the fly and is often used when a function is needed for a short period of time, typically as an argument to higher-order functions (like `map`, `filter`, and `sorted`).

The syntax of a lambda function is:
```python
lambda arguments: expression
```

- **arguments**: One or more parameters that the function will take.  
- **expression**: A single expression that will be evaluated and returned. The expression cannot contain statements or assignments.  

## 2. Basic Example of Lambda Function

Here’s a basic example of a lambda function that adds two numbers:

```python
add = lambda x, y: x + y
print(add(5, 3))  # Output: 8
```
In this example, `lambda x, y: x + y` is a lambda function that takes two arguments `x` and `y`, and returns their sum.

## 3. Lambda Functions in Higher-Order Functions

Lambda functions are commonly used with built-in functions like `map()`, `filter()`, and `sorted()`.

!!! note "Examples"
    === "a. Using `map()`"
        The `map()` function applies a given function to all items in an iterable (e.g., list).

        Example:
        ```python linenums="1"
        numbers = [1, 2, 3, 4, 5]
        squared = list(map(lambda x: x**2, numbers))
        print(squared)  # Output: [1, 4, 9, 16, 25]
        ```
        In this example, `lambda x: x**2` is used to square each number in the list.

    === "b. Using `filter()`"
        The `filter()` function filters elements of an iterable based on a condition defined by a function.

        Example:
        ```python linenums="1"
        numbers = [1, 2, 3, 4, 5, 6]
        even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
        print(even_numbers)  # Output: [2, 4, 6]
        ```
        Here, the lambda function `lambda x: x % 2 == 0` checks if a number is even.
        
    === "c. Using `sorted()`"
        The `sorted()` function can take a key argument, which is often a lambda function, to sort the elements based on a specific criterion.

        Example:
        ```python linenums="1"
        data = [(1, 'apple'), (3, 'banana'), (2, 'cherry')]
        sorted_data = sorted(data, key=lambda x: x[1])
        print(sorted_data)  # Output: [(1, 'apple'), (3, 'banana'), (2, 'cherry')]
        ```
        In this case, the list of tuples is sorted by the second element (the fruit name) using the lambda function `lambda x: x[1]`.


## 4. Lambda Functions with Default Arguments

You can also define lambda functions with default arguments. Here’s an example:

```python linenums="1"
greet = lambda name="Guest": f"Hello, {name}!"
print(greet())           # Output: Hello, Guest!
print(greet("Alice"))    # Output: Hello, Alice!
```

## 5. Lambda Functions with Multiple Arguments

Lambda functions can take multiple arguments. Here’s an example of a lambda function that calculates the product of three numbers:

```python linenums="1"
product = lambda x, y, z: x * y * z
print(product(2, 3, 4))  # Output: 24
```

## 6. Lambda Functions in List Comprehensions

You can combine lambda functions with list comprehensions to perform more complex operations in a concise way.

Example:
```python linenums="1"
numbers = [1, 2, 3, 4, 5]
doubled = [lambda x: x * 2 for x in numbers]
print([f(2) for f in doubled])  # Output: [2, 4, 6, 8, 10]
```

## 7. Why Use Lambda Functions?

- **Conciseness**: Lambda functions allow you to write small, one-off functions in a single line, making your code more concise and easier to read when the function is simple.
- **Functional Programming**: Lambda functions are widely used in functional programming paradigms, where functions are often passed as arguments to other functions.

## 8. When Not to Use Lambda Functions

- **Readability**: If the lambda function is complex or spans multiple lines, it’s better to define a regular function using `def`. It improves readability and makes debugging easier.
- **Lack of Name**: Since lambda functions are anonymous, it’s harder to debug them as they don’t have meaningful names.

## 9. Lambda Functions in Classes
Although lambda functions are often used for quick tasks, they can also be used within classes, especially as short methods or as part of custom sorting or filtering functions.

Example:
```python linenums="1"
class MathOps:
    def __init__(self):
        self.add = lambda x, y: x + y
        self.multiply = lambda x, y: x * y

math_ops = MathOps()
print(math_ops.add(5, 3))      # Output: 8
print(math_ops.multiply(2, 4)) # Output: 8
```

## 10. Conclusion

Lambda functions are a powerful feature in Python for creating concise, functional code. While they’re great for short, throwaway functions, you should avoid using them in more complex situations where readability is important.
