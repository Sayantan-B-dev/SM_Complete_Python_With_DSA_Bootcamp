## The `concurrent.futures` Module – High-Level Asynchronous Execution

### 1. Why `concurrent.futures` Exists

Before Python 3.2, writing concurrent code required direct use of the `threading` and `multiprocessing` modules. While powerful, these low-level interfaces forced developers to manage:

- Thread/process creation and lifecycle manually.
- Result collection and error handling across multiple workers.
- Synchronization and coordination.

The `concurrent.futures` module was introduced to provide a **high-level interface** for asynchronously executing callable objects using threads or processes. It abstracts away the complexity of managing worker pools and collecting results, allowing developers to focus on the task logic.

**Key abstractions:**
- **Executor** – an object that manages a pool of workers (threads or processes) and accepts tasks for execution.
- **Future** – an object representing the eventual result of an asynchronous operation.

Two concrete executors are provided:
- `ThreadPoolExecutor` – uses a pool of threads (good for I/O‑bound tasks).
- `ProcessPoolExecutor` – uses a pool of processes (good for CPU‑bound tasks).

---

### 2. ThreadPoolExecutor

#### 2.1 What It Is and Why It Is Needed

`ThreadPoolExecutor` manages a fixed-size pool of worker threads. Tasks submitted to the executor are executed by idle threads, and results can be collected as they become available.

**Why use a thread pool?**
- Creating a thread for every short-lived task is expensive (overhead of thread creation and destruction).
- A pool reuses threads, amortizing the cost.
- Limits the number of concurrent threads, preventing resource exhaustion.

`ThreadPoolExecutor` is ideal for **I/O‑bound** tasks where the GIL is released during waiting, allowing concurrency.

#### 2.2 How It Works – A Step-by-Step Walkthrough

Consider the provided reference code:

```python
from concurrent.futures import ThreadPoolExecutor
import time

def print_number(number):
    time.sleep(1)
    return f"Number :{number}"

numbers = [1,2,3,4,5,6,7,8,9,0,1,2,3]

with ThreadPoolExecutor(max_workers=3) as executor:
    results = executor.map(print_number, numbers)

for result in results:
    print(result)
```

**Step‑by‑step explanation:**

1. **`with ThreadPoolExecutor(max_workers=3) as executor:`**  
   - Creates a `ThreadPoolExecutor` instance with 3 worker threads.
   - The `with` block ensures that when the block exits, `executor.shutdown(wait=True)` is called automatically, waiting for all submitted tasks to complete before cleaning up the threads.

2. **`results = executor.map(print_number, numbers)`**  
   - The `map` method is similar to the built‑in `map` but executes the function concurrently.
   - It submits `print_number(number)` for each element in `numbers` to the executor.
   - Tasks are queued; the 3 worker threads start processing tasks immediately, taking tasks from the queue as they finish.
   - The `map` method returns an **iterator** that yields results in the **order of the input iterable**, not in the order of completion.

3. **`for result in results:`**  
   - Iterating over the `results` iterator blocks until the result for the first input item is available, then yields it.
   - Because tasks run concurrently, the total time is roughly `(len(numbers) / max_workers) * (time per task)`, assuming each task takes the same amount of time.
   - Here each task sleeps 1 second. With 13 tasks and 3 workers, the total time ≈ ceil(13/3) ≈ 5 seconds (plus overhead), compared to 13 seconds if run sequentially.

**Key point:** The `map` method is convenient when the input is an iterable and output order must match input order. For cases where order does not matter, `submit` + `as_completed` can be used.

#### 2.3 Additional Features of ThreadPoolExecutor

##### 2.3.1 Submitting Individual Tasks with `submit`

`submit` schedules a single callable and returns a `Future` object.

```python
from concurrent.futures import ThreadPoolExecutor
import time

def task(name, delay):
    time.sleep(delay)
    return f"Task {name} finished after {delay}s"

with ThreadPoolExecutor(max_workers=3) as executor:
    # Submit multiple tasks, storing futures in a list
    futures = [executor.submit(task, i, 2) for i in range(5)]

    # Collect results as they complete
    for future in futures:
        print(future.result())  # Blocks until result available
```

