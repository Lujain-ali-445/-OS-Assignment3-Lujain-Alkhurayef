# Assignment 3 - Complete Documentation

**Student Name**: [Lujain ali alkhureyef]  
**Student ID**: [445052022]  
**Date Submitted**: [7/5/2026]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[445052022]_Assignment3_Synchronization.mp4`

**Verification**:
- [✔ ] Link is accessible (tested in incognito mode)
- [✔ ] Video is 3-5 minutes long
- [✔ ] Video shows code walkthrough and commits
- [ ✔] Video has clear audio
- [✔ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [Date, Time]
**What I implemented**:Forked the GitHub repository, cloned the project using VS Code, and updated my student ID in the Java file.

**Challenges encountered**:The Java file was opened in read-only mode and could not be edited.

**How I solved it**:I reopened the project folder correctly in VS Code and fixed the file access issue. 

**Testing approach**: Verified that the file could be edited and saved successfully.

**Time spent**: 30 minutes

---

### Entry 2 - [Date, Time]
**What I implemented**:Added ReentrantLock to protect shared counters and execution logs from race conditions.

**Challenges encountered**:There were red underline errors because required imports were missing. 

**How I solved it**:Added Semaphore and ReentrantLock import statements at the top of the file. 

**Testing approach**:Ran the program multiple times to ensure no synchronization errors occurred. 

**Time spent**:45 minutes 

---

### Entry 3 - [Date, Time]
**What I implemented**: Implemented Semaphore to control concurrent CPU access and completed assignment documentation.

**Challenges encountered**:GitHub changes were not updating after editing the code 

**How I solved it**: Used Commit and Sync Changes in VS Code to upload all modifications to GitHub.

**Testing approach**:Checked GitHub repository and confirmed all updated code was uploaded correctly. 

**Time spent**: 40 minutes

---

### Entry 4 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

### Entry 5 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

[Two race conditions existed in the original code. The first race condition affected the shared counters such as contextSwitchCount and completedProcessCount. Concurrent access was a problem because multiple threads could update the counters at the same time, causing incorrect values and inconsistent results. For example, two threads might increment the same counter simultaneously and one update could be lost.

The second race condition affected the executionLog ArrayList. Multiple threads accessing and modifying the list concurrently could cause ConcurrentModificationException or corrupted log entries. ReentrantLock was added to protect these shared resources and ensure thread-safe access.]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[ReentrantLock is used for mutual exclusion, meaning only one thread can access a critical section at a time. In this assignment, ReentrantLock was used to protect shared counters and execution logs from race conditions.

Semaphore is used to control access to a limited resource using permits. A binary semaphore with one permit was used in this assignment to limit CPU access and ensure that only one process executes at a time. Locks protected shared data while Semaphore controlled concurrent process execution.]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[Deadlock occurs when two or more threads wait for each other indefinitely while holding resources. In this assignment, deadlocks were prevented by using proper synchronization techniques and releasing locks correctly using try-finally blocks.

The locks were always unlocked in the finally section to ensure resources were released even if an exception occurred. Only necessary critical sections were locked and locks were not nested unnecessarily. The semaphore was also released properly after CPU execution. These practices helped prevent deadlocks and ensured smooth thread execution.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[I used separate locks for each shared counter instead of one lock for all counters. This is considered fine-grained locking because each resource has its own lock. I made this choice because the counters are independent and can be updated separately by different threads.

Using separate locks improves concurrency because multiple threads can work on different counters simultaneously without blocking each other. In contrast, using one shared lock for all counters would reduce performance because threads would wait even when accessing unrelated resources.

The advantage of coarse-grained locking is simpler code and easier management, but it reduces parallelism. Fine-grained locking provides better performance and concurrency but requires more careful implementation. Since the counters are independent in this assignment, fine-grained locking provides better concurrency and efficiency.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: contextSwitchCount, completedProcessCount, and totalWaitingTime


**Why they need protection**:These variables are shared between multiple threads. Without synchronization, multiple threads may update them at the same time causing race conditions and incorrect values.
 

**Synchronization mechanism used**: ReentrantLock

**Code snippet**:
contextSwitchLock.lock();
try {
    contextSwitchCount++;
} finally {
    contextSwitchLock.unlock();
}


**Justification**:ReentrantLock was used to ensure that only one thread can modify the shared counters at a time. This prevents race conditions and guarantees consistent counter values. 

---

### Critical Section #2: Execution Log

**What resource**:executionLog ArrayList 

**Why it needs protection**:Multiple threads may access and modify the execution log simultaneously which can cause ConcurrentModificationException or corrupted log entries.
 

**Synchronization mechanism used**:ReentrantLock 

**Code snippet**:
logLock.lock();
try {
    executionLog.add(logEntry);
} finally {
    logLock.unlock();
}

**Justification**:The lock ensures thread-safe access to the shared execution log and prevents concurrent modification problems. 

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**:The semaphore was used to control CPU access and allow only one process to execute at a time. 

**Number of permits and why**: A binary semaphore with 1 permit was used because only one process should access the CPU at a time.

**Where implemented**:The semaphore was implemented around the CPU execution section in the scheduler simulation. 

**Code snippet**:
cpuSemaphore.acquire();

try {
    executeProcess();
} finally {
    cpuSemaphore.release();
}

**Effect on program behavior**:The semaphore prevented multiple processes from executing simultaneously and improved synchronization consistency in the program. 

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 

**Results**: 

**What this proves**: 

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 

**Actual values**: 

**Analysis**: 

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 

**Results**: 

**What I learned**: 

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 

**Example 2**: 

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---

## Part 6: GitHub Repository Information

**Repository URL**: 

**Number of commits**: 

**Commit messages**: 
1. 
2. 
3. 
4. 

---

## Summary

**Total time spent on assignment**: 

**Key takeaways**: 
1. 
2. 
3. 

**Most challenging aspect**: 

**What I'm most proud of**: 

---

**End of Documentation**
