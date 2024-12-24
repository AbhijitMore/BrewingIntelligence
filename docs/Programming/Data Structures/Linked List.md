
## Singly Linked List
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

    def size(self):
        """Return the number of nodes in the linked list."""
        current = self.head
        count = 0
        while current:
            count += 1
            current = current.next
        return count

    def is_empty(self):
        """Check if the linked list is empty."""
        return self.head is None

    def get_first(self):
        """Return the first node (head) of the list."""
        if self.head:
            return self.head.data
        return None

    def get_last(self):
        """Return the last node (tail) of the list."""
        if not self.head:
            return None
        current = self.head
        while current.next:
            current = current.next
        return current.data

    def remove_duplicates(self):
        """Remove duplicate values from the linked list."""
        current = self.head
        seen = set()
        prev = None
        while current:
            if current.data in seen:
                prev.next = current.next  # Bypass the duplicate
            else:
                seen.add(current.data)
                prev = current
            current = current.next

    def clear(self):
        """Clear the entire linked list."""
        self.head = None

    def get_node_at_index(self, index):
        """Return the node at a given index."""
        current = self.head
        current_index = 0
        while current:
            if current_index == index:
                return current.data
            current = current.next
            current_index += 1
        return None  # Index out of range

    def delete_at_index(self, index):
        """Delete a node at the given index."""
        if index == 0:
            if self.head:
                self.head = self.head.next
            return
        current = self.head
        current_index = 0
        while current and current.next:
            if current_index == index - 1:
                current.next = current.next.next
                return
            current = current.next
            current_index += 1
        raise IndexError("Index out of bounds")

    def to_list(self):
        """Convert the linked list to a Python list."""
        result = []
        current = self.head
        while current:
            result.append(current.data)
            current = current.next
        return result

    def sort(self):
        """Sort the linked list."""
        if not self.head or not self.head.next:
            return
        # Implementing merge sort or any sorting algorithm
        self.head = self._merge_sort(self.head)

    def _merge_sort(self, head):
        """Helper function for merge sort."""
        if not head or not head.next:
            return head
        middle = self._get_middle(head)
        next_to_middle = middle.next
        middle.next = None
        left = self._merge_sort(head)
        right = self._merge_sort(next_to_middle)
        sorted_list = self._merge(left, right)
        return sorted_list

    def _get_middle(self, head):
        """Find the middle node of the list."""
        if not head:
            return head
        slow = head
        fast = head
        while fast.next and fast.next.next:
            slow = slow.next
            fast = fast.next.next
        return slow

    def _merge(self, left, right):
        """Merge two sorted linked lists."""
        if not left:
            return right
        if not right:
            return left
        if left.data <= right.data:
            left.next = self._merge(left.next, right)
            return left
        else:
            right.next = self._merge(left, right.next)
            return right

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

# Size of the list
print(f"Size: {ll.size()}")  # Output: Size: 7

# Search for an element
print(f"Search 15: {ll.search(15)}")  # Output: Search 15: 3

# Remove duplicates (if any)
ll.remove_duplicates()

# Clear the linked list
ll.clear()
ll.display()  # Output: (empty list)

# Add some nodes again
ll.append(100)
ll.append(200)
ll.append(300)
ll.display()  # Output: 100 -> 200 -> 300

# Convert to Python list
print(f"List as Python list: {ll.to_list()}")  # Output: List as Python list: [100, 200, 300]

# Delete at index 1
ll.delete_at_index(1)
ll.display()  # Output: 100 -> 300

# Sort the list
ll.sort()
ll.display()  # Output: 100 -> 300 (already sorted in this case)
```
## Doubly Linked List
```python linenums="1"
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
        self.prev = None

class DoublyLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None

    def append(self, data):
        """Add a new node to the end of the doubly linked list."""
        new_node = Node(data)
        if not self.head:
            self.head = self.tail = new_node
            return
        self.tail.next = new_node
        new_node.prev = self.tail
        self.tail = new_node

    def prepend(self, data):
        """Add a new node to the beginning of the doubly linked list."""
        new_node = Node(data)
        if not self.head:
            self.head = self.tail = new_node
            return
        new_node.next = self.head
        self.head.prev = new_node
        self.head = new_node

    def insert_at_position(self, data, position):
        """Insert a new node at the specified position."""
        new_node = Node(data)
        if position == 0:
            self.prepend(data)
            return
        current = self.head
        index = 0
        while current and index < position - 1:
            current = current.next
            index += 1
        if not current:
            raise IndexError("Position out of bounds")
        new_node.next = current.next
        if current.next:
            current.next.prev = new_node
        current.next = new_node
        new_node.prev = current

    def delete(self, data):
        """Delete a node with the specified value."""
        current = self.head
        while current:
            if current.data == data:
                if current.prev:
                    current.prev.next = current.next
                else:
                    self.head = current.next  # If deleting the head
                if current.next:
                    current.next.prev = current.prev
                else:
                    self.tail = current.prev  # If deleting the tail
                return
            current = current.next

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
        """Reverse the doubly linked list."""
        current = self.head
        while current:
            current.prev, current.next = current.next, current.prev
            current = current.prev
        self.head, self.tail = self.tail, self.head

    def display(self):
        """Display the doubly linked list as a sequence of nodes."""
        nodes = []
        current = self.head
        while current:
            nodes.append(str(current.data))
            current = current.next
        print(" <-> ".join(nodes))

    def size(self):
        """Return the number of nodes in the doubly linked list."""
        current = self.head
        count = 0
        while current:
            count += 1
            current = current.next
        return count

    def is_empty(self):
        """Check if the doubly linked list is empty."""
        return self.head is None

    def get_first(self):
        """Return the first node (head) of the list."""
        if self.head:
            return self.head.data
        return None

    def get_last(self):
        """Return the last node (tail) of the list."""
        if self.tail:
            return self.tail.data
        return None

    def remove_duplicates(self):
        """Remove duplicate values from the doubly linked list."""
        current = self.head
        seen = set()
        while current:
            if current.data in seen:
                self.delete(current.data)
            else:
                seen.add(current.data)
            current = current.next

    def clear(self):
        """Clear the entire doubly linked list."""
        self.head = self.tail = None

    def get_node_at_index(self, index):
        """Return the node at a given index."""
        current = self.head
        current_index = 0
        while current:
            if current_index == index:
                return current.data
            current = current.next
            current_index += 1
        return None  # Index out of range

    def delete_at_index(self, index):
        """Delete a node at the given index."""
        if index == 0:
            if self.head:
                self.head = self.head.next
                if self.head:
                    self.head.prev = None
            return
        current = self.head
        current_index = 0
        while current and current.next:
            if current_index == index - 1:
                if current.next:
                    current.next = current.next.next
                    if current.next:
                        current.next.prev = current
                return
            current = current.next
            current_index += 1
        raise IndexError("Index out of bounds")

    def to_list(self):
        """Convert the doubly linked list to a Python list."""
        result = []
        current = self.head
        while current:
            result.append(current.data)
            current = current.next
        return result

    def sort(self):
        """Sort the doubly linked list."""
        if not self.head or not self.head.next:
            return
        # Implementing merge sort or any sorting algorithm
        self.head = self._merge_sort(self.head)

    def _merge_sort(self, head):
        """Helper function for merge sort."""
        if not head or not head.next:
            return head
        middle = self._get_middle(head)
        next_to_middle = middle.next
        middle.next = None
        left = self._merge_sort(head)
        right = self._merge_sort(next_to_middle)
        sorted_list = self._merge(left, right)
        return sorted_list

    def _get_middle(self, head):
        """Find the middle node of the list."""
        if not head:
            return head
        slow = head
        fast = head
        while fast.next and fast.next.next:
            slow = slow.next
            fast = fast.next.next
        return slow

    def _merge(self, left, right):
        """Merge two sorted doubly linked lists."""
        if not left:
            return right
        if not right:
            return left
        if left.data <= right.data:
            left.next = self._merge(left.next, right)
            if left.next:
                left.next.prev = left
            return left
        else:
            right.next = self._merge(left, right.next)
            if right.next:
                right.next.prev = right
            return right

# Example usage
dll = DoublyLinkedList()
dll.append(10)
dll.append(20)
dll.append(30)
dll.display()  # Output: 10 <-> 20 <-> 30

