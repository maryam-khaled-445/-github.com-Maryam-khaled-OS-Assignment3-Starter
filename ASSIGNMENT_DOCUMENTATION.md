# Assignment 3 - Complete Documentation

**Student Name**: [Maryam khaled Almosaed]  
**Student ID**: [445052078]  
**Date Submitted**: [30/4/2026]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [https://drive.google.com/file/d/1onJfPCxrg0FqAFBfICQCvwd1rHe6l3tX/view?usp=sharing]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ T] Link is accessible (tested in incognito mode)
- [ T] Video is 3-5 minutes long
- [T ] Video shows code walkthrough and commits
- [ T] Video has clear audio
- [ T] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)
Entry 1 - 25/4/2026, 6:00 PM
Document your development process with **minimum 3 entries** showing progression:

What I implemented:
Started understanding the given code and how threads and processes are created.

Challenges encountered:
Understanding how threads run concurrently.

How I solved it:
Reviewed thread examples and practiced simple programs.

Testing approach:
Ran basic thread code and observed output.

Time spent: 1 hour

---

Entry 2 - 26/4/2026, 7:00 PM

What I implemented:
Identified shared resources such as counters and execution log.

Challenges encountered:
Understanding race conditions.

How I solved it:
Studied how multiple threads affect shared variables.

Testing approach:
Ran program multiple times and noticed inconsistent results.

Time spent: 1.5 hours 

---

Entry 3 - 27/4/2026, 8:30 PM

What I implemented:
Added ReentrantLock to protect shared resources.

Challenges encountered:
Forgetting to unlock caused program freeze.

How I solved it:
Used try-finally blocks to ensure unlock.

Testing approach:
Ran program multiple times to verify stability.

Time spent: 2 hours 

---

Entry 4 - 28/4/2026, 9:00 PM

What I implemented:
Added Semaphore to control number of running processes.

Challenges encountered:
Understanding how permits work.

How I solved it:
Tested with different permit values.

Testing approach:
Observed how many threads run simultaneously.

Time spent: 1 hour
---

Entry 5 - 29/4/2026, 10:00 PM

What I implemented:
Final testing and debugging.

Challenges encountered:
Ensuring no exceptions occur.

How I solved it:
Handled errors and tested edge cases.

Testing approach:
Ran program multiple times.

Time spent: 1 hour

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

[Question 1: Race Conditions

Your Answer:

The first race condition occurs in shared counters such as contextSwitchCount, completedProcessCount, and totalWaitingTime. These variables are updated by multiple threads using operations like:

contextSwitchCount++;

This operation is not atomic, so multiple threads may overwrite each other's updates, resulting in incorrect values.

The second race condition occurs in the executionLog list. Since ArrayList is not thread-safe, multiple threads adding elements at the same time may cause inconsistent data or runtime errors.]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[ReentrantLock is used to ensure mutual exclusion, allowing only one thread to access shared resources at a time.

Semaphore allows a fixed number of threads to access a resource simultaneously using permits.

In my code, I used ReentrantLock to protect shared variables such as counters and execution log. I used Semaphore to limit the number of processes running at the same time, simulating CPU cores.]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[Deadlock occurs when threads wait indefinitely for resources held by each other.

One prevention technique is using try-finally blocks to ensure locks are always released. Another technique is avoiding nested locks.

In my code, I used try-finally blocks to release both the lock and semaphore, ensuring resources are not held forever.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[I used one lock for all shared counters and execution log, which is coarse-grained locking.

This approach is simple and reduces complexity. However, it limits concurrency because only one thread can access shared resources at a time.

Fine-grained locking would allow multiple threads to update different variables simultaneously, improving performance.

Since the counters are independent, fine-grained locking provides better concurrency, but I chose coarse-grained locking for simplicity and safety.]

---

## Part 3: Synchronization Analysis (1 mark)

Critical Section #2: Counter Variables

Which variables:
contextSwitchCount, completedProcessCount, totalWaitingTime

Why they need protection:
They are shared among multiple threads and updated concurrently

Synchronization mechanism used:
ReentrantLock

Code snippet:

lock.lock();
try {
    contextSwitchCount++;
} finally {
    lock.unlock();
}

Justification:
Prevents race conditions and ensures correct values
---

Critical Section #2: Execution Log

What resource:
executionLog (ArrayList)

Why it needs protection:
ArrayList is not thread-safe and may corrupt data

Synchronization mechanism used:
ReentrantLock

Code snippet:

lock.lock();
try {
    executionLog.add(message);
} finally {
    lock.unlock();
}

Justification:
Ensures safe access to shared list
---

Critical Section #3: CPU Semaphore

Purpose of semaphore:
Limit number of concurrent processes

Number of permits and why:
2 permits to simulate dual-core CPU

Where implemented:
Inside run() method before execution

Code snippet:

SharedResources.cpuSemaphore.acquire();
try {
    // process execution
} finally {
    SharedResources.cpuSemaphore.release();
}

Effect on program behavior:
Controls concurrency and prevents overload
---

#Part 4: Testing and Verification (2 marks)
Test 1: Consistency Check

Testing procedure:

java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync

Results:
The output remained consistent in all runs

Why synchronization is necessary:
Without synchronization, shared variables may produce incorrect values due to race conditions

Conclusion:
Synchronization ensures correct and stable results
---

Test 2: Exception Testing

Testing procedure:
Ran the program multiple times while accessing shared list

Results:
No ConcurrentModificationException occurred

What this proves:
Shared resources are properly synchronized

Test 3: Correctness Verification

Expected values:
Correct total waiting time and completed processes

Actual values:
Matched expected values

Analysis:
Program is working correctly

Test 4: Different Scenarios

Scenario tested:
Different number of processes and time quantum

Purpose:
Test behavior under different conditions

Results:
Program handled all scenarios correctly

What I learned:
Synchronization maintains correctness under different loads
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

I learned that synchronization is important when multiple threads access shared data. Without synchronization, race conditions can lead to incorrect results. Locks ensure only one thread accesses critical sections at a time. Semaphores help control how many threads run simultaneously. I also learned how to prevent deadlocks using proper coding practices. Testing multiple times is important to verify consistency. Overall, synchronization improves reliability and correctness.
---

### Real-world applications:

Example 1: Banking systems (updating account balances)

Example 2: Operating systems (CPU scheduling)

How I would explain synchronization to others:

Synchronization is like taking turns. If multiple people want to use the same resource, only one can use it at a time. Locks ensure fairness and prevent conflicts.


---

Part 6: GitHub Repository Information

Repository URL: []

Number of commits: 4

Commit messages:

Initial setup
Added shared resources
Implemented synchronization
Final fixes
Summary

Total time spent on assignment: 6.5 hours

Key takeaways:

Importance of thread safety
Difference between locks and semaphores
Preventing race conditions

Most challenging aspect:
Understanding synchronization concepts

What I'm most proud of:
Successfully implementing thread-safe code
---

**End of Documentation**
