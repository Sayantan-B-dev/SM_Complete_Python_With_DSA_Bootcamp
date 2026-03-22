```py
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

class BSTNode:
    def __init__(self,data):
        self.data = data
        self.left = None
        self.right = None
    
class BST:
    def __init__(self):
        self.root = None

    def insert(self,data):
        self.root = self.insert_helper(data,self.root)

    def insert_helper(self,data,node):
        if(node==None):
            newNode = BSTNode(data)
            return newNode
        
        if(data <  node.data):
            node.left = self.insert_helper(data,node.left)
        else:
            node.right = self.insert_helper(data,node.right)

        return node           

    def get_min_node(self,node):
        current = node
        while(current.left is not None):
            current = current.left
        return current

    def delete_helper(self,data,root):
        if(root is None):
            return None
        
        if(data<root.data):
            root.left = self.delete_helper(data,root.left)
        elif(data>root.data):
            root.right = self.delete_helper(data,root.right)
        else:
            if(root.left is None):
                return root.right
            elif root.right is None:
                return root.left
            
            min_larger_node = self.get_min_node(root.right)
            root.data = min_larger_node.data

            root.right=self.delete_helper(min_larger_node.data,root.right)

        return root



    def delete(self,data):
        self.root = self.delete_helper(data,self.root)


    def search(self,data):
        return self.searchHelper(data,self.root)

    def searchHelper(self,data,root):
        if(root is None):
            return False
        if(root.data == data):
            return True
        
        if(data < root.data):
            return self.searchHelper(data,root.left)
        else:
            return self.searchHelper(data,root.right)



bstObject = BST()

bstObject.insert(20)
bstObject.insert(25)
bstObject.insert(10)
bstObject.insert(15)
print(bstObject.search(17))
bstObject.delete(20)
print_binary_tree(bstObject.root)
```

## 1. The BSTNode Class – What Is a Node?

```python
class BSTNode:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None
```

- **Purpose**: Each node holds one value (`data`) and two pointers (`left`, `right`) to its left and right child nodes.
- **Attributes**:
  - `data` – the key stored in the node.
  - `left` – reference to the left child (or `None` if no left child).
  - `right` – reference to the right child (or `None` if no right child).
- **Role in search**: The search algorithm uses the node’s `data` and its child pointers to navigate the tree.

---

## 2. The BST Class – Search Methods

```python
class BST:
    def __init__(self):
        self.root = None

    def search(self, data):
        return self.searchHelper(data, self.root)

    def searchHelper(self, data, root):
        if root is None:
            return False
        if root.data == data:
            return True
        if data < root.data:
            return self.searchHelper(data, root.left)
        else:
            return self.searchHelper(data, root.right)
```

### 2.1 Public Method `search(data)`

- **Interface**: User calls `bstObject.search(value)`.
- **What it does**: Delegates the actual search to the helper method `searchHelper`, passing the root of the tree (`self.root`) as the starting point.

### 2.2 Private/Helper Method `searchHelper(data, root)`

This is a recursive function that performs the BST search.  
**Parameters**:
- `data` – the value we are looking for.
- `root` – the current node being examined (initially the whole tree’s root).

**Algorithm (step‑by‑step)**:

1. **Base case – empty subtree**  
   `if root is None:`  
   → If we reach a `None`, the value is not in this branch (and thus not in the tree). Return `False`.

2. **Base case – value found**  
   `if root.data == data:`  
   → The current node holds the target value. Return `True`.

3. **Recursive step – go left or right**  
   Because the tree is a **binary search tree**, for any node:
   - All values in the left subtree are **less** than the node’s value.
   - All values in the right subtree are **greater** than the node’s value.

   - If `data < root.data` → the target, if present, must be in the **left** subtree.  
     Call `searchHelper(data, root.left)` and return its result.

   - Else (`data > root.data`) → the target, if present, must be in the **right** subtree.  
     Call `searchHelper(data, root.right)` and return its result.

> **Note**: The algorithm never uses `data == root.data` in the recursive part; that case is already handled in the base case.

### 2.3 Example Walkthrough

Assume we have inserted the values `20, 25, 10, 15` into the BST. The resulting tree looks like:

```
       20
      /  \
    10    25
      \
       15
```

Now call `bstObject.search(15)`:

1. `search(15)` calls `searchHelper(15, root)` where `root` is the node with `data=20`.
2. `root` is not `None`. Compare `15` with `20`: `15 < 20` → go left.
3. Recursive call: `searchHelper(15, node10)`.
4. `node10` is not `None`. Compare `15` with `10`: `15 > 10` → go right.
5. Recursive call: `searchHelper(15, node15)`.
6. `node15` is not `None`. Compare `15` with `15`: equal → return `True`.
7. The `True` propagates back up the call stack, finally returning `True` to the original caller.

If we search for `17`:
- It follows the same path (`20 → 10 → 15`), but at `node15` the comparison `17 > 15` leads to a right child, which is `None`.
- The `None` base case triggers, returning `False` all the way back.

---

## 3. Summary of the Search Algorithm

- **Input**: a target value and the root of a BST.
- **Output**: `True` if the value exists in the tree, `False` otherwise.
- **Time complexity**: **O(h)** where `h` is the height of the tree. In a balanced BST, `h ≈ log₂(n)`; in the worst case (skewed tree), `h = n`.
- **Space complexity**: **O(h)** due to recursion stack (iterative version would be O(1) extra space).

---

## 4. Connection to Other Parts (Brief Context)

The code also includes `insert` and `delete` methods, but they are **not** the focus here. They rely on the same recursive traversal and node structure. The `print_binary_tree` function is a simple utility to display the tree and is unrelated to the search logic.

The search part is independent and uses only the BST property and the node references.

