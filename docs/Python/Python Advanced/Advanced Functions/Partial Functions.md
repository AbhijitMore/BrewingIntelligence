# Partial Functions: `functools.partial`

## 1. What is a Partial Function?
In Python, a partial function is a function that has some of its arguments fixed (predefined) at the time of creation, allowing you to create specialized versions of a function. This is done using the `functools.partial` function.

When you call a partial function, the arguments you provide will be combined with the fixed arguments to generate the original function's result.

## 2. Why Use Partial Functions?
Partial functions are useful in scenarios where you need to pass around functions with some arguments already filled in, and it can be especially helpful in functional programming paradigms.

Some common use cases include:  
1. **Simplifying repetitive function calls** by fixing some parameters.  
2. **Creating custom handlers** for events or callbacks.  
3. **Functional programming** techniques where higher-order functions are involved.  

## 3. `functools.partial` Overview
The `functools.partial` function is used to create a new function with some default arguments preset.

### Syntax of `functools.partial`
```python
functools.partial(func, *args, **kwargs)
```

- `func`: The original function.  
- `*args`: Positional arguments to pre-fill.  
- `**kwargs`: Keyword arguments to pre-fill.  

!!! note "Examples"
    === "Basic Usage of `partial`"
        Let’s say we have a simple function that takes two arguments:

        ```python linenums="1"
        def multiply(x, y):
            return x * y
        ```

        We can create a partial function that always multiplies by 2:

        ```python linenums="1"
        from functools import partial

        # Create a partial function where x is always 2
        double = partial(multiply, 2)

        # Now we can just pass the second argument (y)
        print(double(4))  # Output: 8
        ```

        In the example above, we created a partial function `double`,  
        which is equivalent to calling `multiply(2, y)` where `y` is the argument we provide.  
        So, `double(4)` is equivalent to `multiply(2, 4)`.  

    === "Using `partial` with Keyword Arguments"
        Now let's look at a function that takes both positional and keyword arguments:

        ```python linenums="1"
        def greet(name, greeting="Hello"):
            return f"{greeting}, {name}!"
        ```

        You can create a partial function where the greeting is always "Hi":

        ```python
        from functools import partial

        # Create a partial function where greeting is always "Hi"
        hi_greet = partial(greet, greeting="Hi")

        # Now we can just pass the name
        print(hi_greet("Alice"))  # Output: Hi, Alice!
        ```

        In this example, `hi_greet` is a function that always uses the greeting `"Hi"`,  
        and we only need to pass the name.

    === "Pre-filling Multiple Arguments"
        You can pre-fill multiple arguments as well. Here’s an example where you can pre-fill both the positional and keyword arguments:

        ```python linenums="1"
        from functools import partial

        # A function that takes two arguments
        def power(base, exponent):
            return base ** exponent

        # Partial function with base pre-filled to 2
        square = partial(power, 2)

        # Now you can just provide the exponent
        print(square(3))  # Output: 8 (2^3)
        ```

        Here, `square` is equivalent to calling `power(2, exponent)` where `exponent` is the argument you provide.

    === "Partial Functions in Callbacks"
        Partial functions are often used in situations where a function is passed as a callback, and some arguments need to be fixed. For example:

        ```python linenums="1"
        import tkinter as tk
        from functools import partial

        def on_button_click(message, times):
            print(message * times)

        root = tk.Tk()
        button = tk.Button(root, text="Click me", command=partial(on_button_click, "Hello", 3))
        button.pack()

        root.mainloop()
        ```

        In this case, `on_button_click` expects two arguments,  
        but using `partial`, we create a new function that is only called when the button is clicked,  
        and the arguments `"Hello"` and `3` are pre-filled.  

    === "`partial` with Functions with Default Arguments"
        You can use `partial` in conjunction with functions that already have default values.

        ```python
        def subtract(a, b=5):
            return a - b

        # Pre-fill b to 10, leaving a to be provided
        minus_ten = partial(subtract, b=10)

        print(minus_ten(20))  # Output: 10 (20 - 10)
        ```

        Here, the argument `b` is fixed to 10, and when calling `minus_ten(20)`,  
        the function behaves as `subtract(20, 10)`.  

    === "The `functools.partial` with Class Methods"
        You can also use `functools.partial` with class methods. Here's an example using a class:

        ```python linenums="1"
        class Multiplier:
            def __init__(self, factor):
                self.factor = factor

            def multiply(self, x):
                return self.factor * x

        # Create a partial function to multiply by 3
        multiplier_by_three = partial(Multiplier(3).multiply)

        print(multiplier_by_three(10))  # Output: 30
        ```

        In this example, the `Multiplier` class has a method `multiply` that takes an argument `x`.  
        By using `partial`, we create a function where the factor is always 3.  

## 4. Benefits of Using `functools.partial`
1. **Cleaner Code**: You avoid repetitive function calls by pre-filling some arguments.
2. **Higher-order Functions**: You can pass around partially applied functions as arguments.
3. **Improved Readability**: Pre-filled arguments make it clear what the function will do with less boilerplate code.
4. **Event Handling**: It is useful in GUI or event-driven programming to simplify callbacks.

## 5. When to Use `functools.partial`
- **Event-driven programming**: For example, GUI programming where callbacks need arguments pre-filled.
- **When passing functions as arguments**: Partial functions allow you to create specialized functions for use as arguments to higher-order functions.
- **Functional programming**: When working with higher-order functions or in systems that require immutability.

## 6. Conclusion
`functools.partial` is a powerful tool for functional programming in Python. It allows you to simplify your functions by pre-filling arguments and creating specialized functions. This makes your code more modular, easier to maintain, and reusable.

With this knowledge, you should be able to handle most use cases involving partial functions and integrate them into your projects seamlessly!