dll.prepend(5)
dll.display()  # Output: 5 <-> 10 <-> 20 <-> 30

dll.insert_at_position(15, 2)
dll.display()  # Output: 5 <-> 10 <-> 15 <-> 20 <-> 30

dll.insert_at_position(35, 5)
dll.display()  # Output: 5 <-> 10 <-> 15 <-> 20 <-> 30 <-> 35

dll.insert_at_position(0, 0)
dll.display()  # Output: 0 <-> 5 <-> 10 <-> 15 <-> 20 <-> 30 <-> 35

# Reverse the doubly linked list
dll.reverse()
dll.display()  # Output: 35 <-> 30 <-> 20 <-> 15 <-> 10 <-> 5 <-> 0

# Size of the list
print(f"Size: {dll.size()}")  # Output: Size: 7

# Search for an element
print(f"Search 15: {dll.search(15)}")  # Output: Search 15: 3

# Remove duplicates (if any)
dll.remove_duplicates()

# Clear the doubly linked list
dll.clear()
dll.display()  # Output: (empty list)

# Add some nodes again
dll.append(100)
dll.append(200)
dll.append(300)
dll.display()  # Output: 100 <-> 200 <-> 300

# Convert to Python list
print(f"List as Python list: {dll.to_list()}")  # Output: List as Python list: [100, 200, 300]

# Delete at index 1
dll.delete_at_index(1)
dll.display()  # Output: 100 <-> 300

# Sort the list
dll.sort()
dll.display()  # Output: 100 <-> 300 (already sorted in this case)
```
## Cicular Linked List
```python linenums="1"
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class CircularLinkedList:
    def __init__(self):
        self.head = None

    def append(self, data):
        """Add a new node to the end of the circular linked list."""
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            new_node.next = self.head  # Points to itself, forming a circle
            return
        current = self.head
        while current.next != self.head:  # Traverse until we find the last node
            current = current.next
        current.next = new_node
        new_node.next = self.head  # Complete the circle

    def prepend(self, data):
        """Add a new node to the beginning of the circular linked list."""
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            new_node.next = self.head  # Points to itself
            return
        new_node.next = self.head
        current = self.head
        while current.next != self.head:  # Traverse until the last node
            current = current.next
        current.next = new_node  # Last node points to the new node
        self.head = new_node  # Update head

    def insert_at_position(self, data, position):
        """Insert a new node at the specified position in the circular linked list."""
        new_node = Node(data)
        if position == 0:
            self.prepend(data)
            return
        current = self.head
        index = 0
        while current and index < position - 1:
            current = current.next
            index += 1
            if current == self.head:  # Loop back to the start
                raise IndexError("Position out of bounds")
        if not current:
            raise IndexError("Position out of bounds")
        new_node.next = current.next
        current.next = new_node

    def delete(self, data):
        """Delete the node with the specified value."""
        if not self.head:
            return
        current = self.head
        prev = None
        while True:
            if current.data == data:
                if prev:  # If not the first node
                    prev.next = current.next
                    if current == self.head:  # If deleting the head node
                        self.head = current.next
                else:
                    # Deleting the only node (head)
                    if current.next == self.head:
                        self.head = None
                    else:
                        self.head = current.next
                        prev = self.head
                        while prev.next != current:  # Update last node's next
                            prev = prev.next
                        prev.next = self.head
                return
            prev = current
            current = current.next
            if current == self.head:  # Loop back to the start
                break

    def search(self, data):
        """Search for a node with the specified value and return its index."""
        if not self.head:
            return -1
        current = self.head
        index = 0
        while True:
            if current.data == data:
                return index
            current = current.next
            index += 1
            if current == self.head:  # Loop back to the start
                break
        return -1

    def reverse(self):
        """Reverse the circular linked list."""
        if not self.head or self.head.next == self.head:
            return  # No need to reverse if list is empty or has one node
        prev = None
        current = self.head
        first_node = self.head
        while True:
            next_node = current.next
            current.next = prev
            prev = current
            current = next_node
            if current == first_node:  # Loop back to the start
                break
        self.head.next = prev  # Complete the circular reference
        self.head = prev  # New head is the last node in original list

    def display(self):
        """Display the circular linked list."""
        if not self.head:
            print("List is empty")
            return
        nodes = []
        current = self.head
        while True:
            nodes.append(str(current.data))
            current = current.next
            if current == self.head:  # Loop back to the start
                break
        print(" -> ".join(nodes))

    def size(self):
        """Return the number of nodes in the circular linked list."""
        if not self.head:
            return 0
        count = 1
        current = self.head.next
        while current != self.head:
            count += 1
            current = current.next
        return count

    def is_empty(self):
        """Check if the circular linked list is empty."""
        return self.head is None

    def get_first(self):
        """Return the first node (head) of the list."""
        if self.head:
            return self.head.data
        return None

    def get_last(self):
        """Return the last node (tail) of the list."""
        if not self.head:
            return None
        current = self.head
        while current.next != self.head:
            current = current.next
        return current.data

    def remove_duplicates(self):
        """Remove duplicate values from the circular linked list."""
        if not self.head:
            return
        seen = set()
        current = self.head
        prev = None
        while True:
            if current.data in seen:
                prev.next = current.next
                if current == self.head:  # Update head if we deleted the first node
                    self.head = current.next
            else:
                seen.add(current.data)
                prev = current
            current = current.next
            if current == self.head:  # Loop back to the start
                break

    def clear(self):
        """Clear the entire circular linked list."""
        self.head = None

    def get_node_at_index(self, index):
        """Return the node at a given index."""
        if not self.head:
            return None
        current = self.head
        current_index = 0
        while True:
            if current_index == index:
                return current.data
            current = current.next
            current_index += 1
            if current == self.head:  # Loop back to the start
                break
        return None  # Index out of range

    def delete_at_index(self, index):
        """Delete a node at the given index."""
        if not self.head:
            raise IndexError("Index out of bounds")
        if index == 0:
            self.delete(self.head.data)
            return
        current = self.head
        prev = None
        current_index = 0
        while True:
            if current_index == index:
                if prev:
                    prev.next = current.next
                return
            prev = current
            current = current.next
            current_index += 1
            if current == self.head:  # Loop back to the start
                break
        raise IndexError("Index out of bounds")

    def to_list(self):
        """Convert the circular linked list to a Python list."""
        result = []
        if not self.head:
            return result
        current = self.head
        while True:
            result.append(current.data)
            current = current.next
            if current == self.head:  # Loop back to the start
                break
        return result

    def sort(self):
        """Sort the circular linked list."""
        if not self.head or self.head.next == self.head:
            return  # No need to sort if list is empty or has one node
        nodes = self.to_list()
        nodes.sort()
        self.clear()
        for node in nodes:
            self.append(node)

