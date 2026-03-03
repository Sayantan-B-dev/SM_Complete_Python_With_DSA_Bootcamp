## 1. Fundamental of Programming


### 1. Flowchart and Pseudocode
- **Basic of Problem Solving - Flowchart and Pseudocode.pdf**: Guide on representing algorithms using flowcharts and pseudocode.
- **Pseudocode**: Structured representation of algorithms using natural language and programming constructs.
  ```
  START
  INPUT values
  PROCESS (e.g., compute sum)
  OUTPUT result
  END
  ```
### 1.2 Searching and Sorting
- **Linear Search**: Algorithm to find a target element by checking each element sequentially.
  ```
  for each index i from 0 to length-1:
      if array[i] == target:
          return i
  return -1
  ```
- **Binary Search**: Efficient search on sorted array by repeatedly dividing the search interval in half.
  ```
  low = 0, high = n-1
  while low <= high:
      mid = (low + high) // 2
      if array[mid] == target: return mid
      elif array[mid] < target: low = mid + 1
      else: high = mid - 1
  return -1
  ```
- **Bubble Sort**: Repeatedly steps through the list, compares adjacent elements and swaps them if they are in the wrong order.
  ```
  for i from 0 to n-1:
      for j from 0 to n-i-2:
          if array[j] > array[j+1]:
              swap(array[j], array[j+1])
  ```
- **Insertion Sort**: Builds the final sorted array one item at a time by inserting each element into its proper position.
  ```
  for i from 1 to n-1:
      key = array[i]
      j = i-1
      while j >= 0 and array[j] > key:
          array[j+1] = array[j]
          j = j-1
      array[j+1] = key
  ```
- **Selection Sort**: Divides the list into sorted and unsorted regions, repeatedly selects the smallest element from the unsorted region and moves it to the sorted region.
  ```
  for i from 0 to n-1:
      min_index = i
      for j from i+1 to n-1:
          if array[j] < array[min_index]:
              min_index = j
      swap(array[i], array[min_index])
  ```
- **Introduction to Recursion**: Concept of a function calling itself to solve smaller instances of the same problem.
  ```
  recursive_function(parameters):
      if base_condition: return base_value
      else: return operation(recursive_function(modified_parameters))
  ```
- **Searching and Sorting - Updated.pdf**: Comprehensive notes covering linear search, binary search, bubble sort, insertion sort, selection sort, and recursion concepts.
- **Searching and Sorting.pdf**: Foundational document on searching and sorting algorithms with examples and complexity analysis.


### 2. Arrays and List
- **Introduction to Arrays**: Definition of arrays as contiguous memory locations storing elements of the same type.
  ```
  create array of size n
  for i from 0 to n-1:
      array[i] = initial_value
  access: array[index]
  ```
- **Custom List Class**: Implementation of a dynamic list with methods like append, insert, remove, etc.
  ```
  class CustomList:
      initialize internal array and capacity
      append(item):
          if full: resize
          add item at end
      insert(index, item):
          shift elements right from index
          place item
      remove(item):
          find index, shift left
  ```
- **Dynamic Behaviour of List**: Explanation of how Python lists grow dynamically by overallocation and resizing.
  ```
  when appending and capacity reached:
      new_capacity = old_capacity * growth_factor
      allocate new array
      copy old elements
      add new element
  ```
- **Introduction to arrays.pdf**: Notes on array fundamentals, memory layout, and basic operations.


## 2. Recursion

### 1. Introduction to Recursion
- **Intro To Recursion**: Basics of recursion, function calling itself.
- **Function Calling itself**: Example of a function that calls itself.
- **Factorial with Recursion**: Compute n! recursively.
  ```
  factorial(n):
      if n==0: return 1
      else: return n * factorial(n-1)
  ```
- **PMI and Recursion**: Relation between recursion and principle of mathematical induction.
- **Sum of n natural numbers**: Compute sum 1+2+...+n using recursion.
  ```
  sum(n):
      if n==1: return 1
      else: return n + sum(n-1)
  ```
- **Number of Digits**: Count digits of a number recursively.
  ```
  countDigits(n):
      if n<10: return 1
      else: return 1 + countDigits(n//10)
  ```
- **Fibonacci**: Compute nth Fibonacci number recursively.
  ```
  fib(n):
      if n<=1: return n
      else: return fib(n-1)+fib(n-2)
  ```
