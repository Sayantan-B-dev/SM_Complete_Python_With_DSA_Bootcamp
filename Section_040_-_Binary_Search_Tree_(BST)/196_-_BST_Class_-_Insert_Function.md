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

## 1. Insert Methods in the BST Class

```python
def insert(self, data):
    self.root = self.insert_helper(data, self.root)

def insert_helper(self, data, node):
    if node is None:
        newNode = BSTNode(data)
        return newNode
    
    if data < node.data:
        node.left = self.insert_helper(data, node.left)
    else:
        node.right = self.insert_helper(data, node.right)

    return node
```

### 1.1 Public Method `insert(data)`

- **Interface**: User calls `bstObject.insert(value)`.
- **What it does**: Calls the recursive helper `insert_helper` with the root of the tree (`self.root`) and the data to insert.
- **Important**: It updates `self.root` with the return value of the helper. This ensures that if the tree was empty, the new node becomes the root; otherwise, the root remains unchanged (but the helper returns the same root after insertion).

### 1.2 Helper Method `insert_helper(data, node)`

This recursive function inserts the `data` into the subtree rooted at `node` and returns the (possibly new) root of that subtree.

**Algorithm (step‑by‑step)**:

1. **Base case – empty subtree**  
   `if node is None:`  
   → We have reached a position where the new node should be attached. Create a new `BSTNode` with the given `data` and return it. This new node will be linked as a child of the parent node in the recursion step above.

2. **Recursive step – decide direction**  
   Because the tree is a binary search tree:
   - If `data < node.data` → the new value belongs in the **left** subtree.  
     Recursively call `insert_helper(data, node.left)`.  
     The result of that call is the new left subtree (which might be the same as before or a new node if the left child was `None`).  
     Set `node.left =` that result.
   - Else (`data >= node.data`) → the new value belongs in the **right** subtree.  
     Recursively call `insert_helper(data, node.right)` and set `node.right =` that result.

3. **Return the current node**  
   After updating the appropriate child, return the current `node`. This allows the parent call to attach this node (or the newly created node) correctly.

---

## 2. Step‑by‑Step Walkthrough

We start with an empty BST: `self.root = None`.

### Insert 20
- `insert(20)` calls `insert_helper(20, None)`.
- `node is None` → create a new node with data `20` and return it.
- `insert` sets `self.root =` that new node.

Tree:
```
20
```

### Insert 25
- `insert(25)` calls `insert_helper(25, root)` where `root` is node `20`.
- `node` is not `None`. Compare: `25 < 20`? No → go right.
- Recursive call: `insert_helper(25, node.right)`. `node.right` is currently `None`.
- In that call, `node is None` → create new node with data `25`, return it.
- Back to the previous call: `node.right =` the new node (the returned node). Return `node` (still node `20`).
- `insert` sets `self.root` to node `20` (unchanged).

Tree:
```
20
  \
   25
```

### Insert 10
- `insert(10)` calls `insert_helper(10, node20)`.
- Compare: `10 < 20` → go left.
- Recursive call: `insert_helper(10, node20.left)`. `node20.left` is `None`.
- In that call, create new node with data `10`, return it.
- Back: `node20.left =` new node. Return node20.
- Tree:
```
    20
   /  \
 10    25
```

### Insert 15
- `insert(15)` calls `insert_helper(15, node20)`.
- `15 < 20` → go left.
- Recursive call: `insert_helper(15, node10)`.
- `15 < 10`? No → go right.
- Recursive call: `insert_helper(15, node10.right)`. `node10.right` is `None`.
- Create new node `15`, return it.
- Back: `node10.right =` new node. Return node10.
- Back to top: node20.left already points to node10, so node20 unchanged. Return node20.

Final tree:
```
       20
      /  \
    10    25
      \
       15
```

---

## 3. Key Points

- **Recursive nature**: The insertion algorithm follows the BST property recursively until it finds an empty spot (`None`), then creates and returns the new node.
- **Return values**: Each recursive call returns the root of the subtree after insertion. This allows linking back up the tree.
- **Handling duplicates**: The code uses `else` (i.e., `data >= node.data`) for the right branch, so duplicates will be placed in the right subtree. Some implementations treat duplicates differently (e.g., ignore, store count), but here duplicates are allowed and inserted to the right.
- **Time complexity**: **O(h)** where `h` is the height of the tree. In a balanced BST, `h ≈ log₂(n)`; in the worst case (skewed), `h = n`.
- **Space complexity**: **O(h)** due to recursion stack. An iterative version would use O(1) extra space.

---

## 4. Connection to Node and Search

- The `BSTNode` class is the building block; insertion creates nodes and sets their `left` and `right` pointers.
- The search algorithm traverses the tree in the same way as insertion (comparing and going left/right) but stops when the value is found or a `None` is reached.
- Both algorithms rely on the BST invariant: left child < current node < right child.