##### 2.3.2 Processing Results as They Complete with `as_completed`

If results are needed as soon as any task finishes (not necessarily in order), use `as_completed`.

```python
from concurrent.futures import ThreadPoolExecutor, as_completed
import time
import random

def download(url):
    time.sleep(random.uniform(0.5, 2))
    return f"Downloaded {url}"

urls = ["page1.html", "page2.html", "page3.html", "page4.html"]

with ThreadPoolExecutor(max_workers=2) as executor:
    futures = [executor.submit(download, url) for url in urls]
    for future in as_completed(futures):
        print(future.result())  # Prints as soon as each download finishes
```

**Difference from `map`:** `as_completed` yields futures in the order of completion, not input order.

##### 2.3.3 Timeouts and Error Handling

Futures support `result(timeout)` to wait a limited time, and exceptions are propagated.

```python
from concurrent.futures import ThreadPoolExecutor, TimeoutError

def faulty_task():
    raise ValueError("Something went wrong")

with ThreadPoolExecutor(max_workers=1) as executor:
    future = executor.submit(faulty_task)
    try:
        result = future.result(timeout=5)
    except ValueError as e:
        print(f"Caught exception: {e}")
    except TimeoutError:
        print("Task timed out")
```

##### 2.3.4 Callbacks

A callback can be attached to a future to be executed when the task completes.

```python
def callback(future):
    print(f"Callback: result = {future.result()}")

with ThreadPoolExecutor(max_workers=2) as executor:
    future = executor.submit(lambda: "Hello from thread")
    future.add_done_callback(callback)
    # The callback will be called when the future completes
```

#### 2.4 Real-Life Example – Web Scraping (I/O-Bound)

```python
import requests
from concurrent.futures import ThreadPoolExecutor, as_completed

def fetch_url(url):
    """Fetch a URL and return its status and length."""
    try:
        response = requests.get(url, timeout=5)
        return url, response.status_code, len(response.content)
    except Exception as e:
        return url, str(e), 0

urls = [
    "https://www.python.org",
    "https://www.github.com",
    "https://www.stackoverflow.com",
    "https://www.wikipedia.org",
    "https://www.reddit.com"
]

with ThreadPoolExecutor(max_workers=5) as executor:
    futures = [executor.submit(fetch_url, url) for url in urls]

    for future in as_completed(futures):
        url, status, size = future.result()
        print(f"{url}: status={status}, size={size} bytes")
```

**Why threads?** Network I/O releases the GIL, so threads can run concurrently, significantly reducing total execution time compared to sequential requests.

---

### 3. ProcessPoolExecutor

#### 3.1 What It Is and Why It Is Needed

`ProcessPoolExecutor` manages a pool of worker **processes**. Each worker has its own Python interpreter and memory space, bypassing the GIL. This allows true parallelism for **CPU‑bound** tasks.

**Why a process pool?**
- Creating a process for every task is heavy; a pool reuses processes.
- The GIL prevents multiple threads from executing Python code in parallel; processes overcome this.
- For CPU‑intensive operations, using multiple processes can fully utilize multiple CPU cores.

#### 3.2 How It Works – Reference Code Walkthrough

```python
from concurrent.futures import ProcessPoolExecutor
import time

def square_number(number):
    time.sleep(2)
    return f"Square: {number * number}"

numbers = [1,2,3,4,5,6,7,8,9,11,2,3,12,14]

if __name__ == "__main__":
    with ProcessPoolExecutor(max_workers=3) as executor:
        results = executor.map(square_number, numbers)

    for result in results:
        print(result)
```

**Important:** The `if __name__ == "__main__":` guard is **essential** on Windows and recommended on all platforms. Without it, the code would attempt to spawn new processes recursively, leading to errors.

**Step‑by‑step:**

1. **`with ProcessPoolExecutor(max_workers=3) as executor:`**  
   - Creates a pool of 3 worker processes (each is a separate Python process).
   - The `with` block ensures proper cleanup (calls `shutdown()`).

