## Comprehensive Guide to Threading, Multithreading, Multiprocessing, and the Factorial Example

This document covers all important concepts, terminology, and practical aspects of concurrency and parallelism in Python, using the provided factorial example as a central case. After reading this, you should have a deep understanding of how to choose between threads and processes, how to use them effectively, and why certain details like `sys.set_int_max_str_digits` are necessary.

---

## 1. Core Concepts and Terminology

### 1.1 Thread
A **thread** is the smallest unit of execution within a process. It has its own stack and registers but shares the code, data, and heap segments with other threads in the same process. Threads allow multiple execution flows to run concurrently within the same memory space.

### 1.2 Process
A **process** is an independent instance of a program in execution. It has its own memory space (code, data, heap, stack) and system resources. Processes are isolated from each other; one process cannot directly access another’s memory.

### 1.3 Multithreading
Multithreading refers to using multiple threads within a single process to achieve concurrency. In Python, due to the **Global Interpreter Lock (GIL)**, only one thread can execute Python bytecode at a time. However, threads can still be useful for I/O-bound tasks because the GIL is released during blocking I/O operations, allowing other threads to run.

### 1.4 Multiprocessing
Multiprocessing involves using multiple processes, each with its own Python interpreter and GIL. This allows **true parallelism** on multi-core CPUs for CPU-bound tasks. Processes communicate through inter-process communication (IPC) mechanisms like queues, pipes, or shared memory.

### 1.5 Global Interpreter Lock (GIL)
The GIL is a mutex that prevents multiple threads from executing Python bytecode simultaneously. It simplifies memory management in CPython but limits the parallelism of threads. I/O operations and some C extensions release the GIL, allowing concurrency. For CPU-bound tasks, multiprocessing is required to bypass the GIL.

### 1.6 Concurrency vs. Parallelism
- **Concurrency**: Multiple tasks making progress at the same time, but not necessarily simultaneously. On a single core, tasks interleave (context switching).
- **Parallelism**: Multiple tasks executing at the exact same moment, requiring multiple CPU cores.

### 1.7 Context Switching
The process of saving the state of a currently executing thread/process and loading the state of another. This overhead can impact performance.

### 1.8 Thread/Process Pool
A pool is a collection of pre-initialized workers (threads or processes) that can be reused to execute tasks. Pools reduce the overhead of creating and destroying workers for each task.

### 1.9 Synchronization Primitives
- **Lock (Mutex)**: Ensures only one thread/process can access a shared resource at a time.
- **RLock (Reentrant Lock)**: Allows the same thread to acquire the lock multiple times.
- **Semaphore**: Controls access to a limited number of resources.
- **Event**: Allows threads to wait for a signal.
- **Condition**: Enables waiting for a condition to be true.
- **Barrier**: Synchronizes a fixed number of threads/processes at a point.

### 1.10 Race Condition
A situation where multiple threads/processes access shared data concurrently, and the final outcome depends on the non-deterministic order of execution.

### 1.11 Deadlock
A state where two or more threads/processes are blocked forever, each waiting for a resource held by another.

### 1.12 Daemon Thread
A thread that runs in the background and is automatically terminated when the main program exits. Daemon threads are used for tasks that should not block program shutdown.

### 1.13 Process Control Block (PCB)
A data structure maintained by the OS that holds information about a process (e.g., process ID, state, program counter, memory allocation).

### 1.14 Pickling
The process of serializing Python objects into a byte stream to transfer them between processes. In multiprocessing, arguments and return values are pickled.

---

## 2. The Factorial Multiprocessing Example – Detailed Walkthrough

```python
import multiprocessing
import math
import sys
import time

# Increase the maximum number of digits for integer conversion
sys.set_int_max_str_digits(100000)

def compute_factorial(number):
    print(f"Computing factorial of {number}")
    result = math.factorial(number)
    print(f"Factorial of {number} is {result}")
    return result

if __name__ == "__main__":
    numbers = [5000, 6000, 700, 8000]

    start_time = time.time()

    # Create a pool of worker processes
    with multiprocessing.Pool() as pool:
        results = pool.map(compute_factorial, numbers)

    end_time = time.time()

    print(f"Results: {results}")
    print(f"Time taken: {end_time - start_time} seconds")
```

