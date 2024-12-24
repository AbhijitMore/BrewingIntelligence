# Queue

## Using List
```python linenums="1"
class QueueList:
    def __init__(self):
        self.queue = []

    def enqueue(self, item):
        self.queue.append(item)

    def dequeue(self):
        if self.is_empty():
            raise IndexError("Dequeue from empty queue")
        return self.queue.pop(0)

    def front(self):
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.queue[0]

    def is_empty(self):
        return len(self.queue) == 0

    def size(self):
        return len(self.queue)

    def __str__(self):
        return str(self.queue)

# Example usage
q1 = QueueList()
q1.enqueue(1)
q1.enqueue(2)
q1.enqueue(3)
print(q1)  # Output: [1, 2, 3]
q1.dequeue()
print(q1)  # Output: [2, 3]
```
## Using Linked list
```python linenums="1"
class Node:
    def __init__(self, data=None):
        self.data = data
        self.next = None

class QueueLinkedList:
    def __init__(self):
        self.front = None
        self.rear = None
        self.size = 0

    def enqueue(self, item):
        new_node = Node(item)
        if self.rear:
            self.rear.next = new_node
        self.rear = new_node
        if self.front is None:
            self.front = new_node
        self.size += 1

    def dequeue(self):
        if self.is_empty():
            raise IndexError("Dequeue from empty queue")
        removed_data = self.front.data
        self.front = self.front.next
        if self.front is None:
            self.rear = None
        self.size -= 1
        return removed_data

    def front_value(self):
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.front.data

    def is_empty(self):
        return self.front is None

    def queue_size(self):
        return self.size

    def __str__(self):
        elements = []
        current = self.front
        while current:
            elements.append(current.data)
            current = current.next
        return str(elements)

# Example usage
q2 = QueueLinkedList()
q2.enqueue(1)
q2.enqueue(2)
q2.enqueue(3)
print(q2)  # Output: [1, 2, 3]
q2.dequeue()
print(q2)  # Output: [2, 3]
```