2. **`results = executor.map(square_number, numbers)`**  
   - Submits `square_number` for each number in the list to the process pool.
   - The 3 workers process the tasks concurrently.
   - Because each task sleeps 2 seconds, total execution time ≈ ceil(14/3) * 2 ≈ 10 seconds (if tasks are evenly distributed), whereas sequential would take 28 seconds.

3. **`for result in results:`**  
   - Iterates over results in input order; blocking as needed.

#### 3.3 Differences from ThreadPoolExecutor

| Aspect | ThreadPoolExecutor | ProcessPoolExecutor |
|--------|-------------------|---------------------|
| **GIL** | Limited by GIL; only one thread executes Python at a time | Bypassed; each process has its own GIL |
| **Parallelism** | Concurrency (not parallelism for Python code) | True parallelism across CPU cores |
| **Memory** | Shared (threads within same process) | Isolated (each process has its own memory) |
| **Overhead** | Low (threads are lightweight) | Higher (process creation, IPC) |
| **Data Sharing** | Easy via shared variables (with locks) | Requires IPC (pickling) – data must be serializable |
| **Best for** | I/O‑bound tasks | CPU‑bound tasks |

#### 3.4 Using `submit` and `as_completed` with ProcessPoolExecutor

Similar to threads, but with the added requirement that all arguments and return values must be picklable.

```python
from concurrent.futures import ProcessPoolExecutor, as_completed
import math

def is_prime(n):
    """CPU‑intensive primality test."""
    if n < 2:
        return False
    for i in range(2, int(math.sqrt(n)) + 1):
        if n % i == 0:
            return False
    return True

numbers = [112272535095293, 112582705942171, 112272535095293, 115280095190773]

if __name__ == "__main__":
    with ProcessPoolExecutor(max_workers=4) as executor:
        futures = [executor.submit(is_prime, n) for n in numbers]
        for future in as_completed(futures):
            n = numbers[futures.index(future)]  # Not efficient; just for demo
            print(f"{n} is prime: {future.result()}")
```

#### 3.5 Real-Life Example – Image Processing (CPU-Bound)

```python
from PIL import Image
import numpy as np
from concurrent.futures import ProcessPoolExecutor
import os

def apply_filter(image_path):
    """Apply a computationally heavy filter (e.g., edge detection)."""
    img = Image.open(image_path).convert('L')  # grayscale
    arr = np.array(img)
    # Simulate a heavy convolution (simplified)
    # In practice, use a proper convolution kernel
    filtered = np.zeros_like(arr)
    for i in range(1, arr.shape[0]-1):
        for j in range(1, arr.shape[1]-1):
            filtered[i,j] = abs(arr[i+1,j] - arr[i-1,j]) + abs(arr[i,j+1] - arr[i,j-1])
    out_path = f"filtered_{os.path.basename(image_path)}"
    Image.fromarray(filtered).save(out_path)
    return out_path

image_files = ["img1.jpg", "img2.jpg", "img3.jpg", "img4.jpg"]

if __name__ == "__main__":
    with ProcessPoolExecutor(max_workers=os.cpu_count()) as executor:
        results = list(executor.map(apply_filter, image_files))
    print("Processed images:", results)
```

---

### 4. Choosing Between ThreadPoolExecutor and ProcessPoolExecutor

| Scenario | Recommended Executor |
|----------|---------------------|
| Network requests, file I/O, database queries | `ThreadPoolExecutor` |
| Heavy number crunching, image processing, simulations | `ProcessPoolExecutor` |
| Mixed I/O and CPU (with small CPU part) | `ThreadPoolExecutor` (often sufficient) |
| CPU‑intensive but requires large shared memory | `ProcessPoolExecutor` with shared memory via `Value`/`Array` or managers |
| Tasks are very short (microseconds) | Neither; overhead may dominate; consider sequential or specialized async |

**Rule of thumb:** If the GIL is a bottleneck, use processes. Otherwise, threads are lighter.

---

### 5. Advanced Tips