### 2.1 Why `sys.set_int_max_str_digits(100000)`?
- `math.factorial` returns a large integer. When printing that integer (e.g., in the `print(f"Factorial of {number} is {result}")`), Python must convert the integer to a string to display it.
- By default, Python has a limit on the number of digits for string conversion to prevent excessive memory usage (e.g., converting a 100,000-digit number could be huge). The limit is controlled by `sys.get_int_max_str_digits()`.
- `sys.set_int_max_str_digits(100000)` raises the limit to allow conversion of integers with up to 100,000 digits. Without this, converting a factorial of 5000 (which has ~16,000 digits) would raise a `ValueError`.
- **When to use**: Whenever you need to display or convert extremely large integers to strings (e.g., printing huge factorials, working with cryptographic keys, large combinatorial results).

### 2.2 The Main Algorithm
The program computes factorials for a list of numbers using multiple processes. Each factorial is a CPU‑intensive operation. By distributing the work across processes, the system can use multiple CPU cores simultaneously, reducing total execution time.

- **`multiprocessing.Pool()`**: Creates a pool of worker processes. By default, the number of workers equals `os.cpu_count()`. The pool manages the processes for you.
- **`pool.map(function, iterable)`**: Similar to the built-in `map`, but it distributes the items in the iterable across the worker processes. The function is called once for each item, and the results are returned in order.
- **`with` block**: Ensures the pool is properly closed and workers are terminated after tasks are completed.

### 2.3 How It Works (Step-by-Step)
1. The main process creates a pool of worker processes (e.g., 4 workers if the CPU has 4 cores).
2. It splits the `numbers` list into chunks and assigns each chunk to a worker.
3. Each worker process:
   - Computes `math.factorial(number)` for its assigned numbers.
   - Prints a message (interleaved because processes write to the console independently).
   - Returns the result.
4. The main process collects all results in order and stores them in `results`.
5. After all tasks finish, the pool is shut down.

### 2.4 Performance
- For CPU‑bound tasks, the speedup is roughly proportional to the number of cores (though overhead from process creation and communication exists).
- In this example, computing factorials of 5000, 6000, 700, and 8000 would take a few seconds sequentially. With 4 cores, the time is reduced to about 1/4 of the sequential time (if the tasks are balanced).

### 2.5 Important Points
- The `if __name__ == "__main__":` guard is **mandatory** on Windows and recommended everywhere to prevent infinite process spawning.
- `math.factorial` is implemented in C and releases the GIL, so even if you used threads, they would run in parallel? Actually, `math.factorial` is CPU‑intensive, but in CPython, C functions that don't release the GIL will block other threads. However, many C functions do release the GIL. In any case, for CPU‑bound work, multiprocessing is the correct tool.

---

## 3. Real-World Use Cases for Multiprocessing

| Domain | Example Tasks |
|--------|---------------|
| **Data Science** | Parallel training of models, cross‑validation, hyperparameter tuning. |
| **Image/Video Processing** | Applying filters, transformations, object detection on multiple frames simultaneously. |
| **Scientific Computing** | Numerical simulations, Monte Carlo methods, large matrix operations. |
| **Web Crawling (CPU‑intensive parsing)** | After fetching pages, parsing with BeautifulSoup can be CPU‑heavy; distribute parsing across processes. |
| **Machine Learning** | Feature extraction, data preprocessing, ensemble model predictions. |

---

## 4. More Examples to Deepen Understanding

### 4.1 Parallel Prime Number Checking
```python
import math
import multiprocessing

def is_prime(n):
    if n < 2:
        return False
    for i in range(2, int(math.sqrt(n)) + 1):
        if n % i == 0:
            return False
    return True

if __name__ == "__main__":
    numbers = [112272535095293, 112582705942171, 115280095190773, 1099726899285419]
    with multiprocessing.Pool() as pool:
        results = pool.map(is_prime, numbers)
    print(results)
```

