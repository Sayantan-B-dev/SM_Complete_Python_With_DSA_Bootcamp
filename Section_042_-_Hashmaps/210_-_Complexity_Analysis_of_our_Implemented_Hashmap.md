```py

class LLNode:
    def __init__(self,key,value):
        self.key = key
        self.value = value
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def add(self,key,value):
        new_node = LLNode(key,value)
        new_node.next = self.head
        self.head = new_node

    def search(self,key):
        current = self.head
        while current is not None:
            if(current.key == key):
                return current
            current = current.next
        
        return None

    def delete(self,key):
        current = self.head
        previous = None

        while current is not None:
            if(current.key == key):
                if previous is None: # Removing the Head
                    self.head = current.next
                else:
                    previous.next = current.next
                return True
            previous = current
            current = current.next

        return False

    def traverse(self):
        current = self.head
        while current is not None:
            print(f"{current.key}: {current.value} -->" , end = " ")
            current = current.next
        
        print("None")


class HashMapUsingChaining:
    def __init__(self,capacity=13):
        self.capacity = capacity
        self.size=0
        self.buckets = self.__create_buckets(self.capacity)

    def __create_buckets(self,capacity):
        buckets = [LinkedList() for i in range(capacity)]
        return buckets
    
    def hash_function(self,key):
        return abs(hash(key)) % self.capacity

    def rehash(self):
        # print("Rehashing ...")
        old_buckets = self.buckets
        self.capacity = self.capacity*2
        self.buckets = self.__create_buckets(self.capacity)
        self.size = 0 # Reset Size before reinserting elements

        for eachLinkedListHead in old_buckets:
            current = eachLinkedListHead.head
            while current is not None:
                self.insert(current.key,current.value)
                current = current.next



    def insert(self,key,value):
        bucket_index = self.hash_function(key)
        bucket = self.buckets[bucket_index]

        node = bucket.search(key)

        if node is None:
            bucket.add(key,value)
            self.size +=1
        else:
            node.value = value # update existing Key with New Value

        load_factor = self.size / self.capacity

        # print("Load Factor", load_factor)
        if(load_factor > 0.75):
            self.rehash()
            # print("Rehashing Done")

    def get(self,key):
        bucket_index = self.hash_function(key)
        bucket = self.buckets[bucket_index]

        node = bucket.search(key)
        if(node is not None):
            return node.value
        else:
            return "Key not found"
        
    def remove(self,key):
        bucket_index = self.hash_function(key)
        bucket = self.buckets[bucket_index]

        removed = bucket.delete(key)
        if(removed ==True):
            self.size-=1
            print(f"Key {key} is removed from Hashmap")
        else:
            print(f"Key {key} not found in Hashmap")
    
    def __len__(self):
        return self.size
    
    def __str__(self):
        for i in range(self.capacity):
            if(self.buckets[i] != None):
                self.buckets[i].traverse()
            
        return ""
    
    def __setitem__(self,key,value):
        return self.insert(key,value)



hash_map = HashMapUsingChaining(3)
# print((hash_map))
hash_map.insert("Apple",100)
hash_map.insert("Orange",105)
hash_map.insert("banana",200)
hash_map.insert("banana2",200) # 0.8
hash_map.insert("banana3",200)

# print((hash_map))
print(hash_map.get("Apple")) # Key should be case sensitive
print(hash_map.get("Orange"))
print(hash_map.get("banana"))

hash_map.remove("Apple")
print(hash_map.get("Apple")) 
print(len(hash_map))
print(hash_map)


```
## Time and Space Complexity Analysis

### 1. `LLNode.__init__(self, key, value)`
- **Time Complexity:** O(1) – simply initializes attributes.
- **Space Complexity:** O(1) – allocates a single node.
- **Math:** Constant time/space.
- **Easy:** Just creating a box to store a key‑value pair.

---

### 2. `LinkedList.add(self, key, value)`
- **Time Complexity:** O(1) – adds new node at the head; no traversal needed.
- **Space Complexity:** O(1) – only one new node created.
- **Math:** Insertion at head is independent of list length.
- **Easy:** Always puts new item at the front, like stacking papers on a desk.

---

### 3. `LinkedList.search(self, key)`
- **Time Complexity:** O(n) in worst case (where `n` = length of this linked list).  
  Average case for a hash map with good distribution is O(1) because chains are short.
- **Space Complexity:** O(1) – uses only a few pointers.
- **Math:** Must walk through the list until key found; in worst case we visit all `n` nodes.
- **Easy:** Might have to look at every item in the chain until we find the right one.

---

### 4. `LinkedList.delete(self, key)`
- **Time Complexity:** O(n) in worst case – same as search, needs to locate the node and adjust pointers.
- **Space Complexity:** O(1) – only a few temporary variables.
- **Math:** Requires traversal to find the node, then pointer update.
- **Easy:** We must find the item first, then remove it; possibly scanning the whole chain.

---

### 5. `LinkedList.traverse(self)`
- **Time Complexity:** O(n) – visits each node once.
- **Space Complexity:** O(1) – no extra storage (only printing).
- **Math:** Loop runs exactly as many times as there are nodes.
- **Easy:** Walk through all items in the list and print them.

---

### 6. `HashMapUsingChaining.__init__(self, capacity=13)`
- **Time Complexity:** O(capacity) – creates a list of `capacity` empty linked lists.
- **Space Complexity:** O(capacity) – the bucket array.
- **Math:** The constructor loops `capacity` times to initialize each bucket.
- **Easy:** Sets up a shelf with a fixed number of slots (buckets).

