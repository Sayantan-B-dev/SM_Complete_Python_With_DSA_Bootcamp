## Multiprocessing in Python – A Comprehensive Guide

### 1. Introduction to Multiprocessing

Multiprocessing is a parallel execution model where multiple independent processes are created, each with its own Python interpreter and memory space. Unlike threading, which runs within a single process, multiprocessing enables true parallelism by utilizing multiple CPU cores.

**When to Use Multiprocessing**

- **CPU-bound tasks**: Computations that require significant processor time (mathematical calculations, data processing, image manipulation, machine learning training). Multiprocessing bypasses the Global Interpreter Lock (GIL), allowing true parallel execution across cores.

- **Parallel execution**: When the goal is to reduce total execution time by distributing work across multiple CPU cores.

- **Isolation**: When tasks must be completely isolated from each other for stability or security reasons.

**When to Use Threading Instead**

- **I/O-bound tasks**: Operations that spend most time waiting for I/O (file, network, database). Threads are lightweight and share memory, making them more efficient for such workloads.

### 2. How Multiprocessing Works

When a process is created using the `multiprocessing` module:

1. A new Python interpreter is launched (fork on Unix-like systems, spawn on Windows).
2. The target function and its arguments are pickled and sent to the new process.
3. Each process gets its own **independent memory space** (code, data, heap, stack).
4. The operating system schedules these processes across available CPU cores.
5. No GIL contention – each process executes Python bytecode in its own interpreter.

**Memory Isolation Illustration**

```
┌─────────────────────┐      ┌─────────────────────┐
│   Process 1         │      │   Process 2         │
│  ┌───────────────┐  │      │  ┌───────────────┐  │
│  │   Code        │  │      │  │   Code        │  │
│  ├───────────────┤  │      │  ├───────────────┤  │
│  │   Data        │  │      │  │   Data        │  │
│  ├───────────────┤  │      │  ├───────────────┤  │
│  │   Heap        │  │      │  │   Heap        │  │
│  ├───────────────┤  │      │  ├───────────────┤  │
│  │   Stack       │  │      │  │   Stack       │  │
│  └───────────────┘  │      │  └───────────────┘  │
│                     │      │                     │
│  Python Interpreter │      │  Python Interpreter │
└─────────────────────┘      └─────────────────────┘
         │                             │
         └───────────┬─────────────────┘
                     │
              ┌──────▼──────┐
              │   CPU Cores │
              └─────────────┘
```

### 3. Basic Process Creation and Execution

The `multiprocessing.Process` class is the core building block.

```python
import multiprocessing
import time

def square_numbers():
    for i in range(5):
        time.sleep(1)
        print(f"Square: {i*i}")

def cube_numbers():
    for i in range(5):
        time.sleep(1.5)
        print(f"Cube: {i * i * i}")

if __name__ == "__main__":
    # Create process objects (they do not start immediately)
    p1 = multiprocessing.Process(target=square_numbers)
    p2 = multiprocessing.Process(target=cube_numbers)

    start = time.time()

    # Start the processes
    p1.start()
    p2.start()

    # Wait for both to complete
    p1.join()
    p2.join()

    elapsed = time.time() - start
    print(f"Total execution time: {elapsed:.2f} seconds")
```

**Output (approximate):**
```
Square: 0
Cube: 0
Square: 1
Square: 4
Cube: 1
Square: 9
Cube: 8
Square: 16
Cube: 27
Total execution time: 7.50 seconds
```

**Explanation:**
- The two processes run **in parallel** on separate CPU cores.
- Total time is roughly the maximum of the individual durations: square takes 5 seconds, cube takes 7.5 seconds → ~7.5 seconds.
- If run sequentially, total time would be 5 + 7.5 = 12.5 seconds.
- The `if __name__ == "__main__":` guard is **critical** on Windows to avoid infinite process creation.

### 4. Process Lifecycle and Methods

| Method | Description |
|--------|-------------|
| `Process(target=func, args=())` | Constructor. `target` is the callable, `args` is a tuple of arguments. |
| `start()` | Starts the process (non‑blocking). |
| `join([timeout])` | Blocks until the process terminates. |
| `is_alive()` | Returns `True` if the process is still running. |
| `terminate()` | Forcefully stops the process (use with caution). |
| `name` | A string identifier (can be set). |
| `daemon` | If `True`, the process exits when the main program ends. |

**Example with arguments and daemon processes:**

