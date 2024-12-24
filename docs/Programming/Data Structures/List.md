
## Python way
```python linenums="1"
# 1. Creating Lists
my_list = [1, 2, 3, 4, 5]
empty_list = []

# 2. Accessing elements
first_item = my_list[0]  # First element
last_item = my_list[-1]  # Last element
sub_list = my_list[1:4]  # Slice from index 1 to 3 (4 exclusive)

# 3. Basic List Operations
# Concatenation
concatenated_list = my_list + [6, 7, 8]

# Repetition
repeated_list = [1, 2] * 3  # Output: [1, 2, 1, 2, 1, 2]

# Length of a list
length = len(my_list)

# Membership Test
is_present = 3 in my_list  # Returns True if 3 is in my_list

# 4. List Methods
# append()
my_list.append(6)  # List becomes [1, 2, 3, 4, 5, 6]

# extend()
my_list.extend([7, 8, 9])  # List becomes [1, 2, 3, 4, 5, 6, 7, 8, 9]

# insert()
my_list.insert(2, 10)  # List becomes [1, 2, 10, 3, 4, 5, 6, 7, 8, 9]

# remove()
my_list.remove(10)  # List becomes [1, 2, 3, 4, 5, 6, 7, 8, 9]

# pop()
popped_item = my_list.pop()  # Removes and returns last element (9)

# index()
index_of_4 = my_list.index(4)  # Returns index of first occurrence of 4

# count()
count_of_3 = my_list.count(3)  # Returns number of times 3 appears

# sort() and reverse()
my_list.sort()  # Sorts the list in ascending order
my_list.reverse()  # Reverses the list

# copy()
my_list_copy = my_list.copy()  # Copies the list

# clear()
my_list.clear()  # Empties the list

# 5. List Comprehensions
# Basic list comprehension
squared_numbers = [x**2 for x in range(5)]  # Output: [0, 1, 4, 9, 16]

# List comprehension with condition
even_numbers = [x for x in range(10) if x % 2 == 0]  # Output: [0, 2, 4, 6, 8]

# Nested list comprehension
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened = [item for sublist in matrix for item in sublist]  # Output: [1, 2, 3, 4, 5, 6, 7, 8, 9]

# 6. Built-in Functions
# map() – Applies a function to each item
squared = list(map(lambda x: x**2, [1, 2, 3]))  # Output: [1, 4, 9]

# filter() – Filters elements based on a condition
even_numbers = list(filter(lambda x: x % 2 == 0, [1, 2, 3, 4]))  # Output: [2, 4]

# sorted() – Returns a new sorted list
sorted_list = sorted([5, 3, 9, 1])  # Output: [1, 3, 5, 9]

# min() and max() – Return the smallest and largest element
smallest = min([5, 3, 9, 1])  # Output: 1
largest = max([5, 3, 9, 1])   # Output: 9

# sum() – Sums all elements in the list
total = sum([1, 2, 3, 4])  # Output: 10

# any() – Returns True if any element is True (non-zero, non-empty)
any_true = any([0, 1, 2])  # Output: True

# all() – Returns True if all elements are True
all_true = all([1, 2, 3])  # Output: True

# 7. Advanced List Operations
# Flatten a list of lists
list_of_lists = [[1, 2], [3, 4], [5, 6]]
flattened = [item for sublist in list_of_lists for item in sublist]  # Output: [1, 2, 3, 4, 5, 6]

# Find intersection of two lists
list1 = [1, 2, 3, 4]
list2 = [3, 4, 5, 6]
intersection = list(set(list1) & set(list2))  # Output: [3, 4]

# Find union of two lists
union = list(set(list1) | set(list2))  # Output: [1, 2, 3, 4, 5, 6]

# 8. More Complex Operations
# Zipping two lists
list_a = [1, 2, 3]
list_b = ['a', 'b', 'c']
zipped = list(zip(list_a, list_b))  # Output: [(1, 'a'), (2, 'b'), (3, 'c')]

# Unzipping a list of tuples
unzipped_a, unzipped_b = zip(*zipped)  # Output: (1, 2, 3), ('a', 'b', 'c')
```

## Implementation of basic functions
```python linenums="1"
class CustomList:
    def __init__(self, elements=None):
        self.elements = elements if elements else []

    def append(self, item):
        """Add an item to the end of the list."""
        self.elements += [item]

    def extend(self, iterable):
        """Extend the list by appending all the items from the iterable."""
        for item in iterable:
            self.append(item)

    def insert(self, index, item):
        """Insert an item at a given position."""
        self.elements = self.elements[:index] + [item] + self.elements[index:]

    def remove(self, item):
        """Remove the first occurrence of a value."""
        index = 0
        found = False
        for value in self.elements:
            if value == item:
                found = True
                break
            index += 1
        if found:
            self.elements = self.elements[:index] + self.elements[index+1:]
        else:
            raise ValueError(f"{item} not in list")

    def pop(self, index=-1):
        """Remove the item at the given position in the list and return it."""
        if index < 0:
            index += self._length()
        if 0 <= index < self._length():
            item = self.elements[index]
            self.elements = self.elements[:index] + self.elements[index+1:]
            return item
        raise IndexError("pop index out of range")

    def clear(self):
        """Remove all items from the list."""
        self.elements = []

    def index(self, item, start=0, end=None):
        """Return the index of the first occurrence of the value."""
        end = end if end is not None else self._length()
        current_index = 0
        for value in self.elements:
            if start <= current_index < end and value == item:
                return current_index
            current_index += 1
        raise ValueError(f"{item} not in list")

    def count(self, item):
        """Return the number of occurrences of the value."""
        count = 0
        for value in self.elements:
            if value == item:
                count += 1
        return count

    def reverse(self):
        """Reverse the list in place."""
        self.elements = self.elements[::-1]

    def sort(self, key=None, reverse=False):
        """Sort the list in ascending order."""
        def default_key(x): return x
        key = key if key else default_key

        # Implement bubble sort as an example
        n = self._length()
        for i in range(n):
            for j in range(0, n-i-1):
                if (key(self.elements[j]) > key(self.elements[j+1])) == (not reverse):
                    self.elements[j], self.elements[j+1] = self.elements[j+1], self.elements[j]

    def copy(self):
        """Return a shallow copy of the list."""
        return CustomList(self.elements[:])

    def _length(self):
        """Calculate the length of the list."""
        count = 0
        for _ in self.elements:
            count += 1
        return count

    def __getitem__(self, index):
        """Get an item by its index."""
        return self.elements[index]

    def __setitem__(self, index, value):
        """Set an item at the specified index."""
        self.elements[index] = value

    def __delitem__(self, index):
        """Delete an item at the specified index."""
        self.elements = self.elements[:index] + self.elements[index+1:]

    def __repr__(self):
        """Return a string representation of the list."""
        return repr(self.elements)

    def __len__(self):
        """Override len() to use our custom _length method."""
        return self._length()


# Example usage
custom_list = CustomList([1, 2, 3])
custom_list.append(4)
custom_list.extend([5, 6])
custom_list.insert(2, 10)
custom_list.remove(2)
popped_item = custom_list.pop()
print("Popped Item:", popped_item)
custom_list.clear()
print(custom_list)  # Output: []
```