# Programs, Processes, and Threads – A Study Reference

## 1. Program

**Definition**  
A **program** is a passive entity stored on a non‑volatile medium (e.g., a disk). It consists of a sequence of instructions designed to perform a specific task and remains inert until loaded and executed by an operating system.

**Example**  
- The executable file `chrome.exe` (Google Chrome) is a program. When launched, the operating system creates a process from it.

**Characteristics**  
- Contains machine‑readable instructions and possibly static data (e.g., initialised global variables).  
- Has no execution state, no allocated memory, and no assigned system resources while on disk.

---

## 2. Process

### 2.1 Definition

A **process** is an active instance of a program in execution. It includes the program code, current activity (program counter, registers), and all resources needed for execution. The operating system represents a process with a **Process Control Block (PCB)** containing metadata such as process ID, state, register values, and memory mapping information.

### 2.2 Resources of a Process (Process Image)

A process is divided into several memory segments. The typical layout in virtual memory is:

```
   High addresses
   ┌─────────────────┐
   │     Stack       │  ← function call frames, local variables, return addresses
   │  (grows down)   │
   ├─────────────────┤
   │                 │
   │     Heap        │  ← dynamically allocated memory (malloc, new)
   │  (grows up)     │
   ├─────────────────┤
   │                 │
   │   Data Segment  │  ← global and static variables (initialised & uninitialised)
   ├─────────────────┤
   │   Code Segment  │  ← executable instructions (read‑only, .text)
   └─────────────────┘
   Low addresses
```

- **Code Segment (Text Segment)**  
  Contains the machine instructions of the program. It is typically **read‑only** to prevent accidental corruption. Multiple processes executing the same program may share this segment in memory.

- **Data Segment**  
  Stores **global** and **static** variables.  
  - `.data` – initialised variables.  
  - `.bss` – uninitialised variables (zero‑initialised at load time).

- **Heap**  
  Used for **dynamic memory allocation**.  
  - Grows upward (towards higher addresses) as memory is requested.  
  - Management is the responsibility of the programmer or a garbage collector; memory is not automatically freed when a function exits.

- **Stack**  
  Manages **function call frames** (activation records). Each frame contains local variables, return address, saved registers, and parameters.  
  - Grows downward (towards lower addresses).  
  - When a function returns, its frame is popped, automatically reclaiming the memory.

- **Registers**  
  CPU registers hold the execution context:  
  - **Program Counter (PC)** – address of the next instruction.  
  - **Stack Pointer (SP)** – current top of the stack.  
  - **General‑purpose registers** – temporary data.

### 2.3 Isolation and Memory Protection

Each process operates in its own **virtual address space**, enforced by the operating system through the **Memory Management Unit (MMU)**.  
- Processes are isolated: one process cannot directly read or modify another process’s memory.  
- A crash in one process does not affect other processes.

### 2.4 Context Switching Overhead

Switching the CPU from one process to another (context switch) involves:  
- Saving the current process’s state (registers, PC, memory maps) into its PCB.  
- Loading the saved state of the next process.  
- Flushing and reloading the Translation Lookaside Buffer (TLB) and caches.

This overhead is particularly noticeable when processes block on I/O operations. Frequent context switches increase execution time and reduce overall throughput.

**Example**  
- **Microsoft Excel** – each open spreadsheet window may run as a separate process (depending on application design), ensuring that a failure in one instance does not corrupt another.

---

## 3. Thread

### 3.1 Definition

A **thread** is the smallest unit of execution within a process. Often called a “lightweight process,” a thread consists of a stack and a set of registers, while sharing the code, data, and heap segments with other threads in the same process.

### 3.2 Single‑Threaded Process

A process that contains exactly one thread. The memory layout is identical to the process image shown earlier, with a single stack belonging to that thread.

**Example**  
- **MS Paint** – older versions were single‑threaded. A lengthy operation (e.g., applying a filter) would freeze the interface because the only thread handled both the UI and the computation.

### 3.3 Multi‑Threaded Process

A process that contains two or more threads. All threads share the same code, data, and heap, but each has its own stack and register context.

**Memory Layout of a Multi‑Threaded Process**:

```
   High addresses
   ┌─────────────────┐
   │ Thread 3 Stack  │
   ├─────────────────┤
   │ Thread 2 Stack  │
   ├─────────────────┤
   │ Thread 1 Stack  │  ← each thread has its own stack
   ├─────────────────┤
   │      Heap       │  ← shared by all threads
   ├─────────────────┤
   │   Data Segment  │  ← shared (global variables)
   ├─────────────────┤
   │   Code Segment  │  ← shared (instructions)
   └─────────────────┘
   Low addresses
```

- **Stack per thread** – allows independent execution of functions and handling of local variables.  
- **Registers per thread** – each thread has its own program counter and general‑purpose registers; switching between threads within the same process is fast because the memory mapping remains unchanged (no TLB flush required).

### 3.4 Advantages of Threads

1. **Resource Sharing** – threads automatically share memory, simplifying communication compared to inter‑process communication (IPC).  
2. **Economy** – creating a thread is cheaper than creating a process; thread context switches are faster than process context switches.  
3. **Responsiveness** – in interactive applications, one thread can handle user input while another performs background tasks, preventing interface freezes.  
4. **Parallelism** – on multi‑core systems, threads can execute concurrently, improving throughput.

### 3.5 Synchronisation Requirements

Because threads share global data and the heap, concurrent access must be coordinated. Mechanisms such as **mutexes**, **semaphores**, and **condition variables** are used to prevent race conditions and data corruption.

---

## 4. Comparison: Process vs. Thread

| Feature                | Process                                          | Thread                                          |
|------------------------|--------------------------------------------------|-------------------------------------------------|
| **Definition**         | An instance of a program in execution            | A unit of execution within a process            |
| **Memory**             | Separate code, data, heap, stack                 | Shares code, data, heap; has its own stack      |
| **Creation/Deletion**  | Heavy (requires OS resources)                    | Light (only stack and registers)                |
| **Context Switch**     | High overhead (TLB flush, memory map change)     | Low overhead (within same process)              |
| **Isolation**          | Fully isolated (a crash does not affect others)  | Shared – a bug can crash the entire process     |
| **Communication**      | IPC (pipes, sockets, shared memory) – complex    | Direct via shared memory – simpler              |
| **Example**            | A spreadsheet application in its own process     | A background calculation thread in the same application |

---

## 5. Putting the Concepts Together

- A **program** is a static recipe stored on disk.  
- A **process** is an active execution environment with its own isolated memory.  
- **Threads** are the independent execution streams inside a process, sharing most resources but each maintaining its own execution context.

When **Google Chrome** is launched, the operating system creates a process. Inside that process, multiple threads are typically created: one for the user interface, one for networking, one for rendering, etc. All threads share the same memory, but modern browsers often use multiple processes (one per tab) to improve stability and security – an example of trading thread economy for stronger isolation.