```python
import multiprocessing
import time

def worker(name, delay):
    print(f"Worker {name} started")
    time.sleep(delay)
    print(f"Worker {name} finished")

if __name__ == "__main__":
    # Regular process
    p = multiprocessing.Process(target=worker, args=("Regular", 2))
    
    # Daemon process (will be killed when main ends)
    d = multiprocessing.Process(target=worker, args=("Daemon", 5), daemon=True)
    
    p.start()
    d.start()
    
    p.join()          # Wait for regular process
    # Daemon process will be terminated when main exits (no join)
    print("Main exiting")
```

### 5. Differences Between Threads and Processes

| Feature | Thread | Process |
|---------|--------|---------|
| **Memory** | Shared (code, data, heap) | Separate (each has its own memory space) |
| **GIL** | Affected (only one thread runs Python at a time) | Bypassed (each process has its own GIL) |
| **Parallelism** | Limited to concurrency (not parallelism for Python code) | True parallelism across multiple cores |
| **Creation Overhead** | Low | Higher (new interpreter, memory allocation) |
| **Communication** | Shared variables (with locks) | IPC (Queue, Pipe, shared memory) |
| **Crash Isolation** | One thread crash can kill the entire process | A crash in one process does not affect others |
| **Best For** | I/O‑bound tasks | CPU‑bound tasks |

### 6. Inter-Process Communication (IPC)

Because processes have separate memory, explicit mechanisms are needed for data exchange.

#### 6.1 Queue

`multiprocessing.Queue` is a thread‑ and process‑safe FIFO queue, ideal for producer‑consumer patterns.

```python
import multiprocessing
import time
import random

def producer(queue, num_items):
    """Produces items and puts them into the queue"""
    for i in range(num_items):
        item = f"Item-{i}"
        time.sleep(random.uniform(0.2, 0.5))
        queue.put(item)
        print(f"Produced: {item}")
    queue.put(None)  # Sentinel to signal end

def consumer(queue, name):
    """Consumes items from the queue"""
    while True:
        item = queue.get()
        if item is None:
            break
        print(f"Consumer {name} processed: {item}")
        time.sleep(random.uniform(0.3, 0.7))

if __name__ == "__main__":
    q = multiprocessing.Queue()
    
    prod = multiprocessing.Process(target=producer, args=(q, 10))
    cons1 = multiprocessing.Process(target=consumer, args=(q, "A"))
    cons2 = multiprocessing.Process(target=consumer, args=(q, "B"))
    
    prod.start()
    cons1.start()
    cons2.start()
    
    prod.join()
    # After producer finishes, send None for each consumer
    q.put(None)
    q.put(None)
    cons1.join()
    cons2.join()
```

**Important:** `Queue` uses a `multiprocessing.Lock` internally and can handle arbitrary Python objects (via pickling). However, large objects can degrade performance.

#### 6.2 Pipe

`Pipe` returns a pair of connection objects representing the two ends of a pipe. It is simpler and faster than `Queue` for two‑way communication.

```python
import multiprocessing

def sender(conn):
    """Send data through the pipe"""
    messages = ["Hello", "from", "sender", "process"]
    for msg in messages:
        conn.send(msg)
    conn.close()

def receiver(conn):
    """Receive data from the pipe"""
    while True:
        try:
            msg = conn.recv()
        except EOFError:
            break
        print(f"Received: {msg}")

if __name__ == "__main__":
    parent_conn, child_conn = multiprocessing.Pipe()
    
    p_send = multiprocessing.Process(target=sender, args=(child_conn,))
    p_recv = multiprocessing.Process(target=receiver, args=(parent_conn,))
    
    p_send.start()
    p_recv.start()
    
    p_send.join()
    p_recv.join()
```

**Pipe Methods:**
- `send(obj)`: Sends an object (pickled).
- `recv()`: Receives an object (blocks until available).
- `close()`: Closes the connection.

Pipes are bidirectional, but typical usage designates one direction for each process.

#### 6.3 Shared Memory (Value, Array)

`multiprocessing.Value` and `multiprocessing.Array` allow storing shared data in memory accessible by multiple processes. They are faster than Queue/Pipe but require careful synchronization.

```python
import multiprocessing
import time

def increment(shared_counter, lock):
    for _ in range(1000):
        with lock:
            shared_counter.value += 1

if __name__ == "__main__":
    # Shared integer with initial value 0
    counter = multiprocessing.Value('i', 0)
    lock = multiprocessing.Lock()
    
    processes = []
    for _ in range(4):
        p = multiprocessing.Process(target=increment, args=(counter, lock))
        processes.append(p)
        p.start()
    
    for p in processes:
        p.join()
    
    print(f"Final counter value: {counter.value}")  # Should be 4000
```

**Type codes:**
- `'i'`: signed integer
- `'d'`: double float
- `'c'`: character
- etc. (similar to `array` module)

For arrays: `multiprocessing.Array('i', [0, 1, 2])` or `multiprocessing.Array('i', 10)` for length 10.

