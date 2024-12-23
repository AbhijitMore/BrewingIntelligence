

## Implementation
```python linenums="1"
class Node:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None

class BinarySearchTree:
    def __init__(self):
        self.root = None

    def insert(self, key):
        def _insert(root, key):
            if root is None:
                return Node(key)
            if key < root.key:
                root.left = _insert(root.left, key)
            else:
                root.right = _insert(root.right, key)
            return root
        
        self.root = _insert(self.root, key)

    def search(self, key):
        def _search(root, key):
            if root is None or root.key == key:
                return root
            if key < root.key:
                return _search(root.left, key)
            return _search(root.right, key)

        return _search(self.root, key) is not None

    def delete(self, key):
        def _delete(root, key):
            if root is None:
                return root
            if key < root.key:
                root.left = _delete(root.left, key)
            elif key > root.key:
                root.right = _delete(root.right, key)
            else:
                if root.left is None:
                    return root.right
                elif root.right is None:
                    return root.left
                min_larger_node = self._get_min(root.right)
                root.key = min_larger_node.key
                root.right = _delete(root.right, min_larger_node.key)
            return root
        
        self.root = _delete(self.root, key)

    def floor(self, key):
        def _floor(root, key, floor_val=None):
            if root is None:
                return floor_val
            if root.key == key:
                return root.key
            elif key < root.key:
                return _floor(root.left, key, floor_val)
            else:
                floor_val = root.key  # Current node could be a candidate for the floor
                return _floor(root.right, key, floor_val)

        return _floor(self.root, key)

    def ceil(self, key):
        def _ceil(root, key, ceil_val=None):
            if root is None:
                return ceil_val
            if root.key == key:
                return root.key
            elif key > root.key:
                return _ceil(root.right, key, ceil_val)
            else:
                ceil_val = root.key  # Current node could be a candidate for the ceil
                return _ceil(root.left, key, ceil_val)

        return _ceil(self.root, key)

    def inorder(self):
        def _inorder(root):
            return _inorder(root.left) + [root.key] + _inorder(root.right) if root else []

        return _inorder(self.root)

    def _get_min(self, root):
        while root.left:
            root = root.left
        return root
        
# Example usage:
bst = BinarySearchTree()
bst.insert(20)
bst.insert(8)
bst.insert(22)
bst.insert(4)
bst.insert(12)
bst.insert(10)
bst.insert(14)

# Inorder traversal
print("Inorder traversal:", bst.inorder())

# Search for an element
print("Search 10:", bst.search(10))  # Should return True
print("Search 25:", bst.search(25))  # Should return False

# Floor and Ceil
print("Floor of 13:", bst.floor(13))  # Should return 12
print("Ceil of 13:", bst.ceil(13))    # Should return 14

# Deleting a node
bst.delete(10)
print("Inorder after deleting 10:", bst.inorder())

bst.delete(8)
print("Inorder after deleting 8:", bst.inorder())
```