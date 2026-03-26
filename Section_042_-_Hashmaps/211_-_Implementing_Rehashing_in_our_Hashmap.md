```py

from LinkedListForChaining import LLNode,LinkedList

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

## **Rehashing in HashMap (Chaining) – Comprehensive Explanation**

### **Why Rehash?**
A hash table using separate chaining stores key–value pairs in buckets (linked lists). The **load factor** is defined as:

\[
\text{load factor} = \frac{\text{number of entries }(size)}{\text{number of buckets }(capacity)}
\]

If the load factor becomes too high, collisions increase → average chain length grows → operations (search, insert, delete) degrade from **O(1)** to **O(n)** in the worst case.  
To maintain **constant‑time average performance**, we enforce a maximum load factor (here **0.75**). When it exceeds this threshold, we **rehash**:  
- Double the number of buckets (capacity)  
- Re‑insert **every** existing entry into the new, larger bucket array  

Rehashing **redistributes** entries so that the new load factor becomes roughly half of the old one, and the expected chain length returns to a small constant.

---

### **Load Factor & Threshold**
- **`size`** – total number of key–value pairs currently stored.
- **`capacity`** – number of buckets in the hash table.
- **Threshold = 0.75** – chosen empirically to balance memory usage and performance.
- After every insert, we compute:
  \[
  \text{load\_factor} = \frac{\text{size}}{\text{capacity}}
  \]
  If `load_factor > 0.75`, we call `rehash()` immediately.

---

### **How `rehash()` Works – Step‑by‑Step**

1. **Save the old buckets**  
   ```python
   old_buckets = self.buckets
   ```
   This keeps a reference to the current bucket array (list of linked lists).  
   *We will need to traverse all its entries later.*

2. **Double the capacity**  
   ```python
   self.capacity = self.capacity * 2
   ```
   New capacity is twice the old one.

3. **Create new empty buckets**  
   ```python
   self.buckets = self.__create_buckets(self.capacity)
   ```
   This builds a new list of `self.capacity` empty linked lists.  
   *The old bucket array is still accessible via `old_buckets`.*

4. **Reset the size**  
   ```python
   self.size = 0
   ```
   We will re‑insert all existing entries, and each insertion will increment `size` again.  
   Starting from zero ensures the final count is correct.

5. **Re‑insert every entry from the old buckets**  
   ```python
   for eachLinkedListHead in old_buckets:
       current = eachLinkedListHead.head
       while current is not None:
           self.insert(current.key, current.value)
           current = current.next
   ```
   - For each bucket in the old array, we walk its linked list node by node.
   - For each node, we call `self.insert(key, value)`.  
     - `insert` uses the **new** capacity to compute the bucket index via `hash_function`.
     - Because the capacity has changed, the key will likely land in a different bucket.
     - `insert` also checks for duplicates (if the same key already exists in the new table – it won’t, because we are starting fresh), so it simply adds the node.
   - After re‑inserting all nodes, `self.size` will equal the original number of entries.

6. **Old buckets are discarded**  
   After the loop, `old_buckets` goes out of scope and is eligible for garbage collection. The hash table now operates with the larger bucket array.

---

### **How `insert()` Triggers Rehashing**

The `insert` method (or `__setitem__`) does:

1. Compute bucket index using current capacity.
2. Search that bucket’s linked list for the key:
   - If found → update the value (no size change).
   - If not found → add a new node at the **head** of the list and increment `size`.
3. Compute load factor after insertion:
   ```python
   load_factor = self.size / self.capacity
   ```
4. If `load_factor > 0.75` → call `self.rehash()`.

**Important:** The new entry that caused the load factor to exceed the threshold is **already** present in the hash table when rehash is called. The rehash then re‑inserts it (along with all other entries) into the new, larger bucket array. This is correct because we saved the entire old bucket array (which includes the new entry) before modifying anything.

---

### **Why This Keeps Operations Constant‑Time (Amortized Analysis)**

- **Average case:** With a good hash function and load factor ≤ 0.75, the expected length of any linked list is **O(1)**.  
  - `insert`, `get`, `remove` each hash to a bucket and then scan a constant‑sized chain.  
- **Worst case (without rehashing):** If we never rehashed, the chains could grow linearly with the number of entries.  
- **Rehashing cost:** Each rehash costs **O(n)** (traverse all existing entries and re‑insert them).  
  - But a rehash happens only after at least **n/2** insertions (because load factor goes from 0.5 to 0.75, doubling capacity when size reaches 75% of capacity).  
  - The cost of rehashing can be “spread” over the insertions that made it necessary → **amortized O(1)** per insertion.  
  - In other words, although a single insert may occasionally trigger an O(n) operation, the average over many inserts remains constant.

---

### **Key Points to Remember**

- **Changing capacity changes bucket indices** – so we cannot simply keep the old linked lists; we must redistribute every entry.  
- **The hash function** uses `abs(hash(key)) % self.capacity`. When `capacity` changes, the modulo result changes.  
- **Rehashing preserves the invariant** `size <= 0.75 * capacity` (just after rehash, `size` is roughly `0.375 * new_capacity`).  
- **The code resets `size = 0`** before re‑inserting, then `insert` increments it correctly for every existing entry.  
- **After rehash, the new buckets are empty**; the old buckets are completely abandoned.  
- **The order of elements in the linked lists may change** because new nodes are always added at the head of the bucket’s list, but order is irrelevant for correctness.

---

### **Visual Example**

Suppose initial `capacity = 4`, and we have inserted 3 entries.  
- Load factor = 3/4 = 0.75 → not yet rehashed.

Now we insert a 4th entry.  
- Load factor becomes 4/4 = 1.0 → exceeds 0.75 → rehash triggered.

**Steps:**
1. Save old buckets (4 buckets with possibly non‑empty chains).
2. Set `capacity = 8`, create 8 empty linked lists.
3. Reset `size = 0`.
4. For each node in the old buckets, re‑insert it using the new capacity:  
   - Each key’s hash is taken modulo 8 → likely a different bucket index.
5. After all 4 entries are re‑inserted, `size = 4`.  
   New load factor = 4/8 = 0.5 (well below 0.75).

Future inserts will stay below threshold until we have 7 entries (7/8 ≈ 0.875), which will trigger another rehash.

---

### **Potential Edge Cases**

- **Duplicate keys:** If a key is already present, `insert` simply updates the value and does **not** increase `size`. The load factor may drop, so rehash might not be triggered even if many updates occur.
- **Rehashing after many updates:** Rehash only cares about the number of entries, not the number of operations. Updates that don’t add new entries never cause rehash.
- **Large hash tables:** The rehash loop is O(n), which can be expensive for millions of entries, but it’s unavoidable to keep the performance guarantee.

---

This design is the classic separate‑chaining hash map with dynamic resizing. It ensures that the average case complexity remains **O(1)** for all dictionary operations, while the worst‑case is bounded by the number of elements but amortized constant over a sequence of inserts.