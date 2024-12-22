

## Tree

### Terminologies

### Traversals
#### Breadth First Traversal
#### Depth First Traversal
##### 1. Inorder
##### 2. Preorder
##### 3. Postorder

#### Implementation
```python linenums="1"
class Node:
    def __init__(self, value):
        self.value = value  # Node's value
        self.left = None    # Left child
        self.right = None   # Right child

class BinaryTree:
    def __init__(self, root_value):
        self.root = Node(root_value)  # Root node of the binary tree

    def insert_left(self, parent, value):
        if parent.left is None:
            parent.left = Node(value)
        else:
            new_node = Node(value)
            new_node.left = parent.left
            parent.left = new_node

    def insert_right(self, parent, value):
        if parent.right is None:
            parent.right = Node(value)
        else:
            new_node = Node(value)
            new_node.right = parent.right
            parent.right = new_node

    # In-order Traversal: Left -> Root -> Right
    def inorder_traversal(self, node):
        if node:
            self.inorder_traversal(node.left)
            print(node.value, end=" ")
            self.inorder_traversal(node.right)

    # Pre-order Traversal: Root -> Left -> Right
    def preorder_traversal(self, node):
        if node:
            print(node.value, end=" ")
            self.preorder_traversal(node.left)
            self.preorder_traversal(node.right)

    # Post-order Traversal: Left -> Right -> Root
    def postorder_traversal(self, node):
        if node:
            self.postorder_traversal(node.left)
            self.postorder_traversal(node.right)
            print(node.value, end=" ")

    # Function to get the size of the tree (number of nodes)
    def size(self, node):
        if node is None:
            return 0
        else:
            # Size is 1 (current node) + size of left subtree + size of right subtree
            return 1 + self.size(node.left) + self.size(node.right)

    # Function to get the maximum value in the tree
    def max_value(self, node):
        if node is None:
            return float('-inf')  # Negative infinity as base case
        else:
            # Get the maximum value in the left and right subtrees
            left_max = self.max_value(node.left)
            right_max = self.max_value(node.right)
            # Return the largest of the current node's value, left max, and right max
            return max(node.value, left_max, right_max)

# Example usage of the BinaryTree class:
if __name__ == "__main__":
    # Creating the root of the binary tree
    tree = BinaryTree(50)

    # Inserting left and right children
    tree.insert_left(tree.root, 27)
    tree.insert_right(tree.root, 32)

    # Inserting more nodes
    tree.insert_left(tree.root.left, 15)
    tree.insert_right(tree.root.left, 39)

    # Traversals
    print("In-order Traversal:")
    tree.inorder_traversal(tree.root)
    print("\nPre-order Traversal:")
    tree.preorder_traversal(tree.root)
    print("\nPost-order Traversal:")
    tree.postorder_traversal(tree.root)

    # Get size of the tree
    print("\nSize of the tree:", tree.size(tree.root))

    # Get maximum value in the tree
    print("Maximum value in the tree:", tree.max_value(tree.root))
```

### Binary Tree 