- **Print 1 to N**: Print numbers from 1 to n using recursion.
- **Print N to 1**: Print numbers from n down to 1.
- **Sum of Digits**: Compute sum of digits recursively.
- **Power of a number**: Compute a^b recursively.
- **Head vs Tail Recursion**: Explanation of recursion where the recursive call is the last operation (tail) or before other operations (head).

### 2. Recursion and Arrays
- **Check if Array is Sorted**: Recursive check whether array is sorted in increasing order.
  ```
  isSorted(arr, i):
      if i == len(arr)-1: return True
      return arr[i] <= arr[i+1] and isSorted(arr, i+1)
  ```
- **Sum of Array using Recursion**: Compute sum of array elements recursively.
- **First and Last Indices of Element**: Find first and last occurrence of a given element using recursion.
- **All indices of a number**: Return list of all indices where element appears.
- **Update in List all Index of An Element**: Update values at all indices of a given element.
- **Update Indices in a Global List**: Use global list to collect indices.
- **Return All indices in a new List**: Return a new list containing all indices.

### 3. Recursion - Searching and Sorting
- **Linear Search Using Recursion**: Recursive implementation of linear search.
- **Binary Search using Recursion**: Recursive binary search.
- **Merge Sort**: Divide-and-conquer sorting algorithm.
  ```
  mergeSort(arr, l, r):
      if l < r:
          mid = (l+r)//2
          mergeSort(arr, l, mid)
          mergeSort(arr, mid+1, r)
          merge(arr, l, mid, r)
  ```
- **Quick Sort**: Divide-and-conquer sorting with pivot.
  ```
  quickSort(arr, l, r):
      if l < r:
          pi = partition(arr, l, r)
          quickSort(arr, l, pi-1)
          quickSort(arr, pi+1, r)
  partition(arr, l, r):
      pivot = arr[r]
      i = l-1
      for j from l to r-1:
          if arr[j] <= pivot:
              i++; swap arr[i], arr[j]
      swap arr[i+1], arr[r]
      return i+1
  ```

### 4. Recursion and String
- **Recursion With Strings**: Recursive operations on strings (length, reverse, etc.).
- **Palindrome**: Check if a string is palindrome recursively.
  ```
  isPalindrome(s, i, j):
      if i >= j: return True
      if s[i] != s[j]: return False
      return isPalindrome(s, i+1, j-1)
  ```
- **Strings, Substring and Subsequence**: Concepts of substrings and subsequences.
- **Return Subsequences of a string**: Generate all subsequences.
  ```
  subsequences(s):
      if s is empty: return [""]
      first = s[0]
      rest = subsequences(s[1:])
      return rest + [first + sub for sub in rest]
  ```
- **Print all subsequences**: Print rather than return.
- **Permutations of a string**: Generate all permutations.
  ```
  permutations(s):
      if len(s)<=1: return [s]
      result = []
      for i,ch in enumerate(s):
          for perm in permutations(s[:i]+s[i+1:]):
              result.append(ch+perm)
      return result
  ```
- **Return Permutations of a string**: Return list of permutations.
- **Words from a phone number**: Given digit string, return all possible letter combinations (like old phone keypad).
- **Return All Codes**: Convert string to possible alphabet codes (1=a, 2=b, ... 26=z).

## 3. Linked List
- **Introduction To Linked List**: Linear data structure where elements are nodes containing data and pointer to next node.
- **Node of a LL**: Node class with data and next reference.
- **Print LL**: Traverse and print elements.
  ```
  current = head
  while current:
      print(current.data)
      current = current.next
  ```
- **Take Input**: Build linked list from user input or array.
- **Length of LL**: Count nodes.
- **Insert in LL**: Insert at given position.
- **Delete a Node in LL**: Remove node at given position.
- **Introduction To Data Structures**: Overview of data structures.
- **Linked List Class**: Complete class with methods.
- **Linked List I Notes.pdf**: Notes on linked list basics.
- **Search in Linked List**: Find element in list.

## 4. Linked List II
- **Middle Of Linked List**: Find middle node using two pointers (slow and fast).
  ```
  slow = head, fast = head
  while fast and fast.next:
      slow = slow.next
      fast = fast.next.next
  return slow
  ```
