# Optimized Stack Implementation Using Linked List

## Core Structural Idea

The most efficient way to implement a stack using a singly linked list is to treat the **head node as the top of the stack**.
Instead of inserting elements at the tail, all operations occur at the **head of the list**.

This design eliminates traversal entirely and makes the most important stack operations constant time.

```text
Top (Head)
   │
   ▼
+------+   +------+   +------+   +------+
| 40   | → | 30   | → | 20   | → | 10   |
+------+   +------+   +------+   +------+
```

The element at the **head is always the stack top**.

---

# Why This Design Is Efficient

Singly linked lists allow extremely fast operations at the **front of the list** because the head pointer is directly accessible.

Operations at the tail require traversal because singly linked lists **do not have backward pointers**.

By using the head as the stack top:

* No traversal is required
* No tail pointer is required
* All stack operations become direct pointer manipulations

---

# Stack Structure

Each node contains two components.

| Field | Description                       |
| ----- | --------------------------------- |
| data  | Value stored inside the node      |
| next  | Pointer referencing the next node |

The stack maintains only one reference:

| Variable | Purpose                        |
| -------- | ------------------------------ |
| head     | Points to the top of the stack |

---

# Push Operation

## Concept

The `push` operation inserts a new node **at the beginning of the linked list**.

Since the head represents the stack top, inserting at the head automatically places the new element at the top.

---

## Step-by-Step Logical Process

1. Create a new node containing the value.
2. Set the new node's `next` pointer to the current head.
3. Move the head pointer to the new node.

---

## ASCII Visualization

### Initial Stack

```text
Top
 │
 ▼
+------+   +------+   +------+
| 30   | → | 20   | → | 10   |
+------+   +------+   +------+
```

---

### Push(40)

Create a new node.

```text
+------+
| 40   |
+------+
```

Attach it before the current head.

```text
Top
 │
 ▼
+------+   +------+   +------+   +------+
| 40   | → | 30   | → | 20   | → | 10   |
+------+   +------+   +------+   +------+
```

---

## Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| Push      | O(1)       |

Only pointer reassignment occurs, therefore traversal is unnecessary.

---

# Pop Operation

## Concept

The `pop` operation removes the node at the head of the linked list.

Because the head represents the stack top, removing it automatically removes the top element.

---

## Step-by-Step Logical Process

1. Store the current head node.
2. Move the head pointer to `head.next`.
3. Remove the old head node.

---

## ASCII Visualization

### Before Pop

```text
Top
 │
 ▼
+------+   +------+   +------+   +------+
| 40   | → | 30   | → | 20   | → | 10   |
+------+   +------+   +------+   +------+
```

---

### Pop Operation

The head moves forward.

```text
Top
 │
 ▼
+------+   +------+   +------+
| 30   | → | 20   | → | 10   |
+------+   +------+   +------+
```

Removed element: **40**

---

## Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| Pop       | O(1)       |

Only a single pointer update occurs.

---

# Top / Peek Operation

## Concept

The `peek` or `top` operation returns the value stored in the head node without removing it.

---

## ASCII Visualization

```text
Top
 │
 ▼
+------+   +------+   +------+
| 40   | → | 30   | → | 20   |
+------+   +------+   +------+
```

Top element returned: **40**

---

## Time Complexity

| Operation  | Complexity |
| ---------- | ---------- |
| Top / Peek | O(1)       |

The value is directly available through the head pointer.

---

# Is Empty Operation

## Concept

The stack is empty if the head pointer does not reference any node.

---

## ASCII Visualization

### Empty Stack

```text
Head → NULL
```

### Non-Empty Stack

```text
Head
 │
 ▼
+------+
| 10   |
+------+
```

---

## Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| isEmpty   | O(1)       |

Only a pointer comparison is required.

---

# Size Operation

## Concept

The `size` operation counts how many nodes exist in the linked list.

This requires traversing from the head until reaching the last node.

---

## ASCII Visualization

```text
Head
 │
 ▼
+------+   +------+   +------+   +------+
| 40   | → | 30   | → | 20   | → | 10   |
+------+   +------+   +------+   +------+
```

Traversal path:

```text
40 → 30 → 20 → 10
```

---

## Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| Size      | O(n)       |

Each node must be visited to count elements.

---

# Optimizing Size Operation

A stack can maintain an additional variable:

```text
stackSize
```

Every push increments this value and every pop decrements it.

---

## Optimized Behavior

| Operation | Complexity |
| --------- | ---------- |
| Size      | O(1)       |

The stored counter directly provides the number of elements.

---

# Final Complexity Summary

| Operation           | Complexity | Reason                            |
| ------------------- | ---------- | --------------------------------- |
| Push                | O(1)       | Insert directly at head           |
| Pop                 | O(1)       | Remove head node                  |
| Top / Peek          | O(1)       | Head pointer contains top element |
| Is Empty            | O(1)       | Head pointer comparison           |
| Size                | O(n)       | Requires traversal                |
| Size (with counter) | O(1)       | Maintained size variable          |

---

# Important Structural Advantage

This approach removes the need for a **tail pointer** entirely.

All stack operations are executed at the **head**, which is the only position in a singly linked list where constant time insertion and deletion are guaranteed.
