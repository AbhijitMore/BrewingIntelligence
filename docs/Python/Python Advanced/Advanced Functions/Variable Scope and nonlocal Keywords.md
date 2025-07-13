# Variable Scope and <i>`nonlocal`</i> Keyword

## 1. Understanding Python Variable Scope

In Python, **scope** refers to the region of a program where a variable is recognized.
Python follows the **LEGB rule** for scope resolution:

| Scope Level      | Meaning                          | Example |
|------------------|----------------------------------|---------|
| **L**: Local      | Inside the current function       | Variable in a function |
| **E**: Enclosing  | Inside any enclosing function(s)  | Nested function variables |
| **G**: Global     | At the top-level of the script    | Declared outside all functions |
| **B**: Built-in   | Python's built-in names           | `len`, `sum`, etc. |

!!! note "Python Variable Scope Examples"
    === "1. Local Scope"
        ```python linenums="1"
        def func():
            x = 10  # Local variable
            print(x)

        func()
        # Output: 10
        ```
    === "2. Global Scope"
        ```python linenums="1"
        x = 5  # Global variable

        def show():
            print(x)

        show()
        # Output: 5
        ```
    === "3. Enclosing Scope (Nested Functions)"
        ```python linenums="1"
        def outer():
            y = 20  # Enclosing variable

            def inner():
                print(y)  # Accessing enclosing variable

            inner()

        outer()
        # Output: 20
        ```
    === "4. Built-in Scope"
        ```python linenums="1"
        print(len("hello"))  # 'len' is a built-in function
        # Output: 5
        ```

### Variable Lookup Order (LEGB)

If a variable is referenced inside a function, Python checks in the following order:  
1. **Local**  
2. **Enclosing**  
3. **Global**  
4. **Built-in**  

## 2. `global` Keyword

Use `global` to modify a **global variable** inside a function.
    ```python linenums="1"
    x = 10

    def change():
        global x
        x = 20

    change()
    print(x)
    # Output: 20
    ```

## 3. `nonlocal` Keyword

The `nonlocal` keyword is used **inside a nested function** to modify a variable in the **enclosing (non-global) scope**.

!!! note "Examples"
    === "Example 1"
        ```python
        def outer():
            x = "outer variable"

            def inner():
                nonlocal x
                x = "modified by inner"

            inner()
            print(x)

        outer()
        # Output: modified by inner
        ```
    === "Example 2"
        ```python
        def counter():
            count = 0

            def increment():
                nonlocal count
                count += 1
                return count

            return increment

        my_counter = counter()
        print(my_counter())  # 1
        print(my_counter())  # 2
        ```

## 4. `nonlocal` vs `global`

| Feature      | `global`                  | `nonlocal`                         |
|--------------|----------------------------|------------------------------------|
| Scope        | Refers to global scope     | Refers to nearest enclosing scope  |
| Use case     | Modify global variable     | Modify enclosing function variable |
| Used in      | Any function               | Nested functions                   |

## 5. Summary

| Concept       | Description |
|----------------|-------------|
| **Scope**      | Region where a variable is accessible |
| **LEGB Rule**  | Lookup order: Local → Enclosing → Global → Built-in |
| **global**     | Refers to and modifies global variable |
| **nonlocal**   | Refers to and modifies enclosing variable in nested function |