- **Middle of LL - 2 Pointer**: Same as above.
- **Merge 2 sorted Linked List**: Merge two sorted lists into one sorted list.
- **Reverse Linked List (Recursive)**: Reverse list using recursion.
- **Reverse Linked List (Iterative)**: Reverse list by changing links iteratively.
- **Merge Sort LL**: Merge sort on linked list.
- **Doubly Linked List**: Node with prev and next pointers.
- **Linekd List 2.pdf**: Notes on advanced linked list problems.
- **common.py**: Common utilities.

## 5. Stack
- **Stack Using List**: Stack implementation using Python list (push/pop).
- **Stack Using Linked list**: Stack using linked list nodes.
- **Stack Data Structure.pdf**: Notes on stack operations and applications.
- **Test.py**: Stack test code.

## 6. Queues
- **Queue Using List**: Queue using list (enqueue/dequeue) – inefficient due to O(n) pop(0).
- **Queue Using Linked list**: Queue using linked list (front and rear pointers).
- **Queue Data Structure.pdf**: Notes on queue operations and applications.
- **Queues.py**: Queue implementation.

## 7. Trees — Generic
- **TreeNode**: Node structure for generic tree (data and list of children).
- **Print a Tree**: Print generic tree recursively.
- **Take Input of Tree**: Build generic tree from user input.
- **Take Input better**: Improved input method.
- **Trees Questions**: Problems on generic trees.
- **Traversals of Tree**: Preorder, postorder, level-order traversals.
- **Generic Trees.pdf**: Notes on generic trees.
- **commons.py**: Common helper functions.
- **genericTreesInput.py**: Input functions for generic trees.

## 8. Binary Tree
- **Types of binary Trees**: Full, complete, perfect, balanced, degenerate.
- **Introduction To Binary Tree**: Binary tree structure (each node has left and right children).
- **BinaryTreeNode**: Node class for binary tree.
- **Take Input Binary Tree**: Build binary tree from user input.
- **(HW) Print Level Wise**: Level-order traversal using queue.
- **Diameter of the Tree**: Longest path between any two nodes.
  ```
  diameter(node):
      if node is None: return 0
      leftHeight = height(node.left)
      rightHeight = height(node.right)
      leftDiameter = diameter(node.left)
      rightDiameter = diameter(node.right)
      return max(leftHeight+rightHeight+1, leftDiameter, rightDiameter)
  ```
- **Balanced Binary Tree Check**: Check if tree is height-balanced (difference <=1).
- **Binary Tree Traversal**: Inorder, preorder, postorder.
- **Construct Tree from Preorder and Inorder**: Reconstruct binary tree given preorder and inorder traversals.
- **Construct a tree from Postorder and Inorder**: Reconstruct from postorder and inorder.
- **Binary Tree Notes.pdf**: Comprehensive notes on binary trees.
- **predefinedBT.py**: Predefined binary trees for testing.

## 9. Binary Search Tree
- **Introduction to Binary Search Tree**: A binary tree where left child < parent < right child for all nodes.
- **BST Node and Printing**: Structure of a BST node (data, left, right) and methods to print the tree (inorder, preorder, postorder).
  ```
  inorder(node):
      if node is None: return
      inorder(node.left)
      print(node.data)
      inorder(node.right)
  ```
- **Search in a BST**: Recursive or iterative search for a value in a BST.
  ```
  search(root, key):
      if root is None or root.data == key: return root
      if key < root.data: return search(root.left, key)
      else: return search(root.right, key)
  ```
- **Sorted Array to BST**: Build a balanced BST from a sorted array.
  ```
  buildBST(arr, start, end):
      if start > end: return None
      mid = (start+end)//2
      node = new Node(arr[mid])
      node.left = buildBST(arr, start, mid-1)
      node.right = buildBST(arr, mid+1, end)
      return node
  ```
- **Check BST (Optimized)**: Validate whether a binary tree is a BST using range limits.
  ```
  isBST(node, min, max):
      if node is None: return True
      if node.data <= min or node.data >= max: return False
      return isBST(node.left, min, node.data) and isBST(node.right, node.data, max)
  ```
- **Check BST**: Basic approach using inorder traversal to check if sorted.
  ```
  inorder traversal yields list
  if list is sorted in increasing order: valid BST
  ```
- **Print in Range**: Print all keys in a BST that lie within a given range [k1, k2].
  ```
  printInRange(node, k1, k2):
      if node is None: return
      if node.data > k1: printInRange(node.left, k1, k2)
      if k1 <= node.data <= k2: print(node.data)
      if node.data < k2: printInRange(node.right, k1, k2)
  ```