---

### 7. `HashMapUsingChaining.__create_buckets(self, capacity)`
- **Time Complexity:** O(capacity) – same reason as above.
- **Space Complexity:** O(capacity) – returns a list of `capacity` linked lists.
- **Math:** Simply builds an array of empty lists.
- **Easy:** Makes a row of empty chains.

---

### 8. `HashMapUsingChaining.hash_function(self, key)`
- **Time Complexity:** O(1) – computes absolute hash and modulo.
- **Space Complexity:** O(1) – no additional memory.
- **Math:** All operations are constant‑time.
- **Easy:** Converts any key into a bucket index using a simple formula.

---

### 9. `HashMapUsingChaining.rehash(self)`
- **Time Complexity:** O(N) where `N` = number of elements in the hash map.  
  The function traverses all existing entries and re‑inserts them into the new, larger array.  
  Because `rehash` is only called when load factor exceeds 0.75, the total cost over many inserts is amortized O(1) per insert.
- **Space Complexity:** O(new_capacity) – new bucket array of double size.  
  Additionally, the old buckets remain until garbage‑collected, so peak space is about O(2 * new_capacity).
- **Math:** For each of the `N` elements, we call `insert`, which itself does O(1) work (average). The loop over old buckets visits each chain fully.
- **Easy:** When the hash map gets too full, we double the number of slots and move every item to its new home.

---

### 10. `HashMapUsingChaining.insert(self, key, value)`
- **Time Complexity:**  
  - **Average case:** O(1) – thanks to good hash function and load factor < 0.75.  
  - **Worst case:** O(N) – if all keys hash to the same bucket, the linked‑list search and possible rehash become linear.  
  - **Amortized:** O(1) because `rehash` only occurs occasionally and its O(N) cost is spread over many inserts.
- **Space Complexity:** O(1) amortized – the new node is allocated, and the bucket array may be doubled during rehash (which is a one‑time O(capacity) allocation).
- **Math:** Insert first searches the chain; with even distribution chain length ≈ N/capacity ≤ 0.75, so constant. If load factor > 0.75, `rehash` is called, which is O(N). Over many inserts, the total cost is linear in N, giving amortized constant.
- **Easy:** Find the right bucket, check if key exists (update) or add new node. If the map gets too crowded, we expand it.

---

### 11. `HashMapUsingChaining.get(self, key)`
- **Time Complexity:** Average O(1), worst‑case O(N) – similar to `search` in linked list.
- **Space Complexity:** O(1) – only local variables.
- **Math:** Hash to bucket, then traverse the chain until key found; chain length is expected constant.
- **Easy:** Go to the bucket and look for the key; usually found quickly.

---

### 12. `HashMapUsingChaining.remove(self, key)`
- **Time Complexity:** Average O(1), worst‑case O(N) – same reasoning as `get`.
- **Space Complexity:** O(1) – no extra storage.
- **Math:** Hash to bucket, then delete from chain; chain length expectation constant.
- **Easy:** Remove the key if it exists; mostly instant.

---

### 13. `HashMapUsingChaining.__len__(self)`
- **Time Complexity:** O(1) – returns stored `size` attribute.
- **Space Complexity:** O(1) – no computation.
- **Math:** Direct attribute access.
- **Easy:** Just tells you how many items are stored.

---

### 14. `HashMapUsingChaining.__str__(self)`
- **Time Complexity:** O(capacity + total_elements) – because it traverses every bucket and every node in all chains.
- **Space Complexity:** O(1) – only prints, no additional data structures.
- **Math:** Loops over all buckets and within each bucket walks the entire chain.
- **Easy:** Prints the whole map, so it must look at every item.

---

### 15. `HashMapUsingChaining.__setitem__(self, key, value)`
- **Time Complexity & Space:** Same as `insert` (O(1) amortized average).
- **Math:** Delegates to `insert`.
- **Easy:** Allows using square‑bracket syntax like `hash_map[key] = value`.

---

## Summary Table

| Function                     | Time (Average) | Time (Worst) | Space (Auxiliary) |
|------------------------------|----------------|--------------|-------------------|
| `LLNode.__init__`            | O(1)           | O(1)         | O(1)              |
| `LinkedList.add`             | O(1)           | O(1)         | O(1)              |
| `LinkedList.search`          | O(chain len)   | O(chain len) | O(1)              |
| `LinkedList.delete`          | O(chain len)   | O(chain len) | O(1)              |
| `LinkedList.traverse`        | O(chain len)   | O(chain len) | O(1)              |
| `HashMap.__init__`           | O(capacity)    | O(capacity)  | O(capacity)       |
| `__create_buckets`           | O(capacity)    | O(capacity)  | O(capacity)       |
| `hash_function`              | O(1)           | O(1)         | O(1)              |
| `rehash`                     | O(N)           | O(N)         | O(new_capacity)   |
| `insert`                     | O(1) amortized | O(N)         | O(1) amortized    |
| `get`                        | O(1)           | O(N)         | O(1)              |
| `remove`                     | O(1)           | O(N)         | O(1)              |
| `__len__`                    | O(1)           | O(1)         | O(1)              |
| `__str__`                    | O(N + capacity)| O(N + capacity)| O(1)            |
| `__setitem__`                | O(1) amortized | O(N)         | O(1) amortized    |

*N* = number of elements in the hash map.  
*capacity* = number of buckets.  
*chain len* = length of the linked list at a specific bucket.