### 4.2 Image Processing with Pillow
```python
import multiprocessing
from PIL import Image, ImageFilter

def apply_blur(image_path):
    img = Image.open(image_path)
    blurred = img.filter(ImageFilter.BLUR)
    blurred.save(f"blurred_{image_path}")

if __name__ == "__main__":
    images = ["img1.jpg", "img2.jpg", "img3.jpg", "img4.jpg"]
    with multiprocessing.Pool() as pool:
        pool.map(apply_blur, images)
```

### 4.3 Using `starmap` for Multiple Arguments
```python
import multiprocessing

def power(base, exponent):
    return base ** exponent

if __name__ == "__main__":
    args = [(2, 10), (3, 5), (4, 3), (5, 2)]
    with multiprocessing.Pool() as pool:
        results = pool.starmap(power, args)
    print(results)  # [1024, 243, 64, 25]
```

### 4.4 Shared Memory with `Value` and `Array`
```python
import multiprocessing

def increment(counter, lock):
    for _ in range(1000):
        with lock:
            counter.value += 1

if __name__ == "__main__":
    counter = multiprocessing.Value('i', 0)
    lock = multiprocessing.Lock()
    processes = [multiprocessing.Process(target=increment, args=(counter, lock)) for _ in range(4)]
    for p in processes:
        p.start()
    for p in processes:
        p.join()
    print(counter.value)  # 4000
```

---

## 5. When to Use Multithreading vs. Multiprocessing

| Aspect | Multithreading | Multiprocessing |
|--------|----------------|-----------------|
| **Task Type** | I/O-bound (network, disk, user input) | CPU-bound (heavy computations) |
| **GIL Impact** | Limited by GIL; only one thread runs at a time | Bypassed; each process has its own GIL |
| **Memory** | Shared; easy to exchange data | Separate; requires IPC (pickling overhead) |
| **Overhead** | Low (threads are lightweight) | Higher (process creation, memory) |
| **Crash Isolation** | One thread can crash the whole process | One process crash does not affect others |
| **Best For** | Web scraping, file I/O, GUI responsiveness | Data processing, simulations, image manipulation |

---

## 6. Advanced Topics and Interview Questions

### 6.1 What is the difference between `Pool.map` and `Pool.apply_async`?
- `map` is blocking and returns results in input order.
- `apply_async` submits a single task and returns a `AsyncResult` object; it is non‑blocking. Use it when you need more control, such as callbacks or error handling.

### 6.2 How do you handle exceptions in multiprocessing?
Exceptions raised in child processes are propagated when calling `result()` on the `AsyncResult`. For `map`, exceptions are raised when iterating over results.

### 6.3 What is pickling and why is it important?
Pickling is the serialization of Python objects. When passing data between processes, objects are pickled and unpickled. Not all objects can be pickled (e.g., file handles, network connections). Use `multiprocessing.Manager` for complex shared state.

### 6.4 What is a `Manager` and when would you use it?
`multiprocessing.Manager` creates a server process that holds Python objects and allows other processes to manipulate them via proxies. It's slower than `Value`/`Array` but can handle arbitrary objects (lists, dicts, etc.).

### 6.5 How can you set the start method for multiprocessing?
On Unix, the default is `fork`. On Windows, it's `spawn`. You can set it explicitly:
```python
import multiprocessing
multiprocessing.set_start_method('spawn')  # or 'fork' or 'forkserver'
```

### 6.6 What is the difference between `fork` and `spawn`?
- **fork**: Child process inherits the parent’s memory state. It's fast but can cause issues with threading and certain libraries.
- **spawn**: Starts a fresh interpreter and imports the main module. It's safer but slower.

---

## 7. Conclusion

This guide has covered the essential concepts, practical implementations, and real-world applications of multithreading and multiprocessing in Python. The factorial example demonstrates how to apply multiprocessing to CPU‑bound tasks, including the crucial detail of increasing the integer string conversion limit for large numbers.

By understanding the trade-offs between threads and processes, you can design efficient concurrent programs that leverage the strengths of each model. Remember to always use the `if __name__ == "__main__":` guard, choose the right synchronization primitives, and be mindful of the GIL when using threads.

For further exploration, consider studying the `concurrent.futures` module (a higher‑level abstraction), asyncio for I/O‑bound concurrency, and the `multiprocessing` module’s advanced features like shared memory and managers.