# Stack

## Using List
```python linenums="1"
class StackList:
    def __init__(self):
        self.stack = []

    def push(self, item):
        """Push item onto the stack."""
        self.stack.append(item)

    def pop(self):
        """Pop item from the stack and return it."""
        if not self.is_empty():
            return self.stack.pop()
        return None

    def peek(self):
        """Return the top item of the stack."""
        if not self.is_empty():
            return self.stack[-1]
        return None

    def is_empty(self):
        """Check if the stack is empty."""
        return len(self.stack) == 0

    def size(self):
        """Return the size of the stack."""
        return len(self.stack)

    def balanced_parentheses(self, expression):
        """Evaluate balanced parentheses."""
        for char in expression:
            if char == '(':
                self.push(char)
            elif char == ')':
                if self.is_empty():
                    return False
                self.pop()
        return self.is_empty()

    def infix_to_postfix(self, expression):
        """Convert infix to postfix."""
        precedence = {'+': 1, '-': 1, '*': 2, '/': 2, '^': 3}
        output = []
        for char in expression:
            if char.isalnum():  # Operand
                output.append(char)
            elif char == '(':  # Left Parenthesis
                self.push(char)
            elif char == ')':  # Right Parenthesis
                while not self.is_empty() and self.peek() != '(':
                    output.append(self.pop())
                self.pop()  # Pop '('
            else:  # Operator
                while (not self.is_empty() and self.peek() != '(' and
                       precedence[char] <= precedence.get(self.peek(), 0)):
                    output.append(self.pop())
                self.push(char)
        while not self.is_empty():
            output.append(self.pop())
        return ''.join(output)

    def evaluate_postfix(self, expression):
        """Evaluate a postfix expression."""
        for char in expression:
            if char.isdigit():  # Operand
                self.push(int(char))
            else:  # Operator
                right = self.pop()
                left = self.pop()
                if char == '+':
                    self.push(left + right)
                elif char == '-':
                    self.push(left - right)
                elif char == '*':
                    self.push(left * right)
                elif char == '/':
                    self.push(left / right)
                elif char == '^':
                    self.push(left ** right)
        return self.pop()

    def infix_to_prefix(self, expression):
        """Convert infix to prefix."""
        # Reverse the infix expression
        expression = expression[::-1]
        expression = expression.replace('(', 'temp').replace(')', '(').replace('temp', ')')
        postfix = self.infix_to_postfix(expression)
        return postfix[::-1]

    def evaluate_prefix(self, expression):
        """Evaluate a prefix expression."""
        # Reverse the expression to handle it from left to right
        expression = expression[::-1]
        for char in expression:
            if char.isdigit():  # Operand
                self.push(int(char))
            else:  # Operator
                left = self.pop()
                right = self.pop()
                if char == '+':
                    self.push(left + right)
                elif char == '-':
                    self.push(left - right)
                elif char == '*':
                    self.push(left * right)
                elif char == '/':
                    self.push(left / right)
                elif char == '^':
                    self.push(left ** right)
        return self.pop()

expression = "3 + 5 * (2 - 8)"

# Using Stack with List
stack_list = StackList()
postfix = stack_list.infix_to_postfix(expression)
print("Postfix:", postfix)  # Postfix conversion
postfix_eval = stack_list.evaluate_postfix(postfix)
print("Postfix evaluation:", postfix_eval)

prefix = stack_list.infix_to_prefix(expression)
print("Prefix:", prefix)  # Prefix conversion
prefix_eval = stack_list.evaluate_prefix(prefix)
print("Prefix evaluation:", prefix_eval)

```

## Using Linked List
```python linenums="1"
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class StackLinkedList:
    def __init__(self):
        self.top = None

    def push(self, item):
        """Push item onto the stack."""
        new_node = Node(item)
        new_node.next = self.top
        self.top = new_node

    def pop(self):
        """Pop item from the stack and return it."""
        if self.is_empty():
            return None
        popped_node = self.top
        self.top = self.top.next
        return popped_node.data

    def peek(self):
        """Return the top item of the stack."""
        if self.is_empty():
            return None
        return self.top.data

    def is_empty(self):
        """Check if the stack is empty."""
        return self.top is None

    def size(self):
        """Return the size of the stack."""
        current = self.top
        count = 0
        while current:
            count += 1
            current = current.next
        return count

    def balanced_parentheses(self, expression):
        """Evaluate balanced parentheses."""
        for char in expression:
            if char == '(':
                self.push(char)
            elif char == ')':
                if self.is_empty():
                    return False
                self.pop()
        return self.is_empty()

    def infix_to_postfix(self, expression):
        """Convert infix to postfix."""
        precedence = {'+': 1, '-': 1, '*': 2, '/': 2, '^': 3}
        output = []
        for char in expression:
            if char.isalnum():  # Operand
                output.append(char)
            elif char == '(':  # Left Parenthesis
                self.push(char)
            elif char == ')':  # Right Parenthesis
                while not self.is_empty() and self.peek() != '(':
                    output.append(self.pop())
                self.pop()  # Pop '('
            else:  # Operator
                while (not self.is_empty() and self.peek() != '(' and
                       precedence[char] <= precedence.get(self.peek(), 0)):
                    output.append(self.pop())
                self.push(char)
        while not self.is_empty():
            output.append(self.pop())
        return ''.join(output)

    def evaluate_postfix(self, expression):
        """Evaluate a postfix expression."""
        for char in expression:
            if char.isdigit():  # Operand
                self.push(int(char))
            else:  # Operator
                right = self.pop()
                left = self.pop()
                if char == '+':
                    self.push(left + right)
                elif char == '-':
                    self.push(left - right)
                elif char == '*':
                    self.push(left * right)
                elif char == '/':
                    self.push(left / right)
                elif char == '^':
                    self.push(left ** right)
        return self.pop()

    def infix_to_prefix(self, expression):
        """Convert infix to prefix."""
        # Reverse the infix expression
        expression = expression[::-1]
        expression = expression.replace('(', 'temp').replace(')', '(').replace('temp', ')')
        postfix = self.infix_to_postfix(expression)
        return postfix[::-1]

    def evaluate_prefix(self, expression):
        """Evaluate a prefix expression."""
        # Reverse the expression to handle it from left to right
        expression = expression[::-1]
        for char in expression:
            if char.isdigit():  # Operand
                self.push(int(char))
            else:  # Operator
                left = self.pop()
                right = self.pop()
                if char == '+':
                    self.push(left + right)
                elif char == '-':
                    self.push(left - right)
                elif char == '*':
                    self.push(left * right)
                elif char == '/':
                    self.push(left / right)
                elif char == '^':
                    self.push(left ** right)
        return self.pop()

expression = "3 + 5 * (2 - 8)"

# Using Stack with Linked List
stack_linked_list = StackLinkedList()
postfix = stack_linked_list.infix_to_postfix(expression)
print("Postfix:", postfix)  # Postfix conversion
postfix_eval = stack_linked_list.evaluate_postfix(postfix)
print("Postfix evaluation:", postfix_eval)

prefix = stack_linked_list.infix_to_prefix(expression)
print("Prefix:", prefix)  # Prefix conversion
prefix_eval = stack_linked_list.evaluate_prefix(prefix)
print("Prefix evaluation:", prefix_eval)
```