#### 5.1 Adjusting `max_workers`
- For `ThreadPoolExecutor`, a common default is `min(32, os.cpu_count() + 4)`.
- For `ProcessPoolExecutor`, a typical value is `os.cpu_count()`, but it can be increased if tasks are I/O‑heavy or if the machine has many cores.

#### 5.2 Handling Large Data with ProcessPoolExecutor
Because processes do not share memory, arguments and return values are pickled and transmitted. This can be expensive for large objects. Consider using shared memory (`multiprocessing.Array`, `multiprocessing.Value`) or reducing the amount of data passed.

#### 5.3 Cancelling Tasks
Futures can be cancelled only if they have not started executing. Use `future.cancel()`; it returns `True` if cancellation succeeded.

#### 5.4 Context Managers and Shutdown
Always use the `with` statement to ensure the executor shuts down cleanly. Alternatively, call `executor.shutdown(wait=True)` manually.

#### 5.5 Combining with `asyncio`
For modern asynchronous I/O, `asyncio` is often a better fit than threads. However, `concurrent.futures` can be integrated with `asyncio` via `loop.run_in_executor()` to run blocking code in a thread or process pool without blocking the event loop.

---

### 6. Summary of `concurrent.futures`

- **`concurrent.futures`** provides a high-level abstraction for asynchronous execution using pools of threads or processes.
- **`ThreadPoolExecutor`** – ideal for I/O‑bound tasks; lightweight; shares memory; affected by GIL.
- **`ProcessPoolExecutor`** – ideal for CPU‑bound tasks; true parallelism; isolated memory; requires picklable data.
- **Key methods:**
  - `map(func, iterable)` – submit tasks in batch, results in input order.
  - `submit(func, *args, **kwargs)` – submit a single task, returns `Future`.
  - `as_completed(futures)` – iterate over futures as they complete.
- **Always use `with` block** to manage executor lifecycle.
- For `ProcessPoolExecutor`, protect the main module with `if __name__ == "__main__":`.

---

### 7. Complete Reference Code with Explanations

**ThreadPoolExecutor example with comments:**

```python
from concurrent.futures import ThreadPoolExecutor
import time

def print_number(number):
    time.sleep(1)                # Simulate I/O wait (e.g., network request)
    return f"Number :{number}"

numbers = [1,2,3,4,5,6,7,8,9,0,1,2,3]

# Create a thread pool with 3 workers. The 'with' block ensures the pool is
# properly shut down after all tasks complete.
with ThreadPoolExecutor(max_workers=3) as executor:
    # map applies print_number to each number in the list concurrently.
    # It returns an iterator that yields results in the order of the input.
    results = executor.map(print_number, numbers)

# Iterate over the results. Because map yields results in order, the loop
# will block for each result until it becomes available.
for result in results:
    print(result)
```

**ProcessPoolExecutor example with comments:**

```python
from concurrent.futures import ProcessPoolExecutor
import time

def square_number(number):
    time.sleep(2)                # Simulate a CPU‑intensive task (e.g., heavy computation)
    return f"Square: {number * number}"

numbers = [1,2,3,4,5,6,7,8,9,11,2,3,12,14]

# The guard is necessary on Windows to avoid infinite process spawning.
if __name__ == "__main__":
    # Create a process pool with 3 workers. Each worker is a separate process.
    with ProcessPoolExecutor(max_workers=3) as executor:
        # map submits all tasks to the pool; they run concurrently.
        results = executor.map(square_number, numbers)

    # Iterate over results in input order. The main process blocks until the
    # result for the first number is ready, then prints it, etc.
    for result in results:
        print(result)
```

---

### 8. Conclusion

The `concurrent.futures` module offers a clean, intuitive way to execute tasks concurrently, whether using threads or processes. By abstracting away the low-level details of pool management and result collection, it allows developers to write parallel code that is both readable and maintainable. Choose `ThreadPoolExecutor` for I/O‑bound workloads and `ProcessPoolExecutor` for CPU‑bound workloads, and always remember to protect the main module when using processes.