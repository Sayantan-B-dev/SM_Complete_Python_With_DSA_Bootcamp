## Introduction to Queue Data Structure

A queue is a linear data structure that follows the **First-In, First-Out (FIFO)** principle. Elements are added at one end (the **rear**) and removed from the other end (the **front**). This is analogous to a line of people waiting for service.

### Core Operations
- **Enqueue:** Add an element to the rear of the queue.
- **Dequeue:** Remove and return the element from the front.
- **Front / Peek:** View the front element without removing it.
- **isEmpty:** Check if the queue has no elements.
- **Size:** Return the number of elements in the queue.

All these operations can be implemented in **O(1)** time using a circular array or a linked list.

---

## FIFO Principle

**First-In, First-Out** means that the element that has been in the queue the longest is the first one to be removed. This creates a natural ordering based on arrival time.  
Examples:  
- People lining up: the first person in line is the first to be served.  
- Documents sent to a printer: the first job submitted prints first.

---

## Real-Life Examples of Queue

1. **Ticket Counter Line** – Customers stand in line; the first customer gets the ticket first.  
2. **Drive-Thru** – Cars line up; the first car orders and receives food first.  
3. **Call Center** – Callers are placed on hold in the order they called; an agent serves the longest-waiting caller.  
4. **Grocery Checkout** – Shoppers queue at a cash register; the first in line checks out first.  
5. **Buses at a Stop** – Passengers board in the order they arrived.

---

## Industry Examples of Queue

### Job Scheduling
- **Operating Systems:** Process scheduling often uses queues. For example, in round-robin scheduling, ready processes are kept in a circular queue and executed in time slices.  
- **Print Spooler:** Print jobs are queued and sent to the printer one by one in the order received.

### Network Traffic
- **Router Queues:** Routers use buffers (queues) to store packets when outgoing links are busy. Packets are forwarded in FIFO order (or with priorities in advanced queuing disciplines like Weighted Fair Queuing).  
- **Traffic Shaping:** Queues help manage bandwidth and reduce packet loss during congestion.

### Messaging Services
- **Message Brokers** (e.g., RabbitMQ, Apache Kafka, AWS SQS): These systems implement distributed queues to decouple producers (senders) and consumers (receivers). Messages are stored temporarily and delivered to consumers in FIFO order (or with other semantics). This enables asynchronous communication, load leveling, and fault tolerance.  
- **Task Queues** (e.g., Celery, Redis queues): Used in web applications to offload long-running tasks (e.g., sending emails, image processing) to background workers.

### Other Industry Uses
- **Web Servers:** Incoming HTTP requests are queued when the server is at capacity, ensuring they are processed fairly.  
- **Breadth-First Search (BFS):** In graph algorithms, BFS uses a queue to explore nodes level by level.  
- **Hardware Buffers:** Keyboard input, audio streams, and I/O requests often use circular queues to handle data flow between devices and processes.