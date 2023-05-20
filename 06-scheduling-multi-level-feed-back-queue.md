# 6. Scheduling: The Multi-Level Feedback Queue (MLFQ)
The multi-level feedback queue is an example of a system that learns from the past to predict the future. It has multiple levels of queues, and uses feedback to determine the priority of a given job.

The MLFQ has a number of queues, each assigned a different priority level. At any given time, a job ready-to-run is on a single queue. MLFQ uses priorities to decide which job should run at a given time. More than one job may be on a given queue, and thus have the same priority. In this case, we will just use round-robin scheduling among those jobs.

Rather than giving a fixed priority to each job, MLFQ varies the priority of a job based on its observed behavior. If a job repeatedly relinquishes the CPU while waiting for input from the keyboard, MLFQ will keep its priority high, as this is how an interactive process might behave. If, instead, a job uses the CPU intensively for long periods of time, MLFQ will reduce its priority. In this way, MLFQ will try to learn about processes as they run, and thus use the history of the job to predict its future behavior.

- Rule 1: If Priority(A) > Priority(B), A runs (B doesn't).
- Rule 2: If Priority(A) = Priority(B), A & B run in RR.

Job priority changes over time. because it doesn't know whether a job will be a short job or a long-running job, it first assumes it might be a short job, thus giving the job high priority. If it actually is a short job, it will run quickly and complete; if it is not a short job, it will slowly move down the queues, and thus soon prove itself to be a long-running more batch-like process.

- Rule 3: When a job enters the system, it is placed at the highest priority.
- Tentative Rule 4a: If a job uses up an entire time slice while running, its priority is reduced by 1.
- Tentative Rule 4b: If a job gives up the CPU before the time slice is up, it stays at the same priority level.

With this, there is the problem of starvation: if there are too many interactive jobs, they will combine to consume all CPU time, and thus long-running jobs will never receive any CPU time (they starve). Also, a smart user could rewrite their program to game the scheduler. Moreover, a program may change its behavior over time; what was CPU-bound may transition to a phase of interactivity.

Scheduling must be secure from attack: Consider the modern datacenter, in which users from around the world share CPUs, memories, networks, and storage systems; without care in policy design and enforcement, a single user may be able to adversely harm others and gain advantage for itself. Thus, scheduling policy forms an important part of the security of a system, and should be carefully constructed. 

- Rule 5: After some time period S, move all the jobs in the system to the topmost queue.

This handles the starving problem, and the transition to interactive problem. To handle the smart gamer: 

- The new Rule 4: Once a job uses up its time allotment at a given level (regardless of how many times it has given up the CPU), its priority is reduced (moves down one queue).

Most MLFQ variants allow for varying time-slice length across different queues. The high-priority queues are usually given short time slices; they are comprised of interactive jobs, after all, and thus quickly alternating between them makes sense (e.g., 10 or fewer ms). The low-priority queues, in contrast, contain long-running jobs that are CPU-bound

One big question is how to parameterize such a scheduler. For example, how many queues should there be? How big should the time slice be per queue? How often should priority be boosted in order to avoid starvation and account for changes in behavior? There are no easy answers to these questions, and thus only some experience with workloads and subsequent tuning of the scheduler will lead to a satisfactory balance. Avoiding voo-doo constants is a good idea whenever possible. Unfortunately, it is often difficult. The frequent result: a configuration file filled with default parameter values, and a seasoned administrator can tweak when something isn't quite working correctly.

In this way, it manages to achieve the best of both worlds: it can deliver excellent overall performance (similar to SJF/STCF) for short-running interactive jobs, and is fair and makes progress for long-running CPU-intensive workloads.
