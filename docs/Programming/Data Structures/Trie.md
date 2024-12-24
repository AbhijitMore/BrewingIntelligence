## Implemntation
```python linenums="1"
class TrieNode:
    def __init__(self):
        self.children = {}  # A dictionary to store child nodes
        self.is_end_of_word = False  # Flag to mark the end of a word

class Trie:
    def __init__(self):
        self.root = TrieNode()  # The root of the Trie is an empty node

    def insert(self, word):
        """Insert a word into the Trie."""
        current = self.root
        for char in word:
            # If the character is not in the current node's children, add it
            if char not in current.children:
                current.children[char] = TrieNode()
            current = current.children[char]
        current.is_end_of_word = True  # Mark the end of the word

    def search(self, word):
        """Search for a word in the Trie."""
        current = self.root
        for char in word:
            if char not in current.children:
                return False  # Word not found
            current = current.children[char]
        return current.is_end_of_word  # Return True if it's the end of a word

    def starts_with(self, prefix):
        """Check if there is any word in the Trie that starts with the given prefix."""
        current = self.root
        for char in prefix:
            if char not in current.children:
                return False  # Prefix not found
            current = current.children[char]
        return True  # Prefix exists in the Trie

    def delete(self, word):
        """Delete a word from the Trie."""
        def _delete(node, word, index):
            if index == len(word):
                # If we've reached the end of the word, mark this node as not end of a word
                if node.is_end_of_word:
                    node.is_end_of_word = False
                return len(node.children) == 0  # Return True if the node has no children
            char = word[index]
            if char not in node.children:
                return False  # Word not found
            can_delete = _delete(node.children[char], word, index + 1)

            # If can delete is True, remove the child node
            if can_delete:
                del node.children[char]
                return len(node.children) == 0  # Return True if the current node has no children
            return False

        _delete(self.root, word, 0)

    def display(self):
        """Display all words in the Trie."""
        def _display(node, prefix):
            if node.is_end_of_word:
                print(prefix)  # Print the word when we reach the end of a word
            for char, child in node.children.items():
                _display(child, prefix + char)

        _display(self.root, "")

# Example usage
trie = Trie()
trie.insert("apple")
trie.insert("app")
trie.insert("bat")
trie.insert("ball")
trie.insert("batman")

# Search for words
print(trie.search("apple"))  # Output: True
print(trie.search("app"))    # Output: True
print(trie.search("bat"))    # Output: True
print(trie.search("ball"))   # Output: True
print(trie.search("batman")) # Output: True
print(trie.search("banana")) # Output: False

# Prefix search
print(trie.starts_with("ba"))  # Output: True
print(trie.starts_with("app")) # Output: True
print(trie.starts_with("ban")) # Output: False

# Delete a word
trie.delete("bat")
trie.display()  # Output: app, apple, ball, batman

# Delete a word that doesn't exist
trie.delete("banana")
trie.display()  # Output: app, apple, ball, batman
```