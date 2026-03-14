# Linked List Operations (Singly Linked List)

## 1. Node Structure Definition

A linked list is a linear data structure where each element is stored in a node. Every node contains two components: the stored data and a reference pointing to the next node in the sequence. The last node always points to `None`, indicating termination of the list.

```python
class Node:
    """
    Node class represents a single element inside the linked list.
    Each node stores data and a reference to the next node.
    """

    def __init__(self, data):
        self.data = data      # actual value stored in the node
        self.next = None      # pointer to the next node
```

---

# INSERT OPERATIONS

## Insert at Head

Insertion at the beginning of the list requires updating the head pointer so that the new node becomes the first element.

```python
class LinkedList:

    def __init__(self):
        self.head = None      # initially the list is empty


    def insert_at_head(self, value):
        """
        Inserts a new node at the beginning of the linked list.

        Steps
        1. Create a new node.
        2. Point new node next reference to current head.
        3. Update head pointer to new node.
        """

        new_node = Node(value)      # create new node
        new_node.next = self.head   # link new node with current head
        self.head = new_node        # update head to new node
```

### Expected Output

```
Insert sequence: 10, 20, 30 using insert_at_head

Linked List:
30 -> 20 -> 10 -> None
```

---

## Insert at Tail

Insertion at the end requires traversing the list until the final node and attaching the new node.

```python
    def insert_at_tail(self, value):
        """
        Inserts a node at the end of the linked list.

        Steps
        1. Create new node.
        2. If list empty, head becomes new node.
        3. Traverse until last node.
        4. Attach new node to last node.
        """

        new_node = Node(value)

        if self.head is None:
            self.head = new_node
            return

        temp = self.head

        while temp.next is not None:
            temp = temp.next

        temp.next = new_node
```

### Expected Output

```
Insert sequence: 10, 20, 30 using insert_at_tail

Linked List:
10 -> 20 -> 30 -> None
```

---

## Insert in Between (Insert at Index)

Insertion at a specific position requires traversing until the node before the desired index.

```python
    def insert_at_index(self, index, value):
        """
        Inserts a node at a specific index.

        Steps
        1. If index is zero, perform insert_at_head.
        2. Traverse until node before desired position.
        3. Adjust pointers accordingly.
        """

        if index == 0:
            self.insert_at_head(value)
            return

        new_node = Node(value)

        temp = self.head
        count = 0

        while temp is not None and count < index - 1:
            temp = temp.next
            count += 1

        if temp is None:
            raise IndexError("Invalid index")

        new_node.next = temp.next
        temp.next = new_node
```

### Expected Output

```
Initial List:
10 -> 20 -> 40 -> None

Insert 30 at index 2

Result:
10 -> 20 -> 30 -> 40 -> None
```

---

# DELETE OPERATIONS

## Delete Head

Deleting the first node only requires updating the head pointer.

```python
    def delete_head(self):
        """
        Deletes the first node of the linked list.

        Steps
        1. Check if list empty.
        2. Move head pointer to next node.
        """

        if self.head is None:
            return

        self.head = self.head.next
```

### Expected Output

```
Before:
10 -> 20 -> 30 -> None

After delete_head():

20 -> 30 -> None
```

---

## Delete Tail

Deleting the last node requires finding the second last node.

```python
    def delete_tail(self):
        """
        Deletes the last node of the list.

        Steps
        1. If only one node exists, remove head.
        2. Traverse until second last node.
        3. Set second last node next reference to None.
        """

        if self.head is None:
            return

        if self.head.next is None:
            self.head = None
            return

        temp = self.head

        while temp.next.next is not None:
            temp = temp.next

        temp.next = None
```

### Expected Output

```
Before:
10 -> 20 -> 30 -> None

After delete_tail():

10 -> 20 -> None
```

---

## Delete by Index

Deleting by position requires reaching the node before the target node.

```python
    def delete_by_index(self, index):
        """
        Deletes a node using its index.

        Steps
        1. If index zero, delete head.
        2. Traverse until previous node.
        3. Skip the target node.
        """

        if self.head is None:
            return

        if index == 0:
            self.delete_head()
            return

        temp = self.head
        count = 0

        while temp.next is not None and count < index - 1:
            temp = temp.next
            count += 1

        if temp.next is None:
            raise IndexError("Invalid index")

        temp.next = temp.next.next
```

### Expected Output

```
Before:
10 -> 20 -> 30 -> 40 -> None

delete_by_index(2)

After:
10 -> 20 -> 40 -> None
```

---

## Delete by Value

Deletes the first node containing a specified value.

```python
    def delete_by_value(self, value):
        """
        Deletes the first node containing the given value.
        """

        if self.head is None:
            return

        if self.head.data == value:
            self.head = self.head.next
            return

        temp = self.head

        while temp.next is not None:
            if temp.next.data == value:
                temp.next = temp.next.next
                return
            temp = temp.next
```

### Expected Output

```
Before:
10 -> 20 -> 30 -> 40 -> None

delete_by_value(30)

After:
10 -> 20 -> 40 -> None
```

---

# SEARCH OPERATIONS

## Search by Index

Returns the value stored at a specific index.

```python
    def search_by_index(self, index):
        """
        Returns data stored at given index.
        """

        temp = self.head
        count = 0

        while temp is not None:
            if count == index:
                return temp.data
            temp = temp.next
            count += 1

        raise IndexError("Index out of range")
```

### Expected Output

```
Linked List:
10 -> 20 -> 30 -> None

search_by_index(1)

Output:
20
```

---

## Search by Value

Returns the index of a value if it exists.

```python
    def search_by_value(self, value):
        """
        Returns index of first occurrence of value.
        """

        temp = self.head
        index = 0

        while temp is not None:
            if temp.data == value:
                return index
            temp = temp.next
            index += 1

        return -1
```

### Expected Output

```
Linked List:
10 -> 20 -> 30 -> None

search_by_value(30)

Output:
2
```

---

# TRAVERSE OPERATION

## Print Linked List

Traversal means visiting every node sequentially.

```python
    def traverse(self):
        """
        Prints every element of the linked list sequentially.
        """

        temp = self.head

        while temp is not None:
            print(temp.data, end=" -> ")
            temp = temp.next

        print("None")
```

### Expected Output

```
Linked List:

10 -> 20 -> 30 -> 40 -> None
```
