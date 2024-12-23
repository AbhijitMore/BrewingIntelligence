

## MinHeap Implementation

```python linenums="1"
class MinHeap:
    def __init__(self, array=None):
        self.arr = []
        if array:
            self.buildHeap(array)

    def parent(self, i):
        return (i - 1) // 2

    def leftChild(self, i):
        return 2 * i + 1

    def rightChild(self, i):
        return 2 * i + 2

    def insert(self, x):
        self.arr.append(x)
        i = len(self.arr) - 1
        while i > 0 and self.arr[self.parent(i)] > self.arr[i]:
            self.arr[self.parent(i)], self.arr[i] = self.arr[i], self.arr[self.parent(i)]
            i = self.parent(i)

    def minHeapify(self, i):
        left = self.leftChild(i)
        right = self.rightChild(i)
        smallest = i

        if left < len(self.arr) and self.arr[left] < self.arr[smallest]:
            smallest = left
        if right < len(self.arr) and self.arr[right] < self.arr[smallest]:
            smallest = right

        if smallest != i:
            self.arr[i], self.arr[smallest] = self.arr[smallest], self.arr[i]
            self.minHeapify(smallest)

    def extractMin(self):
        if len(self.arr) == 0:
            raise IndexError("extractMin from empty heap")
        
        min_elem = self.arr[0]
        self.arr[0] = self.arr[-1]
        self.arr.pop()
        self.minHeapify(0)
        
        return min_elem

    def decreaseKey(self, i, x):
        if x > self.arr[i]:
            raise ValueError("New key is greater than the current key")
        self.arr[i] = x
        while i > 0 and self.arr[self.parent(i)] > self.arr[i]:
            self.arr[self.parent(i)], self.arr[i] = self.arr[i], self.arr[self.parent(i)]
            i = self.parent(i)

    def delete(self, i):
        self.decreaseKey(i, float('-inf'))
        self.extractMin()

    def buildHeap(self, array):
        self.arr = array
        # Start from the last non-leaf node and apply minHeapify to each node
        start_index = (len(self.arr) // 2) - 1
        for i in range(start_index, -1, -1):
            self.minHeapify(i)

    def __str__(self):
        return str(self.arr)

# Example usage:

# Step 1: Create a MinHeap instance with an unsorted array
arr = [10, 4, 15, 1, 7]
heap = MinHeap(arr)

# Step 2: Print the heap after building it
print("Heap after building:", heap.arr)

# Step 3: Insert more elements into the heap
heap.insert(5)
print("Heap after inserting 5:", heap.arr)

# Step 4: Extract the minimum element
min_val = heap.extractMin()
print("Extracted min:", min_val)

# Step 5: Decrease a key (reduce the value at index 2 to 3)
heap.decreaseKey(2, 3)
print("Heap after decreasing key:", heap.arr)

# Step 6: Delete an element at index 1
heap.delete(1)
print("Heap after deleting element at index 1:", heap.arr)
```

## MaxHeap Implementation
```python linenums="1"
class MaxHeap:
    def __init__(self, array=None):
        self.arr = []
        if array:
            self.buildHeap(array)

    def parent(self, i):
        return (i - 1) // 2

    def leftChild(self, i):
        return 2 * i + 1

    def rightChild(self, i):
        return 2 * i + 2

    def insert(self, x):
        self.arr.append(x)
        i = len(self.arr) - 1
        while i > 0 and self.arr[self.parent(i)] < self.arr[i]:
            self.arr[self.parent(i)], self.arr[i] = self.arr[i], self.arr[self.parent(i)]
            i = self.parent(i)

    def maxHeapify(self, i):
        left = self.leftChild(i)
        right = self.rightChild(i)
        largest = i

        if left < len(self.arr) and self.arr[left] > self.arr[largest]:
            largest = left
        if right < len(self.arr) and self.arr[right] > self.arr[largest]:
            largest = right

        if largest != i:
            self.arr[i], self.arr[largest] = self.arr[largest], self.arr[i]
            self.maxHeapify(largest)

    def extractMax(self):
        if len(self.arr) == 0:
            raise IndexError("extractMax from empty heap")

        max_elem = self.arr[0]
        self.arr[0] = self.arr[-1]
        self.arr.pop()
        self.maxHeapify(0)

        return max_elem

    def increaseKey(self, i, x):
        if x < self.arr[i]:
            raise ValueError("New key is smaller than the current key")
        self.arr[i] = x
        while i > 0 and self.arr[self.parent(i)] < self.arr[i]:
            self.arr[self.parent(i)], self.arr[i] = self.arr[i], self.arr[self.parent(i)]
            i = self.parent(i)

    def delete(self, i):
        self.increaseKey(i, float('inf'))
        self.extractMax()

    def buildHeap(self, array):
        self.arr = array
        # Start from the last non-leaf node and apply maxHeapify to each node
        start_index = (len(self.arr) // 2) - 1
        for i in range(start_index, -1, -1):
            self.maxHeapify(i)

    def __str__(self):
        return str(self.arr)

# Example usage:

# Step 1: Create a MaxHeap instance with an unsorted array
arr = [10, 4, 15, 1, 7]
heap = MaxHeap(arr)

# Step 2: Print the heap after building it
print("Heap after building:", heap.arr)

# Step 3: Insert more elements into the heap
heap.insert(20)
print("Heap after inserting 20:", heap.arr)

# Step 4: Extract the maximum element
max_val = heap.extractMax()
print("Extracted max:", max_val)

# Step 5: Increase a key (increase the value at index 2 to 18)
heap.increaseKey(2, 18)
print("Heap after increasing key:", heap.arr)

# Step 6: Delete an element at index 1
heap.delete(1)
print("Heap after deleting element at index 1:", heap.arr)
```

