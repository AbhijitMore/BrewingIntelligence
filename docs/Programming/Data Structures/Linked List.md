
## Simple implementation

```python linenums="1"
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def append(self, data):
        """Add a new node to the end of the linked list."""
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            return
        current = self.head
        while current.next:
            current = current.next
        current.next = new_node

    def prepend(self, data):
        """Add a new node to the beginning of the linked list."""
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node

    def insert_at_position(self, data, position):
        """Insert a new node at the specified position."""
        new_node = Node(data)
        if position == 0:
            new_node.next = self.head
            self.head = new_node
            return
        current = self.head
        index = 0
        while current and index < position - 1:
            current = current.next
            index += 1
        if not current:
            raise IndexError("Position out of bounds")
        new_node.next = current.next
        current.next = new_node

    def delete(self, data):
        """Delete a node with the specified value."""
        if not self.head:
            return
        if self.head.data == data:
            self.head = self.head.next
            return
        current = self.head
        while current.next and current.next.data != data:
            current = current.next
        if current.next:
            current.next = current.next.next

    def search(self, data):
        """Search for a node with the specified value and return its index."""
        current = self.head
        index = 0
        while current:
            if current.data == data:
                return index
            current = current.next
            index += 1
        return -1

    def reverse(self):
        """Reverse the linked list."""
        prev = None
        current = self.head
        while current:
            next_node = current.next  # Store the next node
            current.next = prev      # Reverse the pointer
            prev = current           # Move prev to the current node
            current = next_node      # Move to the next node
        self.head = prev             # Update the head to the new first node

    def display(self):
        """Display the linked list as a sequence of nodes."""
        nodes = []
        current = self.head
        while current:
            nodes.append(str(current.data))
            current = current.next
        print(" -> ".join(nodes))

# Example usage
ll = LinkedList()
ll.append(10)
ll.append(20)
ll.append(30)
ll.display()  # Output: 10 -> 20 -> 30
ll.prepend(5)
ll.display()  # Output: 5 -> 10 -> 20 -> 30
ll.insert_at_position(15, 2)
ll.display()  # Output: 5 -> 10 -> 15 -> 20 -> 30
ll.insert_at_position(35, 5)
ll.display()  # Output: 5 -> 10 -> 15 -> 20 -> 30 -> 35
ll.insert_at_position(0, 0)
ll.display()  # Output: 0 -> 5 -> 10 -> 15 -> 20 -> 30 -> 35

# Reverse the linked list
ll.reverse()
ll.display()  # Output: 35 -> 30 -> 20 -> 15 -> 10 -> 5 -> 0
```