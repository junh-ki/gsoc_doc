**Future Work**
===============

Many implementations and tests have been left for the future due to the limited time but the topic has so much potential to be developed further. 
The future work concerns the followings: 

**1. Reaction Update**
----------------------

The current implementation covers `Early Reaction` but does not cover `Reaction Update`.
To calculate `Reaction Update`, the number of sampled task-chain entity instances should be taken into account first, and then `Early Reaction` can finally be utilized to get `Reaction Update`.

**2. Blocking**
---------------

The current implementation only focuses on preemtive tasks but does not cover cooperative tasks.
Preemptive tasks preempt each other at any moment in time while cooperative tasks preempt each other at runnable boundaries.
Therefore, the preempting task should be blocked until the currently executing runnable of the preempted task to finish.

**3. Scheduling mode: EDF**
---------------------------

The type of real-time scheduling algorithm used in this project is RMS (Rate Monotonic Scheduling).
Under RMS, a task with the shorter period obtains a higher priority.
To analyze different response times and mapping scenarios, extending the current scheduling algorithm further to EDF (Earliest Deadline First).
Under EDF, tasks are sorted by using their deadlines.
Therefore, a task which has the earliest deadline runs first.

**4. Read & Write latency setting feature**
-------------------------------------------

The current implementation derives `memory access costs` with `read` and `write` latency attribute values from the processing unit. 
If the selected model does not describe these attributes, the default latency value is assigned to the processing unit and then the `memory access costs` is calculated.
Therefore, having a GUI feature for assigning these attribute values is reasonable and useful for users to analyze with different processing unit configurations.

**5. Data Age metrics should be organized**
-------------------------------------------

Currently, the GUI features for `Data Age` latency are not well-designed because the list for label names and the rest of the lists for latency values are not synchronized.
Therefore, this should be restructured in a more tidy way to prevent possible confusions.

With these extensions, `APP4RTA` users can analyze response times under more various configuration settings with the better quality of GUI features.
