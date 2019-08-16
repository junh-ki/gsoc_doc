************************
Implementation (APP4RTA)
************************

A. Approached Theories
######################

<Basic RTA>
===========

1. Memory Accessing Time
************************

Memory accessing time is different depending upon the target hardware.
In this project, the memory accessing time is defined based on NVIDIA-TX2 platform.
The equation for deriving this is referenced from one of WATERS19 projects, `CPU-GPU Response Time and Mapping Analysis for High-Performance Automotive Systems <https://www.ecrts.org/forum/viewtopic.php?f=43&t=134&sid=777ff03160a9434451d721748c8a8aea#p264>`_

:math:`L_{a,i}^+ = \sum_{x \in \mathcal{R}_i} \left( \left\lceil \frac {\mathcal{S}_x} {64} \right \rceil \right) \cdot \frac {L_{\uparrow m\to l}} {f_m} + \sum_{y \in \mathcal{W}_i} \left(  \left \lceil \frac {\mathcal{S}_y} {64} \right \rceil \right) \cdot \frac {L_{\downarrow m\to l}} {f_m}`

The constant 64 is used here as the baseline derived from the challenge WATERS19.
Here, :math:`ls` denotes the label size and :math:`rl` and :math:`wl` define given read label and write label latencies specified in the given AMALTHEA model.

2. Synchronous & Asynchronous Mechanism
***************************************

In the AMALTHEA model, some of the tasks that are mapped to CPU trigger the tasks that are mapped to GPU.
In this case, the execution or response time can be different according to the mechanism.

* Synchronous

The triggering task triggers its target GPU task when it reaches `InterProcessStimulus` and waits until it gets the response from the triggered GPU task. Then it finishes the remaining job.

* Asynchronous

The triggering task triggers its target GPU task when it reaches `InterProcessStimulus` and does not wait for the response from the triggered GPU task and finishes the remaining job. The asynchronous methodology described here can be modified according to the user's interpretation.

3. Response Time
****************

The response time analysis approach implemented here is not only designed for Multi-core Systems but also for Heterogeneous Systems.
Basically, the classic response time analysis equation is used.

:math:`R_i = C_i + \sum_{j \in hp(i)} \lceil \frac {R_i} {T_j} \rceil C_j`

The equation is based on RMS (Rate Monotonic Scheduling) which means the static priorities are assigned according to the period of the task, so a task with the shorter period results in a higher task priority.
Here, R_i denotes the response time of a task with i-th priority in the set of tasks where hp(i) is the set of tasks with priority higher than task i.

<End to End Latency>
====================

The approach & equations used here are referenced from a yet-unpublished paper, "Model-based Task Chain Latency and Blocking Analysis for Automotive Software" by the same author who published `CPU-GPU Response Time and Mapping Analysis for High-Performance Automotive Systems <https://www.ecrts.org/forum/viewtopic.php?f=43&t=134&sid=777ff03160a9434451d721748c8a8aea#p264>`_.

4. Task Chain Reaction
**********************

The time between the task chain's first task release to the earliest task response of the last task in the chain.

* Best-case Task-Chain Reaction (Implicit Communication Paradigm)

:math:`\delta_{\gamma,\rho,\iota} ^-=\sum_j R_{j}^- \text{ with } \tau_j \in \gamma`

The best-case task chain reaction latency for implicit communication can be calculated by considering the sum of all task's best case response times within task chain.
Here, :math:`\gamma` refers to a task chain, :math:`\rho` corresponds the reaction latency, and :math:`\iota` outlines that this latency considers the implicit communication paradigm.

* Worst-case Task-Chain Reaction (Implicit Communication Paradigm)

:math:`\delta_{\gamma,\rho,\iota}^+ = \sum_{j=0}^{j=|\gamma|-2} \left(2\cdot T_{j}\right) +R_{j = |\gamma|-1}^+ \text{ with } \tau_j \in \gamma`

* Best-case Task-Chain Reaction (Logical Execution Time)

:math:`\delta_{\gamma,\rho,\lambda} ^- = \sum_j T_{j} \text{ with } \tau_j \in \gamma`

The best-case task chain reaction latency for LET communication is the sum of all task's periods within task chain :math:`\gamma`.

* Worst-case Task-Chain Reaction (Logical Execution Time)

:math:`\delta_{\gamma,\rho, \lambda}^+= T_{j=0}+\sum_{j=1}^{j=|\gamma|-1} \left(2\cdot T_{j}\right) \text{ with } \tau_j \in \gamma`

5. Task Chain Age
*****************

The time a task chain result is initially available until the next task chain instance's initial results are available.
A task chain age latency equals the chain's last (response) task age latency, i.e. :math:`\delta_{\gamma,\alpha} = \delta_{i,\alpha}` with :math:`\tau_i` being the last task of the task chain :math:`\gamma`, i.e. :math:`i=|\gamma|-1`.

* Best-case Task-Chain Age

:math:`\delta_{i, \alpha}^- = T_i - R_i^+ + R_i^-`

* Worst-case Task-Chain Age

:math:`\delta_{i,\alpha}^+ = 2 \cdot T_i - R_i^- - (T_i - R_i^+) = T_i - R_i^- + R_i^+`

6. Reaction Update
******************

Due to the fact that tasks can have varying periods across the task chain, propagation between task chain entities can be over or under sampled such that a task X's result (a) serves as an input for several subsequent task chain entity instances or (b) does not serve as an input at all due to the fact that the subsequent task can already work with newer results produced by X's next instance.

* Early Reaction

:math:`\delta_{\gamma, \rho 0, \iota}^+ = R_{\gamma0} + \sum_{j=0}^{j = |\gamma|-2} T_{j+1} + \min(T_{j+1}, \epsilon_j + R_{j+1})`

:math:`\epsilon_j = 2\cdot T_{j} - R_{j} - T_{j+1} - \epsilon_{j-1}`

:math:`\epsilon_{-1} = 0`

* Reaction Update

Accordingly, the reaction update is the subtraction of two consecutive task chains instances best case early reaction and worst case early reaction.

:math:`\delta_{\gamma, \upsilon, \iota}^+ = \max_{k} \left(T_{j=0} + \delta_{\gamma, \rho 0, \iota, k+1}^+ - \delta_{\gamma, \rho , \iota, k}^- \right)`

7. Data Age
***********

It describes the longest time some data version persists in memory. 
This is independent of task chains and simply depends on the period of entities writing a particular label (i.e. data).

* Best-case Data Age

:math:`\delta_{l,\alpha}^+ = \min_i \delta_{i,\alpha}^+` 
with :math:`\tau_i` being any task that accesses label :math:`l`.

* Worst-case Data Age

:math:`\delta_{l,\alpha}^- = \min_i \delta_{i,\alpha}^- %R_i^- + (T_i - R_i^+)` 
with :math:`\tau_i` being any task that accesses label :math:`l`.

B. Class Tree with Implemented Methods
######################################



C. APP4RTA User Interface
#########################



D. Git Repository
#################



`yahoo <http://yahoo.com>`_
.. https://www.ecrts.org/forum/viewtopic.php?f=43&t=134&sid=777ff03160a9434451d721748c8a8aea#p264