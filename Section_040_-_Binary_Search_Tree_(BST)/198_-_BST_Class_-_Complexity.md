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


## 1. Complexity Analysis (Per Operation)

Let **n** be the number of nodes in the tree, and **h** be the height of the tree.  
- **Best case** (balanced tree): \(h = \Theta(\log n)\)  
- **Worst case** (skewed tree): \(h = \Theta(n)\)

### `insert(data)`
- **Time**: \(O(h)\)  
  – Recursively compares and goes left/right until an empty spot is found.
- **Space**: \(O(h)\)  
  – Due to the recursion stack (each recursive call adds a frame). An iterative version would be \(O(1)\).

### `search(data)`
- **Time**: \(O(h)\)  
  – Traverses the tree following comparisons.
- **Space**: \(O(h)\) (recursion) or could be \(O(1)\) if implemented iteratively.

### `delete(data)` (including `delete_helper` and `get_min_node`)
- **Time**: \(O(h)\)  
  – Locating the node costs \(O(h)\).  
  – If the node has two children, `get_min_node` traverses only left links from the right child, which is at most \(O(h)\).  
  – Then a recursive delete on the successor also costs \(O(h)\) in total.  
  – Overall, the dominant term is \(O(h)\).
- **Space**: \(O(h)\)  
  – Due to recursion in `delete_helper`. `get_min_node` is iterative (while loop) and uses \(O(1)\) space.

### `get_min_node(node)`
- **Time**: \(O(h')\) where \(h'\) is the height of the subtree rooted at `node`. In the worst case, it could traverse the entire left spine (up to \(h\)).
- **Space**: \(O(1)\) – iterative loop, no recursion.

### `print_binary_tree(node)`
- **Time**: \(O(n)\) – visits every node exactly once.
- **Space**: \(O(h)\) – recursion stack depth equals tree height.

---

## 2. Full Algorithm Explanation

### BSTNode Class
- Each node stores `data`, and references to `left` and `right` children (initially `None`).

### BST Class
- Maintains a `root` reference (initially `None`).

---

### Insertion Algorithm (`insert`, `insert_helper`)

1. **Start** from the root.
2. **Recursive rule**:
   - If the current node is `None`, create and return a new node (base case).
   - Compare the new data with the current node’s data:
     - If less → go left (`node.left = insert_helper(data, node.left)`)
     - If greater or equal → go right (`node.right = insert_helper(data, node.right)`)
3. **Return** the current node so that the parent can update its child reference.
4. The public `insert` updates `self.root` with the result.

**Invariant**: After insertion, the BST property holds: all nodes in the left subtree are smaller, all in the right subtree are larger (or equal, with duplicates going to the right).

---

### Search Algorithm (`search`, `searchHelper`)

1. Start from the root.
2. **Base cases**:
   - If current node is `None` → return `False` (not found).
   - If current node’s data equals target → return `True`.
3. **Recursive step**:
   - If target < current data → go left.
   - Else → go right.
4. Propagate the boolean result back up.

---

### Deletion Algorithm (`delete`, `delete_helper`, `get_min_node`)

Deletion is the most complex operation. It handles three cases:

1. **Node with no left child** → replace the node with its right child (which may be `None`).  
2. **Node with no right child** → replace the node with its left child.  
3. **Node with two children**:
   - Find the **in‑order successor** (smallest node in the right subtree) using `get_min_node(root.right)`.
   - Copy the successor’s data into the current node.
   - Recursively delete the successor from the right subtree (since the successor is now duplicated).

**Recursive search**:
- Traverse the tree to locate the node to delete, updating child pointers along the way.
- When found, apply the appropriate case and return the new subtree root.

**Key points**:
- The recursion ensures that after deletion, the tree remains a valid BST.
- Using the in‑order successor guarantees that the BST property is preserved.

---

### Print Tree Algorithm (`print_binary_tree`)

- Pre‑order traversal: print current node, then recursively print left subtree, then right subtree.
- For each node, it prints its data and the data of its left/right children (or “None” if absent).

---

## 3. Additional Considerations

- **Recursion depth**: For very deep trees (e.g., 10⁵ nodes), recursion may cause a stack overflow. Iterative implementations (e.g., using loops) would avoid this but are more complex.
- **Duplicates**: The code inserts duplicates to the right subtree, so they appear in the tree (no special handling).
- **Balance**: The BST is not self‑balancing, so repeated insertions in sorted order lead to a skewed tree, degrading performance to O(n) per operation.
- **Memory**: Each node uses constant extra space; total memory O(n).

---

## 4. Summary Table

| Operation  | Time Complexity (Worst) | Time Complexity (Best/Average) | Space Complexity |
|------------|-------------------------|--------------------------------|------------------|
| Insert     | O(n)                    | O(log n) (if balanced)         | O(h) (recursion) |
| Search     | O(n)                    | O(log n) (if balanced)         | O(h) (recursion) |
| Delete     | O(n)                    | O(log n) (if balanced)         | O(h) (recursion) |
| Get Min    | O(n)                    | O(log n) (if balanced)         | O(1)             |
| Print Tree | O(n)                    | O(n)                           | O(h) (recursion) |

**n** = number of nodes, **h** = height of the tree.

---
