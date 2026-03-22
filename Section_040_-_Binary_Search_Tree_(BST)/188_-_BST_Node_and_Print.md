```py
from predefinedBSTs import create_predefined_bsts_manual

class BSTNode:
    def __init__(self,data):
        self.data = data
        self.left = None
        self.right = None

def print_bst(root):
    if(root is None):
        return
    print_bst(root.left)
    print(root.data,end = " ") # Inorder Traversal of my BST
    print_bst(root.right)

root1, root2, root3 = create_predefined_bsts_manual()


def print_binary_tree(node):
    if node is None:
        return
    
    # Format: Node: L->LeftChild, R->RightChild
    print(f"{node.data}:", end=" ")
    
    if node.left:
        print(f"L->{node.left.data}", end=", ")
    else:
        print("L->None", end=", ")
    
    if node.right:
        print(f"R->{node.right.data}")
    else:
        print("R->None")
    
    # Recur for the left and right children
    print_binary_tree(node.left)
    print_binary_tree(node.right)

# Example usage to print the tree
print("Binary Tree Structure:")


print_binary_tree(root2)

```


The provided code demonstrates two ways to visualize a Binary Search Tree (BST) using Python. Below is a breakdown of each component.

---

## BSTNode Class

```python
class BSTNode:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None
```

- **Purpose**: Defines a node in the BST.
- **Attributes**:
  - `data`: The value stored in the node.
  - `left`: Reference to the left child (initially `None`).
  - `right`: Reference to the right child (initially `None`).

This is a minimal node definition; no methods are added for searching or insertion.

---

## print_bst – Inorder Traversal

```python
def print_bst(root):
    if root is None:
        return
    print_bst(root.left)
    print(root.data, end=" ")
    print_bst(root.right)
```

- **Purpose**: Prints the BST keys in **sorted order** using an inorder traversal.
- **Algorithm**:
  1. Recursively traverse the left subtree.
  2. Print the current node’s data.
  3. Recursively traverse the right subtree.
- **Output**: Values separated by spaces, in ascending order (assuming a valid BST).
- **Note**: The function does not return anything; it directly prints to the console.

---

## print_binary_tree – Structural Output

```python
def print_binary_tree(node):
    if node is None:
        return
    
    print(f"{node.data}:", end=" ")
    
    if node.left:
        print(f"L->{node.left.data}", end=", ")
    else:
        print("L->None", end=", ")
    
    if node.right:
        print(f"R->{node.right.data}")
    else:
        print("R->None")
    
    print_binary_tree(node.left)
    print_binary_tree(node.right)
```

- **Purpose**: Prints the tree structure in a human‑readable format, showing each node’s children.
- **Process**:
  1. For the given node, print its data followed by a colon.
  2. If the left child exists, print `L-><child.data>`; otherwise print `L->None`.
  3. Similarly for the right child, with a newline after the right part.
  4. Recursively traverse the left subtree, then the right subtree.
- **Output Example**:
  ```
  10: L->5, R->15
  5: L->None, R->8
  8: L->None, R->None
  15: L->12, R->20
  ...
  ```

---

## Retrieving Predefined BSTs

```python
root1, root2, root3 = create_predefined_bsts_manual()
```

- The module `predefinedBSTs` contains a function `create_predefined_bsts_manual()` that returns three BST roots.
- These roots are presumably constructed manually for testing or demonstration purposes. Their exact structure is not shown in the snippet.

---

## Example Usage

```python
print("Binary Tree Structure:")
print_binary_tree(root2)
```

- The code calls `print_binary_tree` on `root2` to display the structure of the second predefined BST.
- The output will show the hierarchical relationships for that tree.

---

## Observations

- **Missing BST properties**: The code does not enforce BST invariants (e.g., left < node < right). It assumes the provided trees are valid BSTs.
- **Traversal functions**: Both functions use recursion, which may cause a stack overflow for very deep trees. Iterative alternatives are possible.
- **Printing format**: `print_binary_tree` prints a node’s children immediately after the node, which gives a clear but not indented view. For a more visually structured tree, an indented representation would be needed.

---

## Suggested Improvements

- Add `__repr__` or `__str__` methods to the node class for easier debugging.
- Use iterative traversal to avoid recursion limits.
- Consider adding a method to verify the BST property (useful for debugging predefined trees).
- Provide a function to print the tree in a hierarchical, indented format for better readability.