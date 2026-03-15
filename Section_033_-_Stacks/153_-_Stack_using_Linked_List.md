
# Stack Implementation Using Linked List

## Structural Concept

A stack implemented with a linked list stores elements in **nodes connected through pointers** rather than contiguous memory locations. Each node contains two fields:

| Field  | Description                                     |
| ------ | ----------------------------------------------- |
| `data` | The value stored in the node                    |
| `next` | Reference pointing to the next node in the list |

The stack follows **LIFO (Last In First Out)** semantics. In a linked list based stack, the **tail node represents the top of the stack**.

```
Head                                     Tail (Top)
 │                                          │
 ▼                                          ▼
+------+   +------+   +------+   +------+
| 10   | → | 20   | → | 30   | → | 40   |
+------+   +------+   +------+   +------+
```

Element `40` is the top of the stack because it was inserted last.

---

# Maintaining Tail Pointer

A linked list stack must maintain two references:

| Pointer | Purpose                              |
| ------- | ------------------------------------ |
| `head`  | Starting node of the linked list     |
| `tail`  | Last node representing the stack top |

Without maintaining a tail pointer, many operations become inefficient because traversal would be required to reach the top element.

---

# Push Operation

## Concept

The `push` operation inserts a new node at the **top of the stack**, which corresponds to the **tail of the linked list**.

### Step-by-Step Logical Behavior

1. Create a new node containing the new value.
2. Set the `next` pointer of the current tail node to the new node.
3. Move the tail reference to the new node.

---

## ASCII Visualization

### Initial Stack

```
Head                              Tail
 │                                  │
 ▼                                  ▼
+------+   +------+   +------+
| 10   | → | 20   | → | 30   |
+------+   +------+   +------+
```

### Push(40)

A new node is created.

```
New Node
+------+
| 40   |
+------+
```

Current tail connects to the new node.

```
Head                              Tail
 │                                  │
 ▼                                  ▼
+------+   +------+   +------+   +------+
| 10   | → | 20   | → | 30   | → | 40   |
+------+   +------+   +------+   +------+
```

---

## Time Complexity

### Without Tail Pointer

If only the head pointer exists, inserting at the end requires traversing the entire list.

```
Head → 10 → 20 → 30 → NULL
```

Traversal required to find the last node.

| Operation | Complexity |
| --------- | ---------- |
| Push      | O(n)       |

If push is executed repeatedly in nested contexts or loops, cumulative behavior may appear as **O(n²)**.

---

### With Tail Pointer

The tail pointer directly references the top node.

```
Tail → 30
```

Insertion becomes immediate.

| Operation | Complexity |
| --------- | ---------- |
| Push      | O(1)       |

---

# Size Operation

## Concept

The `size` operation counts how many nodes exist in the linked list.

Traversal begins from the head and continues until the end of the list.

---

## ASCII Visualization

```
Head
 │
 ▼
+------+   +------+   +------+   +------+
| 10   | → | 20   | → | 30   | → | 40   |
+------+   +------+   +------+   +------+
```

Traversal path:

```
10 → 20 → 30 → 40
```

Each node increments a counter.

---

## Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| Size      | O(n)       |

The algorithm must visit every node to determine the count.

Note: Some implementations maintain a **separate counter variable** to achieve **O(1)** complexity.

---

# Is Empty Operation

## Concept

The stack is empty when **no nodes exist**.

This condition is detected by checking whether the head pointer is `NULL`.

---

## ASCII Visualization

### Empty Stack

```
Head → NULL
Tail → NULL
```

### Non-Empty Stack

```
Head                     Tail
 │                        │
 ▼                        ▼
+------+
| 10   |
+------+
```

---

## Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| isEmpty   | O(1)       |

The check requires only a single pointer comparison.

---

# Peek / Top Operation

## Concept

The `top` operation retrieves the value stored at the **top of the stack** without removing it.

Since the stack top corresponds to the **tail node**, the tail reference directly provides access.

---

## ASCII Visualization

```
Head                              Tail
 │                                  │
 ▼                                  ▼
+------+   +------+   +------+   +------+
| 10   | → | 20   | → | 30   | → | 40   |
+------+   +------+   +------+   +------+
                                       ▲
                                   Top Element
```

The top element returned is `40`.

---

## Time Complexity

| Operation  | Complexity |
| ---------- | ---------- |
| Peek / Top | O(1)       |

No traversal is required because the tail pointer directly identifies the top node.

---

# Pop Operation

## Concept

The `pop` operation removes the **top node**, which corresponds to the **tail node**.

However, a singly linked list has **only forward pointers**. There is no pointer from a node to its previous node.

This creates a challenge when removing the tail.

---

# Why Moving Tail Back Is Difficult

In a singly linked list:

```
10 → 20 → 30 → 40
```

Each node only knows its **next node**, not its previous one.

```
20 knows 30
30 knows 40
40 knows NULL
```

The node before `40` cannot be identified directly.

---

# Pop Operation Strategy

To remove the tail node:

1. Start traversal from the head.
2. Move forward until reaching the **second last node**.
3. Update the second last node's `next` pointer to `NULL`.
4. Move the tail pointer to the second last node.

---

# ASCII Visualization

### Before Pop

```
Head                              Tail
 │                                  │
 ▼                                  ▼
+------+   +------+   +------+   +------+
| 10   | → | 20   | → | 30   | → | 40   |
+------+   +------+   +------+   +------+
```

---

### Traversal to Find Second Last Node

```
Traversal Path

10 → 20 → 30
              ▲
      Second Last Node
```

---

### Remove Tail

Pointer update:

```
30.next → NULL
```

---

### After Pop

```
Head                       Tail
 │                          │
 ▼                          ▼
+------+   +------+   +------+
| 10   | → | 20   | → | 30   |
+------+   +------+   +------+
```

The removed element was `40`.

---

# Pop Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| Pop       | O(n)       |

Traversal from head to the second last node requires visiting multiple nodes.

---

# Operation Complexity Summary

| Operation  | Complexity             | Reason                                        |
| ---------- | ---------------------- | --------------------------------------------- |
| Push       | O(1) with tail pointer | Direct insertion at tail                      |
| Size       | O(n)                   | Traversal required to count nodes             |
| Is Empty   | O(1)                   | Single pointer check                          |
| Peek / Top | O(1)                   | Tail pointer directly accessed                |
| Pop        | O(n)                   | Traversal required to locate second last node |

---

# Important Structural Observation

When stacks are implemented using linked lists in most practical systems, **the head node is treated as the stack top** instead of the tail. This allows both `push` and `pop` operations to occur at the head with **O(1)** complexity.

However, when the **tail is treated as the stack top**, pop becomes inefficient because singly linked lists cannot move backward without traversal.
