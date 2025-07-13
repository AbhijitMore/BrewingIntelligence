# Parameter Unpacking: `*args`, `**kwargs`

In Python, functions can accept a variable number of arguments using `*args` and `**kwargs`. These allow for more flexible and dynamic function definitions.

## 1. Understanding `*args` (Non-keyworded Variable Arguments)

- `*args` allows a function to accept any number of **positional arguments**.  
- It collects extra positional arguments into a **tuple**.  
**Example:**  
```python linenums="1"
def print_args(*args):
    for arg in args:
        print(arg)

print_args(1, 2, 3, 'hello', True)
```
**Output:**
```
1
2
3
hello
True
```
- `*args` is just a convention. You can name it anything.  
- It must be placed after all regular positional parameters in a function definition.  

## 2. Understanding `**kwargs` (Keyworded Variable Arguments)

- `**kwargs` allows a function to accept any number of **keyword arguments**.  
- It collects them into a **dictionary**.  
**Example:**  
```python linenums="1"
def print_kwargs(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_kwargs(name="Abhijit", age=25, profession="Engineer")
```
**Output:**
```
name: Abhijit
age: 25
profession: Engineer
```
- `**kwargs` is also a convention and can be renamed.  
- It should be the last parameter in the function signature if used with others.

## 3. Combining `*args` and `**kwargs`

You can combine both in one function. Just remember: `*args` must come before `**kwargs`.
**Example:**
```python linenums="1"
def show_details(name, *args, **kwargs):
    print(f"Name: {name}")
    print("Additional Info (Positional Arguments):")
    for arg in args:
        print(arg)
    print("Additional Info (Keyword Arguments):")
    for key, value in kwargs.items():
        print(f"{key}: {value}")

show_details("Abhijit", 30, "Engineer", country="India", city="Mumbai")
```
**Output:**  
```
Name: Abhijit
Additional Info (Positional Arguments):
30
Engineer
Additional Info (Keyword Arguments):
country: India
city: Mumbai
```

## 4. Using `*args` and `**kwargs` in Function Calls

You can unpack arguments from lists/tuples and dictionaries using `*` and `**` during function calls.
**Example:**
```python linenums="1"
def greet(name, age):
    print(f"Hello, my name is {name} and I am {age} years old.")

def greet_all(*args, **kwargs):
    for person in args:
        greet(*person)

people = [("Alice", 30), ("Bob", 25), ("Charlie", 35)]
greet_all(*people)
```

**Output:**
```
Hello, my name is Alice and I am 30 years old.
Hello, my name is Bob and I am 25 years old.
Hello, my name is Charlie and I am 35 years old.
```

## 5. Using `*args` and `**kwargs` with Default Parameters

You can mix `*args` and `**kwargs` with regular/default parameters.
**Example:**
```python linenums="1"
def introduce(name, age=25, *args, **kwargs):
    print(f"Name: {name}, Age: {age}")
    print("Additional Info:")
    for arg in args:
        print(arg)
    for key, value in kwargs.items():
        print(f"{key}: {value}")

introduce("Abhijit", 30, "Engineer", country="India", city="Mumbai")
```
**Output:**
```
Name: Abhijit, Age: 30
Additional Info:
Engineer
country: India
city: Mumbai
```

## 6. Summary
- `*args`: Collects extra **positional** arguments into a tuple.  
- `**kwargs`: Collects extra **keyword** arguments into a dictionary.  
- You can use both together in function definitions and calls.  
- They provide flexibility and scalability to function definitions.  