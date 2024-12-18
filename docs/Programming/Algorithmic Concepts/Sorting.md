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

## 5. Quick Sort

## 6. Counting Sort

## 7. Radix Sort

## 8. Bucket Sort

## 9. Cyclic Sort

## 10. Custom Sort