### 7. Synchronization Primitives

Multiprocessing provides synchronization tools similar to threading, but they work across processes.

| Primitive | Purpose |
|-----------|---------|
| `Lock` | Mutual exclusion |
| `RLock` | Reentrant lock |
| `Semaphore` | Limit concurrent access |
| `Event` | Simple signaling |
| `Condition` | Wait/notify with predicate |
| `Barrier` | Synchronize a fixed number of processes |

All these use inter‑process synchronization mechanisms (e.g., semaphores from the OS).

**Example with Event:**

```python
import multiprocessing
import time

def waiter(event):
    print("Waiting for event...")
    event.wait()
    print("Event received, proceeding")

def setter(event):
    time.sleep(2)
    print("Setting event")
    event.set()

if __name__ == "__main__":
    event = multiprocessing.Event()
    p1 = multiprocessing.Process(target=waiter, args=(event,))
    p2 = multiprocessing.Process(target=setter, args=(event,))
    
    p1.start()
    p2.start()
    p1.join()
    p2.join()
```

### 8. Process Pools

`multiprocessing.Pool` manages a pool of worker processes, distributing tasks and collecting results. It simplifies parallel execution of many independent tasks.

```python
import multiprocessing
import time
import math

def compute_factorial(n):
    """CPU‑intensive function"""
    result = math.factorial(n)
    time.sleep(0.1)  # Simulate additional work
    return result

if __name__ == "__main__":
    numbers = [100000, 100001, 100002, 100003, 100004, 100005]
    
    # Sequential execution
    start = time.time()
    seq_results = [compute_factorial(n) for n in numbers]
    seq_time = time.time() - start
    print(f"Sequential time: {seq_time:.2f}s")
    
    # Parallel execution using Pool
    start = time.time()
    with multiprocessing.Pool(processes=4) as pool:
        # map distributes and collects results
        par_results = pool.map(compute_factorial, numbers)
    par_time = time.time() - start
    print(f"Parallel time: {par_time:.2f}s")
    print(f"Speedup: {seq_time/par_time:.2f}x")
```

**Pool Methods:**

| Method | Description |
|--------|-------------|
| `map(func, iterable)` | Distributes iterable items, returns list of results in order |
| `map_async(func, iterable)` | Non‑blocking version of `map` |
| `apply(func, args)` | Executes a single function in a worker (blocking) |
| `apply_async(func, args)` | Non‑blocking version, returns `AsyncResult` |
| `starmap(func, iterable)` | Like `map` but unpacks tuples as arguments |
| `close()` | Prevents any more tasks from being submitted |
| `join()` | Wait for all worker processes to finish |

**Using `apply_async` with callbacks:**

```python
import multiprocessing
import time

def worker(num):
    time.sleep(1)
    return num * num

def callback(result):
    print(f"Callback received: {result}")

if __name__ == "__main__":
    with multiprocessing.Pool(processes=2) as pool:
        results = []
        for i in range(5):
            res = pool.apply_async(worker, args=(i,), callback=callback)
            results.append(res)
        
        # Wait for all tasks to complete
        for res in results:
            res.wait()
        
        # Collect all results
        final_results = [res.get() for res in results]
        print(f"All results: {final_results}")
```

### 9. Manager Objects for Shared State

Managers provide a way to share Python objects across processes (lists, dicts, namespaces) using a server process. They are slower than `Value`/`Array` but more flexible.

```python
import multiprocessing

def worker(shared_list, shared_dict, name):
    shared_list.append(name)
    shared_dict[name] = len(shared_list)

if __name__ == "__main__":
    with multiprocessing.Manager() as manager:
        shared_list = manager.list()
        shared_dict = manager.dict()
        
        processes = []
        for name in ["Alice", "Bob", "Charlie"]:
            p = multiprocessing.Process(target=worker, args=(shared_list, shared_dict, name))
            processes.append(p)
            p.start()
        
        for p in processes:
            p.join()
        
        print("Shared list:", list(shared_list))
        print("Shared dict:", dict(shared_dict))
```

**Manager Types:**
- `list()` – proxy to a shared list
- `dict()` – proxy to a shared dictionary
- `Namespace()` – object with shared attributes
- `Queue()` – a managed queue (rarely needed, as `multiprocessing.Queue` is already process‑safe)
- `Array()` – shared array
- `Value()` – shared value

### 10. Common Pitfalls and Best Practices

#### 10.1 Pickling Issues

Arguments passed to processes must be picklable. Avoid using objects with non‑picklable attributes (e.g., file handles, network connections). If a complex object is needed, consider using managers or `multiprocessing.Queue`.

#### 10.2 Fork vs Spawn on Different Platforms

