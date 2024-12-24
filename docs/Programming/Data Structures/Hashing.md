## Introduction

## Hash Collisions
### Chaining
```python linenums="1"
class HashTableChaining:
    def __init__(self, size=11):
        self.size = size
        self.table = [None] * self.size

    # Hash function
    def hash(self, key):
        return hash(key) % self.size

    # Insert a key-value pair (chaining)
    def insert(self, key, value):
        idx = self.hash(key)
        if self.table[idx] is None:
            self.table[idx] = [(key, value)]  # Initialize a new chain (linked list)
        else:
            self.table[idx].append((key, value))  # Append to the existing chain

    # Retrieve a value by key (chaining)
    def get(self, key):
        idx = self.hash(key)
        if self.table[idx] is not None:
            for pair in self.table[idx]:
                if pair[0] == key:
                    return pair[1]
        return None

    # Remove a key-value pair (chaining)
    def remove(self, key):
        idx = self.hash(key)
        if self.table[idx] is not None:
            for i, pair in enumerate(self.table[idx]):
                if pair[0] == key:
                    del self.table[idx][i]
                    return True
        return False

    # Display hash table
    def display(self):
        for idx, chain in enumerate(self.table):
            if chain is not None:
                print(f"Index {idx}: {chain}")
            else:
                print(f"Index {idx}: Empty")
    

# Example Usage (Chaining)
ht_chaining = HashTableChaining()
ht_chaining.insert("apple", 10)
ht_chaining.insert("banana", 20)
ht_chaining.insert("cherry", 30)

print(ht_chaining.get("apple"))    # Output: 10
print(ht_chaining.get("banana"))   # Output: 20
print(ht_chaining.get("cherry"))   # Output: 30

ht_chaining.remove("banana")
print(ht_chaining.get("banana"))   # Output: None

ht_chaining.display()
```
### Open Addressing
#### Linear Probing
```python linenums="1"
class HashTableLinearProbing:
    def __init__(self, size=11):
        self.size = size
        self.table = [None] * self.size
        self.count = 0

    # Primary hash function
    def hash(self, key):
        return hash(key) % self.size

    # Insert using Linear Probing
    def insert(self, key, value):
        if self.count / self.size >= 0.7:
            self._resize()
        idx = self.hash(key)
        original_idx = idx
        while self.table[idx] is not None:
            if self.table[idx][0] == key:
                self.table[idx] = (key, value)  # Update existing key
                return
            idx = (idx + 1) % self.size
            if idx == original_idx:
                raise Exception("Hash table is full!")
        self.table[idx] = (key, value)
        self.count += 1

    # Get value using Linear Probing
    def get(self, key):
        idx = self.hash(key)
        original_idx = idx
        while self.table[idx] is not None:
            if self.table[idx][0] == key:
                return self.table[idx][1]
            idx = (idx + 1) % self.size
            if idx == original_idx:
                break
        return None

    # Remove a key-value pair using Linear Probing
    def remove(self, key):
        idx = self.hash(key)
        original_idx = idx
        while self.table[idx] is not None:
            if self.table[idx][0] == key:
                self.table[idx] = None
                self.count -= 1
                self._rehash_after_removal(idx)
                return True
            idx = (idx + 1) % self.size
            if idx == original_idx:
                break
        return False

    # Resize the table when load factor exceeds threshold
    def _resize(self):
        new_size = self._next_prime(self.size * 2)
        new_table = [None] * new_size
        old_table = self.table
        self.table = new_table
        self.size = new_size
        self.count = 0

        for item in old_table:
            if item is not None:
                key, value = item
                self.insert(key, value)

    # Helper functions for resizing
    def _rehash_after_removal(self, idx):
        next_idx = (idx + 1) % self.size
        while self.table[next_idx] is not None:
            key, value = self.table[next_idx]
            self.table[next_idx] = None
            self.count -= 1
            self.insert(key, value)  # Rehash the removed items
            next_idx = (next_idx + 1) % self.size

    def _next_prime(self, n):
        while not self._is_prime(n):
            n += 1
        return n

    def _is_prime(self, num):
        if num < 2:
            return False
        for i in range(2, int(num ** 0.5) + 1):
            if num % i == 0:
                return False
        return True


# Example Usage (Linear Probing)
ht_linear = HashTableLinearProbing()
ht_linear.insert("apple", 10)
ht_linear.insert("banana", 20)
print(ht_linear.get("apple"))    # Output: 10
print(ht_linear.get("banana"))   # Output: 20
ht_linear.remove("apple")
print(ht_linear.get("apple"))    # Output: None
```
#### Quadratic Probing
```python linenums="1"
class HashTableQuadraticProbing:
    def __init__(self, size=11):
        self.size = size
        self.table = [None] * self.size
        self.count = 0

    # Primary hash function
    def hash(self, key):
        return hash(key) % self.size

    # Insert using Quadratic Probing
    def insert(self, key, value):
        if self.count / self.size >= 0.7:
            self._resize()
        idx = self.hash(key)
        i = 0
        while self.table[(idx + i * i) % self.size] is not None:
            if self.table[(idx + i * i) % self.size][0] == key:
                self.table[(idx + i * i) % self.size] = (key, value)
                return
            i += 1
        self.table[(idx + i * i) % self.size] = (key, value)
        self.count += 1

    # Get value using Quadratic Probing
    def get(self, key):
        idx = self.hash(key)
        i = 0
        while self.table[(idx + i * i) % self.size] is not None:
            if self.table[(idx + i * i) % self.size][0] == key:
                return self.table[(idx + i * i) % self.size][1]
            i += 1
        return None

    # Remove a key-value pair using Quadratic Probing
    def remove(self, key):
        idx = self.hash(key)
        i = 0
        while self.table[(idx + i * i) % self.size] is not None:
            if self.table[(idx + i * i) % self.size][0] == key:
                self.table[(idx + i * i) % self.size] = None
                self.count -= 1
                self._rehash_after_removal((idx + i * i) % self.size)
                return True
            i += 1
        return False

    # Resize the table when load factor exceeds threshold
    def _resize(self):
        new_size = self._next_prime(self.size * 2)
        new_table = [None] * new_size
        old_table = self.table
        self.table = new_table
        self.size = new_size
        self.count = 0

        for item in old_table:
            if item is not None:
                key, value = item
                self.insert(key, value)

    # Helper functions for resizing
    def _rehash_after_removal(self, idx):
        next_idx = (idx + 1) % self.size
        while self.table[next_idx] is not None:
            key, value = self.table[next_idx]
            self.table[next_idx] = None
            self.count -= 1
            self.insert(key, value)  # Rehash the removed items
            next_idx = (next_idx + 1) % self.size

    def _next_prime(self, n):
        while not self._is_prime(n):
            n += 1
        return n

    def _is_prime(self, num):
        if num < 2:
            return False
        for i in range(2, int(num ** 0.5) + 1):
            if num % i == 0:
                return False
        return True


# Example Usage (Quadratic Probing)
ht_quadratic = HashTableQuadraticProbing()
ht_quadratic.insert("apple", 10)
ht_quadratic.insert("banana", 20)
print(ht_quadratic.get("apple"))    # Output: 10
print(ht_quadratic.get("banana"))   # Output: 20
ht_quadratic.remove("apple")
print(ht_quadratic.get("apple"))    # Output: None
```
#### Double Hashing
```python linenums="1"
class HashTableDoubleHashing:
    def __init__(self, size=11):
        self.size = size
        self.table = [None] * self.size
        self.count = 0

    # Primary hash function
    def hash(self, key):
        return hash(key) % self.size

    # Secondary hash function (used for double hashing)
    def secondary_hash(self, key):
        return 7 - (hash(key) % 7)  # A prime number for secondary hash

    # Insert using Double Hashing
    def insert(self, key, value):
        if self.count / self.size >= 0.7:
            self._resize()
        idx = self.hash(key)
        step = self.secondary_hash(key)
        original_idx = idx
        while self.table[idx] is not None:
            if self.table[idx][0] == key:
                self.table[idx] = (key, value)
                return
            idx = (idx + step) % self.size
            if idx == original_idx:
                raise Exception("Hash table is full!")
        self.table[idx] = (key, value)
        self.count += 1

    # Get value using Double Hashing
    def get(self, key):
        idx = self.hash(key)
        step = self.secondary_hash(key)
        original_idx = idx
        while self.table[idx] is not None:
            if self.table[idx][0] == key:
                return self.table[idx][1]
            idx = (idx + step) % self.size
            if idx == original_idx:
                break
        return None

    # Remove a key-value pair using Double Hashing
    def remove(self, key):
        idx = self.hash(key)
        step = self.secondary_hash(key)
        original_idx = idx
        while self.table[idx] is not None:
            if self.table[idx][0] == key:
                self.table[idx] = None
                self.count -= 1
                self._rehash_after_removal(idx)
                return True
            idx = (idx + step) % self.size
            if idx == original_idx:
                break
        return False

    # Resize the table when load factor exceeds threshold
    def _resize(self):
        new_size = self._next_prime(self.size * 2)
        new_table = [None] * new_size
        old_table = self.table
        self.table = new_table
        self.size = new_size
        self.count = 0

        for item in old_table:
            if item is not None:
                key, value = item
                self.insert(key, value)

    # Helper functions for resizing
    def _rehash_after_removal(self, idx):
        next_idx = (idx + 1) % self.size
        while self.table[next_idx] is not None:
            key, value = self.table[next_idx]
            self.table[next_idx] = None
            self.count -= 1
            self.insert(key, value)  # Rehash the removed items
            next_idx = (next_idx + 1) % self.size

    def _next_prime(self, n):
        while not self._is_prime(n):
            n += 1
        return n

    def _is_prime(self, num):
        if num < 2:
            return False
        for i in range(2, int(num ** 0.5) + 1):
            if num % i == 0:
                return False
        return True


# Example Usage (Double Hashing)
ht_double = HashTableDoubleHashing()
ht_double.insert("apple", 10)
ht_double.insert("banana", 20)
print(ht_double.get("apple"))    # Output: 10
print(ht_double.get("banana"))   # Output: 20
ht_double.remove("apple")
print(ht_double.get("apple"))    # Output: None
```