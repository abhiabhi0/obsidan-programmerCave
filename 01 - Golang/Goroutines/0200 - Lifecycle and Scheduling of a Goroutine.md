#### 1. Key Components in Go Scheduler
- **G (Goroutine):** Lightweight concurrent threads of execution.
- **M (Machine):** Represents OS threads.
- **P (Processor):** Logical processors that manage goroutines and enable parallelism.

#### 2. Go Scheduler:
- **M:N Scheduler:**
    - M goroutines are mapped to N OS threads.
    - Executes at most `GOMAXPROCS` threads simultaneously.
- **Global Queue and Local Run Queues:**
    - **Global Queue:** Holds goroutines waiting for execution (checked occasionally).
    - **Local Run Queues:** Each P maintains its own queue for runnable goroutines.
- **Work Stealing:** When a P finishes its queue, it randomly steals goroutines from another P's queue.

---

#### 3. Lifecycle of a Goroutine

1. **Creation:** Added to the Local Run Queue of a P.
2. **Execution:** Executed by an M assigned to a P.
3. **Blocking:** Temporarily relinquishes control (e.g., waiting on I/O or synchronization).
4. **Unblocking:** Added back to a Local Run Queue after resolving the blocking operation.
5. **Termination:** Completes execution and frees up resources.

---

#### 4. Scheduling Algorithm

1. Look for a runnable goroutine:
    - **Global Queue:** Checked 1/61 of the time for fairness.
    - **Local Queue:** Primary source for scheduling.
    - **Work Stealing:** If local queue is empty, attempts to steal tasks from another P.
2. Execute the runnable goroutine until it blocks.
3. Handle idle and spinning threads to balance resource use:
    - **Spinning Threads:** Keep threads active to minimize preemption and latency.
    - Only up to `GOMAXPROCS` threads can spin at a time.

---

#### 5. Work Sharing vs. Work Stealing

- **Work Sharing:** Proactively distributes new threads to underutilized processors.
- **Work Stealing:** Underutilized processors actively fetch work from others (used in Go since version 1.1).

---

#### 6. Optimizations in the Scheduler

- **Minimized Handoff:** Reduces latency and overhead by avoiding frequent thread handoff.
- **Spinning Threads:** Ensures no runnable goroutine is left idle without significant CPU overhead.

---

### **Lifecycle and Scheduler Diagram**

```plaintext
Goroutine Lifecycle:
+----------------+          +-----------------+         +----------------+
|   Created      |  ---->  |   Runnable      | ---->   |   Executed      |
+----------------+          +-----------------+         +----------------+
                                 ^   |                         |
                                 |   v                         v
                          +--------------+          +----------------+
                          |   Blocked    | <----->  |   Unblocked    |
                          +--------------+          +----------------+
                                  |
                                  v
                          +----------------+
                          |   Terminated   |
                          +----------------+

Scheduler Workflow:
1. Check Local Run Queue --> Global Queue (1/61 time) --> Steal from others --> Poll Network
2. Execute Goroutine
3. Handle Block/Unblock

Processor P Management:
+-----------+       +-----------+       +-----------+
|  P1: Q1   | <---> |  P2: Q2   | <---> |  P3: Q3   |
+-----------+       +-----------+       +-----------+
```

[[0300 - Goroutines vs Threads]]
[[0400 - Concurrency Patterns]]

---
## Expanded Details for Specific Sections

### 1. Global Queue and Local Run Queues

#### Global Queue:

- **Purpose:** Holds goroutines waiting for execution when they are not assigned to a specific P (Processor).
- **Behavior:**
    - Checked occasionally (approximately 1 out of every 61 scheduling cycles) to avoid becoming a bottleneck.
    - Acts as a fallback when local run queues are empty.
    - Ensures fairness by allowing goroutines created by different Ps to have a chance of being executed across the system.

#### Local Run Queues:

- **Purpose:** Each P maintains its own queue of runnable goroutines, which is the primary source for scheduling.
- **Behavior:**
    - When a goroutine is created or becomes runnable, it is added to the local run queue of the current P.
    - Each P prefers to execute goroutines from its own local queue for efficiency.
    - **Work Stealing:** If the local run queue is empty, P attempts to steal goroutines from another P’s queue.

---

### 2. Lifecycle of a Goroutine

1. **Creation:**
    - A new goroutine is created using the `go` keyword.
    - It is added to the local run queue of the P associated with the creating M.
    
1. **Execution:**
    - The scheduler assigns an M (OS thread) to the P holding the goroutine.
    - The goroutine runs on the assigned M until:
        - It completes execution.
        - It encounters a blocking operation.
        - It is preempted by the scheduler for fairness or resource management.
        
1. **Blocking:**
    - Happens when a goroutine waits for external resources, such as:
        - Network I/O.
        - File I/O.
        - Channel operations.
    - During blocking:
        - The M associated with the P may also block (e.g., during a syscall).
        - The scheduler detaches the M from the P and parks the M.
        - The P is freed to schedule another runnable goroutine.
2. **Unblocking:**
    - Once the blocking operation completes, the goroutine is marked as runnable.
    - It is added back to the local run queue of the P (or the global queue if necessary).
    
1. **Termination:**
    - A goroutine completes execution when its function returns.
    - The resources associated with the goroutine are released.

---

### 3. Handling Idle and Spinning Threads

#### Spinning Threads:

- **Purpose:** Minimize preemption and latency while balancing CPU resource usage.
- **When Threads Spin:**
    - An M with a P assignment looks for runnable goroutines in queues or via work stealing.
    - An M without a P assignment looks for an idle P.
    - When a new runnable goroutine is added and no spinning threads are available, the scheduler unparks an additional thread to spin.
- **Limits:**
    - At most `GOMAXPROCS` spinning Ms are allowed at any time to control CPU usage.

#### Idle Threads:
- Idle threads are parked to save CPU resources when:
    - No work is available in the queues.
    - All runnable goroutines are currently blocked or being executed.

---

### 4. Optimizations in the Scheduler

#### Minimized Handoff:
- **Purpose:** Reduces latency and overhead by avoiding frequent movement of runnable goroutines between threads.
- **How It Works:**
    - A goroutine runs on the same P-M pairing whenever possible.
    - Handoff (switching threads) is only done when absolutely necessary (e.g., work stealing or syscall blocking).

#### Spinning Threads Optimization:
- **Advantages:**
    - Keeps CPUs active to ensure no runnable goroutines are left idle.
    - Balances power efficiency by limiting the number of spinning threads.
