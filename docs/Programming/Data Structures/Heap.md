

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