- **Unix (Linux, macOS)**: Default start method is `fork`. The child inherits the parent’s memory state, making pickling unnecessary for some objects, but can cause issues with threading and certain libraries.
- **Windows**: Only `spawn` is available. The child process starts a fresh interpreter and imports the main module.

To set start method explicitly:

```python
import multiprocessing
multiprocessing.set_start_method('spawn')  # or 'fork' (Unix) or 'forkserver'
```

#### 10.3 Deadlocks with Locks and Queues

Improper use of locks can cause deadlocks, just as in threading. Similarly, when using `Queue.get()` without timeout and no items are produced, processes may block indefinitely.

**Mitigation:** Use timeouts and proper shutdown sentinels.

#### 10.4 Resource Leaks

Always ensure processes are joined or terminated properly. Using `with` context managers (like `Pool`) helps automatically clean up.

#### 10.5 Performance Overhead

Creating processes is expensive. For many small tasks, use a `Pool` with a limited number of workers to amortize the overhead.

#### 10.6 Using `multiprocessing` Inside Jupyter Notebooks

Jupyter often has issues with `multiprocessing` due to the way it manages processes. Workarounds:
- Use `multiprocessing.get_context('spawn')` to explicitly set start method.
- Or use `concurrent.futures.ProcessPoolExecutor` (often more compatible).

### 11. Process vs Thread vs AsyncIO Comparison

| Aspect | Multiprocessing | Threading | AsyncIO |
|--------|----------------|-----------|---------|
| **Parallelism** | True parallelism (cores) | Concurrency (GIL limited) | Single‑threaded concurrency |
| **Best for** | CPU‑bound tasks | I/O‑bound tasks | Network I/O, high‑concurrency I/O |
| **Overhead** | High (memory, startup) | Low | Very low |
| **Communication** | IPC (Queue, Pipe) | Shared memory + locks | No need (single thread) |
| **Ease of use** | Moderate | Moderate | Requires async/await mindset |

### 12. Complete Example: Parallel Data Processing

```python
import multiprocessing
import time
import random

def process_chunk(data_chunk, result_queue):
    """Simulate CPU‑intensive work on a chunk of data"""
    processed = []
    for value in data_chunk:
        # Heavy computation: sum of squares
        result = sum(i*i for i in range(10000)) + value
        processed.append(result)
    result_queue.put(processed)

def split_data(data, num_chunks):
    """Split data into approximately equal chunks"""
    chunk_size = len(data) // num_chunks
    chunks = []
    for i in range(num_chunks):
        start = i * chunk_size
        end = start + chunk_size if i < num_chunks - 1 else len(data)
        chunks.append(data[start:end])
    return chunks

if __name__ == "__main__":
    # Generate large dataset
    data = list(range(1, 1001))
    
    # Sequential processing
    start_seq = time.time()
    seq_results = []
    for val in data:
        res = sum(i*i for i in range(10000)) + val
        seq_results.append(res)
    seq_time = time.time() - start_seq
    print(f"Sequential time: {seq_time:.2f}s")
    
    # Parallel processing
    num_workers = multiprocessing.cpu_count()
    chunks = split_data(data, num_workers)
    
    result_queue = multiprocessing.Queue()
    processes = []
    
    start_par = time.time()
    for chunk in chunks:
        p = multiprocessing.Process(target=process_chunk, args=(chunk, result_queue))
        processes.append(p)
        p.start()
    
    # Collect results
    results = []
    for _ in range(num_workers):
        results.extend(result_queue.get())
    
    for p in processes:
        p.join()
    par_time = time.time() - start_par
    
    print(f"Parallel time: {par_time:.2f}s")
    print(f"Speedup: {seq_time/par_time:.2f}x")
    print(f"Results match: {results == seq_results}")
```

### 13. Summary

| Concept | Key Takeaway |
|---------|--------------|
| **Purpose** | True parallelism for CPU‑bound tasks; bypasses GIL |
| **Memory** | Each process has isolated memory → no race conditions by default, but communication requires IPC |
| **IPC** | Queue, Pipe, Value/Array, Manager |
| **Synchronization** | Locks, Events, Conditions, etc. (similar to threading) |
| **Pools** | Efficient for many independent tasks |
| **Platform differences** | Start methods: fork (Unix), spawn (Windows) |
| **Overhead** | Higher than threads; use for long‑running CPU tasks |
| **Safety** | Crash isolation; one process failure doesn't affect others |

Multiprocessing in Python is a powerful tool for leveraging multi‑core architectures. When used correctly, it can dramatically speed up CPU‑intensive computations while maintaining code clarity. For I/O‑bound workloads, threading or asyncio remain more appropriate due to lower overhead.