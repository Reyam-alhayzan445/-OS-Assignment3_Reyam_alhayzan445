# Assignment 3 - Complete Documentation

**Student Name**: [Reyam_alhayzan]  
**Student ID**: [445052172]  
**Date Submitted**: []

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [       ]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [[April 30, 2026, 6:30 PM]]
**What I implemented**: began by modifying the student ID in the SchedulerSimulationSync.java file and configuring the repository. In addition, I looked over the assignment specifications and found the shared resources that might lead to racial circumstances.

**Challenges encountered**: I wasn't entirely sure at first which variables required synchronization and which areas of the code were crucial.

**How I solved it**: I examined the shared variables utilized by several threads and went over the ideas of race situations and important passages from Operating System Concepts (10th Edition), Chapters 1 and 2.

**Testing approach**:After upgrading the student ID, I constructed and ran the application to make sure the original beginning code was operating properly.

**Time spent**: 2 hours

---

### Entry 2 - [April 30, 2026, 9:00 PM]
**What I implemented**: 
To safeguard the shared counters—contextSwitchCount, completedProcessCount, and totalWaitingTime—I used ReentrantLock.
**Challenges encountered**: At first, I neglected to release the lock in a few methods, which would have resulted in deadlocks.

**How I solved it**: employed try-finally blocks to ensure that, in the event of an exception, every lock is always released.

**Testing approach**: I ran the application multiple times to make sure the counter values didn't change.

**Time spent**: 
3 hours
---

### Entry 3 - [April 30, 2026, 10:30 PM]
**What I implemented**: To prevent several threads from accessing the shared ArrayList concurrently, a separate ReentrantLock was used to add synchronization for the execution log.

**Challenges encountered**: discovered that concurrent changes may result in incorrect behavior or runtime exceptions during execution, and that ArrayList is not thread-safe.

**How I solved it**:  made a special lock for the execution log and used try-finally blocks to encapsulate all changes to the ArrayList in lock and unlock actions.

**Testing approach**: 

I ran the scheduler simulation multiple times, keeping an eye on the execution log to make sure there were no corrupted log entries or ConcurrentModificationExceptions.
**Time spent**: 
2 hours
---

### Entry 4 - [May 1, 2026, 2:00 PM]

**Challenges encountered**: To mimic CPU access control and guarantee that only one process runs at a time, a binary semaphore was built.

**How I solved it**: I had to figure out where acquire() and release() should be performed.

**Testing approach**: observed the program's output and verified that processes ran one after the other without any synchronization problems.

**Time spent**: 
3 hours
---

### Entry 5 - [May 1, 2026, 6:00 PM]
**What I implemented**: I developed the documentation and video demonstration, finished the final testing, examined the output, and confirmed synchronized behavior.

**Challenges encountered**: ANSI color formatting and progress bar updates occasionally made the terminal output hard to read.

**How I solved it**: Instead of depending just on the visual formatting, I thoroughly examined the execution outcomes several times and concentrated on verifying the synchronization logic, process completion, and final statistics.

**Testing approach**: Using the same student ID, I ran the software multiple times to confirm that every operation was successful and that the synchronization data stayed accurate and constant.

**Time spent**: 2 hours

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:The shared counter variables contextSwitchCount and completedProcessCount contain the first race condition. Because the increment procedure is not atomic, multiple threads may try to increase these variables at the same time, resulting in inconsistent results. Certain increments might be missed in the absence of synchronization.

The executionLog ArrayList contains the second race condition. Concurrent changes made by several threads could alter the list's internal structure or result in inconsistent log entries because ArrayList is not thread-safe. It might potentially cause a ConcurrentModificationException in certain circumstances.

ReentrantLock was employed to safeguard the execution log and the shared counters in order to address these problems.

[Your answer here - 4-6 sentences with code examples]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:By only permitting one thread to visit a crucial part at a time, ReentrantLock offers mutual exclusion. Since shared counters and the execution log should only be altered by one thread at a time, I utilized ReentrantLock in this assignment to safeguard these resources.

Semaphore is distinct in that it uses permits to regulate access to a restricted set of resources. To mimic CPU access control and make sure that only one process runs on the CPU at a time, I utilized a binary semaphore with a single permit.

While the semaphore regulates resource access, the lock safeguards data consistency.

[Your answer here - explain your implementation choices]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:When several threads wait endlessly for resources owned by one another, a deadlock occurs. The software cannot continue to run because of this circumstance.

Try-finally blocks are one approach to ensure that locks are always released. Avoiding circular waiting between threads and reducing lock consumption are two further strategies.

By constantly releasing locks and semaphores inside finally blocks, I avoided deadlocks in my code. In order to lessen the chance of circular waiting, I additionally employed a straightforward synchronization approach devoid of nested locks.

[Your answer here - reference try-finally blocks, lock ordering, etc.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:employed a coarse-grained locking strategy by using a single shared ReentrantLock to safeguard all three counter variables. I selected this solution because it lowers implementation complexity and streamlines synchronization.

Coarse-grained locking has the benefit of being simpler to handle and less prone to programming errors. However, since only one thread may update any counter at a time, it might decrease concurrency.

Because independent counters can be updated concurrently, fine-grained locking permits more concurrency by using distinct locks for each counter variable. But it makes things more complicated and might incur more synchronization overhead.

Fine-grained locking may improve concurrency because the three counters are conceptually separate. Coarse-grained locking, however, proved enough and simpler to maintain securely for this task.

[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 

**Why they need protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #2: Execution Log

**What resource**: 

**Why it needs protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 

**Number of permits and why**: 

**Where implemented**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Effect on program behavior**: 

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