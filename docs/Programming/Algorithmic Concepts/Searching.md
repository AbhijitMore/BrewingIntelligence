# Searching

## Linear Search

```python linenums="1"
def linear_search(arr, target):
    for index in range(len(arr)):
        if arr[index] == target:
            return index  # Return the index if target is found
    return -1  # Return -1 if the target is not found

numbers = [10, 20, 30, 40, 50]
target = 30
result = linear_search(numbers, target)
if result != -1:
    print(f"Element found at index {result}")
else:
    print("Element not found")
```

## Binary Search

```python linenums="1"
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] > target:
            right = mid - 1
        else:
            left = mid + 1
    return -1
    
numbers = [10, 20, 30, 40, 50, 60, 70]
target = 30
result = binary_search(numbers, target)
if result != -1:
    print(f"Element found at index {result}")
else:
    print("Element not found")
```

## Problems

### Introductory Problems
### Upper Bound and Lower Bound
### Search on Matrix
### Missing and Repeating Number
### Binary Search on Semi-Sorted Space
### Binary Search On Answer
### Minmax Problems
### Finding the K-th Element