## heapq in python

```python linenums="1"
import heapq

# 1. heapq.heappush(heap, item)
# Adds item to the heap while maintaining heap property
heap = [1, 3, 5, 7, 9, 2]
heapq.heappush(heap, 4)
print("After heappush:", heap)
# Output: [1, 3, 2, 7, 9, 5, 4]

# 2. heapq.heappop(heap)
# Removes and returns the smallest item from the heap
smallest = heapq.heappop(heap)
print("Popped smallest:", smallest)  # Output: 1
print("After heappop:", heap)        # Output: [2, 3, 5, 7, 9]

# 3. heapq.heappushpop(heap, item)
# Pushes item to heap and pops and returns the smallest item
result = heapq.heappushpop(heap, 4)
print("Pushed and popped:", result)  # Output: 2
print("After heappushpop:", heap)    # Output: [3, 4, 5, 7, 9]

# 4. heapq.heapreplace(heap, item)
# Pops the smallest item and pushes item to heap
smallest = heapq.heapreplace(heap, 4)
print("Heap replace popped:", smallest)  # Output: 3
print("After heapreplace:", heap)        # Output: [4, 4, 5, 7, 9]

# 5. heapq.nlargest(n, iterable, key=None)
# Returns the n largest elements from the iterable
numbers = [1, 3, 5, 7, 9, 2]
largest = heapq.nlargest(3, numbers)
print("3 largest numbers:", largest)  # Output: [9, 7, 5]

# 6. heapq.nsmallest(n, iterable, key=None)
# Returns the n smallest elements from the iterable
smallest = heapq.nsmallest(3, numbers)
print("3 smallest numbers:", smallest)  # Output: [1, 2, 3]

# 7. heapq.merge(*iterables, key=None, reverse=False)
# Merges multiple sorted iterables into a single sorted iterator
list1 = [1, 3, 5]
list2 = [2, 4, 6]
merged = heapq.merge(list1, list2)
print("Merged lists:", list(merged))  # Output: [1, 2, 3, 4, 5, 6]

# Simulating a Max-Heap (by negating the values)
heap = []
heapq.heappush(heap, -10)
heapq.heappush(heap, -5)
heapq.heappush(heap, -15)
print("Max heap simulation:", [-x for x in heap])  # Output: [15, 5, 10]

# Popping the largest element in the simulated max-heap
max_value = -heapq.heappop(heap)
print("Popped max value:", max_value)  # Output: 15
print("After popping:", [-x for x in heap])  # Output: [10, 5]

# Explanation of heapq functions:
# - heapq.heappush(heap, item) - Push item onto heap
# - heapq.heappop(heap) - Pop smallest item from heap
# - heapq.heappushpop(heap, item) - Push item, then pop smallest item
# - heapq.heapreplace(heap, item) - Pop smallest item, then push item
# - heapq.nlargest(n, iterable) - Get the n largest elements
# - heapq.nsmallest(n, iterable) - Get the n smallest elements
# - heapq.merge(*iterables) - Merge multiple sorted iterables into one
```
