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

## 1. Deletion Methods in the BST Class

```python
def get_min_node(self, node):
    current = node
    while(current.left is not None):
        current = current.left
    return current

def delete_helper(self, data, root):
    if root is None:
        return None
    
    if data < root.data:
        root.left = self.delete_helper(data, root.left)
    elif data > root.data:
        root.right = self.delete_helper(data, root.right)
    else:
        # Node to delete found
        # Case 1: No left child
        if root.left is None:
            return root.right
        # Case 2: No right child
        elif root.right is None:
            return root.left
        
        # Case 3: Two children
        min_larger_node = self.get_min_node(root.right)
        root.data = min_larger_node.data
        root.right = self.delete_helper(min_larger_node.data, root.right)
    
    return root

def delete(self, data):
    self.root = self.delete_helper(data, self.root)
```

---

## 2. Helper Functions

### `get_min_node(node)`

- **Purpose**: Finds the node with the minimum value in a given subtree.
- **How**: Starting from `node`, repeatedly go to the left child until `left` is `None`. The last visited node is the minimum.
- **Used in**: Two‑child deletion case to find the **in‑order successor** (the smallest node in the right subtree).

### `delete_helper(data, root)`

Recursively finds the node with `data` and removes it. Returns the root of the (possibly changed) subtree.

**Algorithm outline**:

1. **Base case**: If `root is None` → return `None` (value not found).
2. **Search phase**: If `data < root.data` → recurse on left child, update `root.left`.  
   If `data > root.data` → recurse on right child, update `root.right`.  
   If `data == root.data` → we have found the node to delete.
3. **Deletion phase** (when node is found):
   - **Case 1**: Node has **no left child** → return its right child (which may be `None`). This effectively removes the node.
   - **Case 2**: Node has **no right child** → return its left child.
   - **Case 3**: Node has **two children**:
     - Find the **in‑order successor** (minimum node in the right subtree) using `get_min_node(root.right)`.
     - Copy the successor’s value into the current node.
     - Recursively delete that successor from the right subtree.
4. Return the (possibly updated) root.

---

## 3. Step‑by‑Step Walkthrough with Examples

We'll use the tree from earlier:

```
       20
      /  \
    10    25
      \
       15
```

### Example 1: Delete a leaf node (Case 1 or 2) – Delete `15`

1. `delete(15)` calls `delete_helper(15, root20)`.
2. `15 < 20` → go left. Call `delete_helper(15, node10)`.
3. `15 > 10` → go right. Call `delete_helper(15, node15.right)`.
4. `node15.right` is `None` → base case returns `None`.
5. Back in call with `node15`:  
   - `data == root.data` (15 == 15) → found node.
   - `root.left is None` (True) → return `root.right` (which is `None`).
6. Back in call with `node10`:  
   - The call returned `None`, so `node10.right = None`.  
   - Return `node10`.
7. Back in call with `root20`:  
   - `node20.left` is updated to `node10`.  
   - Return `node20`.

Resulting tree:
```
    20
   /  \
 10    25
```
(Leaf 15 is removed.)

### Example 2: Delete a node with one child (Case 2) – Delete `10`

Tree after previous deletion:

```
    20
   /  \
 10    25
```

1. `delete(10)` calls `delete_helper(10, root20)`.
2. `10 < 20` → left: `delete_helper(10, node10)`.
3. At `node10`, `data == root.data` → found node.
4. `root.left is None`? No (left is `None` actually? Wait: `node10` has no left child, but it has a right child? In this tree after deletion, node10 has no children. Let's check: after deleting 15, node10 had no right child. So node10 is a leaf. But the code will still go to Case 1 because `root.left is None` (true). It returns `root.right` which is `None`.
5. Back in call with `root20`, set `node20.left = None`. Return `node20`.

Result:
```
20
  \
   25
```
(10 is removed.)

### Example 3: Delete a node with two children – Delete `20`

Starting from original tree with 20,10,15,25.

```
       20
      /  \
    10    25
      \
       15
```

1. `delete(20)` calls `delete_helper(20, root20)`.
2. At `root20`, `data == root.data` (20==20) → found node.
3. Both children exist → **Case 3**.
   - `min_larger_node = get_min_node(root.right)`.  
     `root.right` is node `25`. Its minimum is itself because no left child → `min_larger_node` is node `25`.
   - `root.data = min_larger_node.data` → set node20's data to `25`.
   - Now we need to delete the original successor (node `25`) from the right subtree.  
     `root.right = delete_helper(25, root.right)`.
4. Recursively delete `25` from subtree rooted at `node25` (the original node).  
   Inside `delete_helper(25, node25)`:
   - Found node. `root.left is None` → return `root.right` (which is `None`).  
   So node25 is removed.
5. Back in the top‑level call, `root.right` is now `None`. Return the current node (which still points to left child node10, but its data is now `25`).

Final tree after deletion:

```
       25
      /
    10
      \
       15
```

Notice that the root is now `25`, with left child `10` and `10`'s right child `15`. This maintains BST order because all values in the left subtree (10,15) are less than 25, and the right subtree is empty.

---

## 4. Key Points

- **Three cases**:  
  1. **No left child** → replace with right child.  
  2. **No right child** → replace with left child.  
  3. **Two children** → replace value with in‑order successor (or predecessor) and then delete that successor.
- **Return values**: The helper always returns the (new) root of the subtree after deletion, allowing parent links to be updated.
- **Successor choice**: The code uses the **minimum node in the right subtree** (in‑order successor). An alternative would be the maximum in the left subtree.
- **Recursive deletion of successor**: After copying the successor’s value, the algorithm calls `delete_helper` on the right subtree to remove the original successor node. This ensures the successor is removed, and the BST property remains intact.
- **Time complexity**: **O(h)**, where `h` is the tree height. In the worst case (skewed tree), it's O(n); in a balanced tree, O(log n).
- **Space complexity**: **O(h)** due to recursion stack (iterative version possible but more complex).

---

## 5. Connection to Node and Search

- Deletion relies on the same search logic as `search` to locate the node to delete.
- It uses `get_min_node` to traverse the tree (moving only left) to find the successor.
- Node manipulation: updating `left`/`right` pointers and reassigning values.
- The delete algorithm preserves the BST invariant: for every node, left subtree values < node.data < right subtree values.