- **BST Class**: Implementation of a BST with methods like insert, delete, search, etc.
- **predefinedBSTs.py**: Predefined BST structures for testing and examples.

## 10. Hashmaps
- **Introduction to Hashmaps**: Data structure that stores key-value pairs, providing average O(1) insertion, deletion, and lookup.
- **Dictionaries Question**: Problems involving Python dictionaries, e.g., counting frequencies, grouping.
- **Dictionaries Question Answer**: Solutions to dictionary-based problems.
- **Hashmap using Open Addressing**: Hash table implementation where collisions are resolved by probing (linear, quadratic, double hashing).
  ```
  insert(key, value):
      index = hash(key) % size
      while slot[index] is occupied and slot[index].key != key:
          index = probe(index)
      slot[index] = (key, value)
  ```
- **Hashmap using Chaining**: Hash table where each bucket contains a linked list of entries.
  ```
  insert(key, value):
      index = hash(key) % size
      append (key, value) to linked list at bucket[index]
  ```
- **LinkedListForChaining.py**: Linked list implementation used for chaining in hashmap.
- **Hashmaps.pdf / Hashmaps_Notes.pdf**: Detailed notes on hashing techniques, collision resolution, and complexity.
- **hash_of_a_value.py**: Demonstration of computing hash values for objects.

## 11. Graphs
- **Introduction to Graphs**: Definition of graph as vertices and edges, directed/undirected, weighted/unweighted.
- **Graph Using Adjacency List**: Graph representation where each vertex stores a list of neighbors.
  ```
  class Graph:
      def __init__(self):
          self.adj = {}
      add_edge(u, v):
          self.adj[u].append(v)
          (for undirected also add v->u)
  ```
- **Graph Using Adjacency Matrix**: Graph representation using a 2D matrix where matrix[i][j] indicates edge presence.
  ```
  initialize VxV matrix with 0
  add_edge(i,j): matrix[i][j] = 1 (or weight)
  ```
- **Graph Traversal - BFS**: Breadth‑first search using a queue.
  ```
  BFS(start):
      visited = set()
      queue = [start]
      while queue:
          v = queue.pop(0)
          if v not visited:
              visited.add(v)
              for neighbor in adj[v]:
                  if neighbor not visited:
                      queue.append(neighbor)
  ```
- **Graph Traversal - DFS**: Depth‑first search using stack or recursion.
  ```
  DFS(v):
      visited.add(v)
      for neighbor in adj[v]:
          if neighbor not visited:
              DFS(neighbor)
  ```
- **Graph.pdf**: Comprehensive notes on graph theory and algorithms.
- **Test.py**: Possibly test code for graph operations.

## 12. Dynamic Programming
- **Introduction to DP**: Optimization technique that solves problems by breaking them into overlapping subproblems and storing results.
  ```
  DP approach:
      define state
      recurrence relation
      base case
      compute bottom‑up or top‑down with memoization
  ```
- **Count of Balanced Binary Tree of Given height H**: Count number of balanced binary trees of height H (where difference between left and right subtree heights ≤ 1).
  ```
  dp[0] = 1  (height 0)
  dp[1] = 2  (height 1)
  for h from 2 to H:
      dp[h] = dp[h-1] * (2*dp[h-2] + dp[h-1])
  ```
- **Dynamic Programming.pdf**: Notes on DP concepts and examples.
- **Fibonacci Number - Bottom up**: Compute Fibonacci numbers iteratively.
  ```
  fib[0]=0, fib[1]=1
  for i=2 to n:
      fib[i]=fib[i-1]+fib[i-2]
  ```
- **Min Cost Path**: Find minimum cost path from top-left to bottom-right in a grid moving right/down.
  ```
  dp[i][j] = cost[i][j] + min(dp[i-1][j], dp[i][j-1])
  ```
- **Minimum steps to 1**: Given a number, reduce to 1 by subtracting 1, dividing by 2 (if even), or dividing by 3 (if divisible by 3), minimize steps.
  ```
  dp[1]=0
  for i=2 to n:
      dp[i]=1+dp[i-1]
      if i%2==0: dp[i]=min(dp[i], 1+dp[i//2])
      if i%3==0: dp[i]=min(dp[i], 1+dp[i//3])
  ```
