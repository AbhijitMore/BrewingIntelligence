# Sorting

## 1. Bubble Sort

=== "Basic"
    ```python linenums="1"
    def bubble_sort(numbers):
        length = len(numbers)
        for i in range(length-1):
            for j in range(length-i-1):
                if numbers[j]>numbers[j+1]:
                    numbers[j],numbers[j+1] = numbers[j+1], numbers[j]
        return numbers

    numbers = [60,40,70,20,50,30,90,45]
    sorted_numbers = bubble_sort(numbers)
    print(sorted_numbers)
    ```
=== "Optimized"
    ```python linenums="1"
    def bubble_sort(numbers):
        length = len(numbers)
        for i in range(length-1):
            swapped = False
            for j in range(length-i-1):
                if numbers[j]>numbers[j+1]:
                    numbers[j],numbers[j+1] = numbers[j+1], numbers[j]
                    swapped = True
            if not swapped:
                break
        return numbers

    numbers = [60,40,70,20,50,30,90,45]
    sorted_numbers = bubble_sort(numbers)
    print(sorted_numbers)
    ```

## 2. Insertion Sort

```python linenums="1"
def insertion_sort(numbers):
    for i in range(1, len(numbers)):
        key = numbers[i]
        j = i - 1
        while j >= 0 and numbers[j] > key:
            numbers[j + 1] = numbers[j]
            j -= 1
        numbers[j + 1] = key
    return numbers

numbers = [60,40,70,20,50,30,90,45]
sorted_numbers = selection_sort(numbers)
print(sorted_numbers)
```

## 3. Selection Sort

```python linenums="1"
def selection_sort(numbers):
    length = len(numbers)
    for i in range(length-1):
        min_index = i
        for j in range(i+1, length):
            if numbers[j]<numbers[min_index]:
                min_index = j
        numbers[min_index],numbers[i] = numbers[i], numbers[min_index]
    return numbers

numbers = [60,40,70,20,50,30,90,45]
sorted_numbers = selection_sort(numbers)
print(sorted_numbers)
```

## 4. Merge Sort

```python linenums="1"
def merge(left, right):
    result = []
    i = j = 0

    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1

    result.extend(left[i:])
    result.extend(right[j:])

    return result

def merge_sort(numbers):
    
    if len(numbers) <= 1: return numbers
    mid = len(numbers) // 2

    left_half = merge_sort(numbers[:mid])
    right_half = merge_sort(numbers[mid:])

    return merge(left_half, right_half)

numbers = [60,40,70,20,50,30,90,45]
sorted_numbers = merge_sort(numbers)
print(sorted_numbers)
```

## 5. Quick Sort
=== "Naive partition"
    ```python linenums="1"
    def naive_partition(numbers, pivot):
        length = len(numbers)
        numbers[pivot], numbers[length-1] = numbers[length-1], numbers[pivot]
        temp = []
        
        for x in numbers:
            if x<=numbers[length-1]:
                temp.append(x)
        for x in numbers:
            if x>numbers[length-1]:
                temp.append(x)
        for i in range(length):
            numbers[i] = temp[i]

    def quick_sort(numbers):
        if len(numbers) <= 1:
            return numbers
        pivot_index = len(numbers) - 1
        naive_partition(numbers, pivot_index)
        left = [x for x in numbers if x <= numbers[pivot_index] and x != numbers[pivot_index]]
        right = [x for x in numbers if x > numbers[pivot_index]]
        return quick_sort(left) + [numbers[pivot_index]] + quick_sort(right)

    numbers = [60, 40, 70, 20, 50, 30, 90, 45]
    sorted_numbers = quick_sort(numbers)
    print(sorted_numbers)
    ```
=== "Lomuto partition"
    ```python linenums="1"
    def lomuto_partition(numbers, low, high):
        pivot = numbers[high]
        i = low - 1
        for j in range(low, high):
            if numbers[j] <= pivot:
                i += 1
                numbers[i], numbers[j] = numbers[j], numbers[i]
        numbers[i + 1], numbers[high] = numbers[high], numbers[i + 1]
        return i + 1

    def lomuto_quick_sort(numbers, low, high):
        if low < high:
            pi = lomuto_partition(numbers, low, high)
            lomuto_quick_sort(numbers, low, pi - 1)
            lomuto_quick_sort(numbers, pi + 1, high)

    numbers = [60, 40, 70, 20, 50, 30, 90, 45]
    lomuto_quick_sort(numbers, 0, len(numbers) - 1)
    print("Lomuto Quick Sort:", numbers)
    ```
=== "Hoare partition"
    ```python linenums="1"
    def hoare_partition(numbers, low, high):
        pivot = numbers[low]
        i = low - 1
        j = high + 1
        while True:
            i += 1
            while numbers[i] < pivot:
                i += 1
            j -= 1
            while numbers[j] > pivot:
                j -= 1
            if i >= j:
                return j
            numbers[i], numbers[j] = numbers[j], numbers[i]

    def hoare_quick_sort(numbers, low, high):
        if low < high:
            pi = hoare_partition(numbers, low, high)
            hoare_quick_sort(numbers, low, pi)
            hoare_quick_sort(numbers, pi + 1, high)

    numbers = [60, 40, 70, 20, 50, 30, 90, 45]
    hoare_quick_sort(numbers, 0, len(numbers) - 1)
    print("Hoare Quick Sort:", numbers)
    ```

## 6. Counting Sort

```python linenums="1"
def counting_sort(numbers):
    max_value = max(numbers)
    count = [0] * (max_value + 1)
    
    for num in numbers:
        count[num] += 1
    
    sorted_numbers = []
    for value in range(len(count)):
        sorted_numbers.extend([value] * count[value])
    
    return sorted_numbers

numbers = [60, 40, 70, 20, 50, 30, 90, 45]
sorted_numbers = counting_sort(numbers)
print("Sorted numbers:", sorted_numbers)
```
## 7. Radix Sort
```python 
def counting_sort_for_radix(numbers, place):
    size = len(numbers)
    output = [0] * size
    count = [0] * 10
    
    for num in numbers:
        index = num // place
        count[index % 10] += 1
    
    for i in range(1, 10):
        count[i] += count[i - 1]
    
    for num in reversed(numbers):
        index = num // place
        output[count[index % 10] - 1] = num
        count[index % 10] -= 1
    
    for i in range(size):
        numbers[i] = output[i]

def radix_sort(numbers):
    max_value = max(numbers)
    place = 1
    while max_value // place > 0:
        counting_sort_for_radix(numbers, place)
        place *= 10

numbers = [60, 40, 70, 20, 50, 30, 90, 45]
radix_sort(numbers)
print("Sorted numbers:", numbers)
```
## 8. Bucket Sort

```python linenums="1"
def insertion_sort(numbers):
    for i in range(1, len(numbers)):
        key = numbers[i]
        j = i - 1
        while j >= 0 and numbers[j] > key:
            numbers[j + 1] = numbers[j]
            j -= 1
        numbers[j + 1] = key

def bucket_sort(numbers):
    if not numbers:
        return numbers
    min_value = min(numbers)
    max_value = max(numbers)
    bucket_count = len(numbers)
    buckets = [[] for _ in range(bucket_count)]
    for num in numbers:
        index = (num - min_value) * (bucket_count - 1) // (max_value - min_value) if max_value != min_value else 0
        buckets[index].append(num)
    for bucket in buckets:
        insertion_sort(bucket)
    return [num for bucket in buckets for num in bucket]

numbers = [60, 40, 70, 20, 50, 30, 90, 45]
sorted_numbers = bucket_sort(numbers)
print("Sorted numbers:", sorted_numbers)
```