# Example usage
cll = CircularLinkedList()
cll.append(10)
cll.append(20)
cll.append(30)
cll.display()  # Output: 10 -> 20 -> 30

cll.prepend(5)
cll.display()  # Output: 5 -> 10 -> 20 -> 30

cll.insert_at_position(15, 2)
cll.display()  # Output: 5 -> 10 -> 15 -> 20 -> 30

cll.insert_at_position(35, 5)
cll.display()  # Output: 5 -> 10 -> 15 -> 20 -> 30 -> 35

cll.insert_at_position(0, 0)
cll.display()  # Output: 0 -> 5 -> 10 -> 15 -> 20 -> 30 -> 35

# Reverse the circular linked list
cll.reverse()
cll.display()  # Output: 35 -> 30 -> 20 -> 15 -> 10 -> 5 -> 0

# Size of the list
print(f"Size: {cll.size()}")  # Output: Size: 7

# Search for an element
print(f"Search 15: {cll.search(15)}")  # Output: Search 15: 3

# Remove duplicates (if any)
cll.remove_duplicates()

# Clear the circular linked list
cll.clear()
cll.display()  # Output: (empty list)

# Add some nodes again
cll.append(100)
cll.append(200)
cll.append(300)
cll.display()  # Output: 100 -> 200 -> 300

# Convert to Python list
print(f"List as Python list: {cll.to_list()}")  # Output: List as Python list: [100, 200, 300]

# Delete at index 1
cll.delete_at_index(1)
cll.display()  # Output: 100 -> 300

# Sort the list
cll.sort()
cll.display()  # Output: 100 -> 300 (already sorted in this case)
```