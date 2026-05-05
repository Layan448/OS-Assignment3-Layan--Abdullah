# Assignment 3 - Complete Documentation

**Student Name**: [Layan Abdullah]  
**Student ID**: [444052747]  
**Date Submitted**: [2026/5/5]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[444052747]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [Day 1 – April 30, 2026]
**What I implemented**: 
 I began by examining the Round Robin scheduler code that was provided. I comprehended how the time quantum is used during execution and how processes are kept in a queue. Additionally, I found shared variables like execution logs and counters.

**Challenges encountered**: 

Understanding how processes return back to the queue after partial execution was initially confusing.

**How I solved it**: 
 
I traced the code step-by-step and added temporary print statements to follow process flow.

**Testing approach**: 
Ran the program with sample processes and observed execution order.

**Time spent**: 
Time spent: 2 hours

---

### Entry 2 - [May 1, 2026, 3:00 PM]
**What I implemented**: 
I found important parts of the code, such shared counters and logs. I began using ReentrantLock to implement synchronization.

**Challenges encountered**: 

Ensuring locks are always released correctly without causing deadlocks.

**How I solved it**: 
 Used try-finally blocks to guarantee lock release.

**Testing approach**:
Ran the program multiple times to check for inconsistencies 

**Time spent**: 
Time spent: 2.5 hours

---

### Entry 3 - [May 1, 2026, 9:00 PM]
**What I implemented**: 
I implemented a Semaphore to control access to CPU simulation and limit concurrent execution

**Challenges encountered**: 
Choosing the correct number of permits

**How I solved it**: 
 Set permits to 1 to simulate a single CPU environment.

**Testing approach**: 
Observed behavior when multiple threads attempt execution.

**Time spent**: 
Time spent: 1.5 hours

---

### Entry 4 - [May 2, 2026, 2:00 PM]
**What I implemented**: 
I improved synchronization for execution logs and ensured thread-safe modifications.

**Challenges encountered**: 
Preventing ConcurrentModificationException

**How I solved it**: 
Protected shared collections using locks

**Testing approach**:
Simulated concurrent access scenarios. 

**Time spent**: 
Time spent: 2 hours

---

### Entry 5 - [May 2, 2026, 8:00 PM]
**What I implemented**: 
Final testing and verification of all synchronization mechanisms.

**Challenges encountered**: 
 Ensuring consistent output across runs.

**How I solved it**: 
 Repeated execution and verified correctness manually.

**Testing approach**:
Ran program 5+ times and compared outputs. 

**Time spent**: 
 Time spent: 1.5 hours

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?
When several threads update shared counter variables, like total burst time or context switches, the first race condition arises. Because these variables are shared, overwriting during concurrent updates may result in inaccurate values.

When several threads try to write at the same time, the second race situation takes place in the execution log. Logs may become incorrect or corrupted as a result.

Because activities like increasing a counter are not atomic, concurrent access is problematic. Incorrect behavior, including missing updates or inconsistent output, could happen in the absence of synchronization.




---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?


A ReentrantLock is used for mutual exclusion, allowing only one thread to access a critical section at a time. A Semaphore, on the other hand, controls access to a resource by allowing a fixed number of threads.

In my code, I used ReentrantLock to protect shared resources such as counters and logs. I used a Semaphore to simulate CPU access, ensuring only one thread executes at a time.

This combination provides both safety (locks) and control over concurrency (semaphore).


---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.


Deadlock occurs when two or more threads are waiting for each other indefinitely, preventing progress.

One prevention technique is using a consistent lock ordering, ensuring all threads acquire locks in the same sequence.

Another technique is using try-finally blocks to guarantee locks are always released.

In my code, I prevented deadlocks by using try-finally for every lock and avoiding nested locks whenever possibl

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?


I used fine-grained locking by assigning separate locks for each counter. This approach allows multiple threads to update different counters simultaneously without blocking each other.

The reason for this choice is that the counters are independent and do not rely on each other. Using a single lock would reduce concurrency and create unnecessary blocking.

The trade-off is that fine-grained locking increases complexity, while coarse-grained locking is simpler but less efficient.

Since the counters are independent, fine-grained locking provides better concurrency and improves performance.


[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: totalBurstTime, contextSwitches, completedProcesses 

**Why they need protection**: They are shared across multiple threads and updated concurrently

**Synchronization mechanism used**:  ReentrantLock

**Code snippet**:
lock.lock();
try {
    totalBurstTime += burst;
} finally {
    lock.unlock();
}
```

**Justification**: Ensures atomic updates and prevents race conditions.

---

### Critical Section #2: Execution Log

**What resource**:  Shared log list

**Why it needs protection**:  
Multiple threads may write simultaneously causing inconsistency.

**Synchronization mechanism used**: ReentrantLock

**Code snippet**: 
 logLock.lock();
try {
    executionLog.add(message);
} finally {
    logLock.unlock();
}

**Justification**: Prevents concurrent modification issues.

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: Control access to CPU simulation

**Number of permits and why**: 1 permit to simulate a single CPU

**Where implemented**: Before process execution

**Code snippet**:
cpuSemaphore.acquire();
try {
    executeProcess();
} finally {
    cpuSemaphore.release();
}

**Effect on program behavior**:  Ensures only one process executes at a time, maintaining correct scheduling.

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
java Main
java Main
java Main
java Main
java Main
**What I tested**: Running program multiple times to verify consistent results
**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: All runs produced consistent and correct outpu
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: Without synchronization, shared variables could produce incorrect results due to race conditions.
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**:  Synchronization ensures deterministic and correct behavior

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: Ran program under concurrent access conditions

**Results**: No ConcurrentModificationException occurred

**What this proves**: Synchronization is correctly implemented.

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: Correct total burst time and execution order

**Actual values**: 
Matched expected values

**Analysis**: Program behaves as intended.

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]
Different time quantum values
**Purpose**: Observe scheduling behavior

**Results**: Smaller quantum increased context switching

**What I learned**: 
Time quantum significantly affects performance

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

I learned that synchronization is essential when multiple threads access shared resources. Without it, race conditions can cause incorrect results. I also understood the importance of choosing the right synchronization mechanism, such as locks or semaphores. Additionally, I learned how to prevent deadlocks using proper coding practices. This assignment improved my understanding of concurrent programming and thread safety.

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: Banking systems where multiple transactions update account balances

**Example 2**: Operating systems managing multiple processes accessing CPU

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

--- Synchronization is like organizing people to use a shared resource one at a time. For example, if multiple people want to use a printer, only one person can use it at a time. Locks act like a key to the printer, and only the person holding the key can use it

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/Layan448/OS-Assignment3-Layan--Abdullah.git

**Number of commits**:  9 commits

**Commit messages**: 
1. change my student ID to 444052747
2. Add ReentrantLock for shared counters
3. Add Semaphore for CPU access control
4. protect shared countes using reentrantLock
5. proticting the shared variable (contextSwitchCount) using lock and in…
6. proticting shard variable (totalWaitingTime)
7. proticting (executionLog)
8. Always release in finally block to prevent deadlocks!
9. apply semaphore in runToCompletion



---

## Summary

**Total time spent on assignment**: 2 day

**Key takeaways**: 
1. Importance of synchronization
2. Difference between locks and semaphores
3. Preventing race conditions

**Most challenging aspect**: Understanding concurrent behavior and debugging race conditions

**What I'm most proud of**: Successfully implementing synchronization and ensuring correct program behavior

---

**End of Documentation**

