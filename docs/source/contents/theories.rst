**Approached Theories**
#######################

|

**Basic RTA**
*************

* Table of Notation for **Basic RTA**
=================  =============================
Description 	   Symbol
=================  =============================
Task 			   :math:`i`
WC Response time   :math:`R_i^+`
WC Execution time  :math:`C_i^+`
Period 			   :math:`T_i`
Frequency in Hz	   :math:`f_m`
Latency 		   :math:`L`
Read Latency 	   :math:`L_{\uparrow m\to l}`
Write Latency 	   :math:`L_{\downarrow m\to l}`
Read labels 	   :math:`\mathcal{R}_i`
Written Labels 	   :math:`\mathcal{W}_i`
Label 			   :math:`\mathcal{L}`
Label Size 		   :math:`\mathcal{S}`
=================  =============================

|

.. _memory-accessing-cost:

**Memory Access Cost**
======================

Memory access time is different depending on the target hardware.
In this project, the memory access time is defined based on NVIDIA-TX2 platform.
The equation for deriving this is referenced the WATERS19 projects namely `CPU-GPU Response Time and Mapping Analysis for High-Performance Automotive Systems <https://www.ecrts.org/forum/viewtopic.php?f=43&t=134&sid=777ff03160a9434451d721748c8a8aea#p264>`_

:math:`L_{a,i} = \sum_{x \in \mathcal{R}_i} \left( \left\lceil \frac {\mathcal{S}_x} {64} \right \rceil \right) \cdot \frac {L_{\uparrow m\to l}} {f_m} + \sum_{y \in \mathcal{W}_i} \left(  \left \lceil \frac {\mathcal{S}_y} {64} \right \rceil \right) \cdot \frac {L_{\downarrow m\to l}} {f_m}`

Here, the constant 64 is used as the baseline derived from the WATERS19 challenge description.
:math:`ls` denotes the label size and :math:`rl` and :math:`wl` define given read label and write label latencies specified in the given AMALTHEA model.

To find relevant methods, see :ref:`method-task-execution-time`.

|

.. _offloading-mechanism:

**Synchronous & Asynchronous Mechanism**
========================================

In the provided AMALTHEA WATERS19 model, some of the tasks that are mapped to CPU trigger tasks mapped to GPU.
In this case, the execution or response time can be different according to the offloading mechanism.

.. image:: /_images/offloading.PNG 
	:width: 400
	:align: center

* **Synchronous**

The triggering task triggers its target GPU task when it reaches `InterProcessTrigger` and actively waits until it receives the triggered task's result after the response from the triggered GPU task. Then it finishes the remaining job.

* **Asynchronous**

The triggering task triggers its target GPU task when it reaches `InterProcessTrigger` and passively waits for the response from the triggered GPU task and finishes the remaining job. 
During the passive waiting phase, other lower priority tasks can execute on the processor.
The asynchronous methodology described here can be modified according to the user's interpretation.

This concept is used in two of the four execution cases introduced by a method, :ref:`method-task-execution-time`.

|

.. _wc-response-time:

**Worst-case Response Time**
============================

The response time analysis approach implemented here is not only designed for Multi-core Systems but also for Heterogeneous Systems.
Basically, the following classical response time analysis equation is used.

:math:`R_i^+ = C_i^+ + \sum_{j \in hp(i)} \left\lceil \frac {R_{i-1}^+} {T_j} \right\rceil C_j^+`

The equation is based on RMS (Rate Monotonic Scheduling) which means that static priorities are assigned to tasks according to their period. A task with the shorter period results in a higher task priority.
Here, :math:`R_i` denotes the response time of task :math:`\tau_i` and :math:`hp(i)` is the set of tasks indexes (j) which have a priority higher than task i.

To find relevant methods, see :ref:`method-response-time-sum`.

.. _bc-response-time:

**Best-case Response Time**
===========================

Unlike the worst-case analysis, considering all tasks arriving at the same point of time does not work since every new task instance can have a different response time after the first iteration as long as it is not the highest priority task. Hence, the response time analysis which takes this point into account is required.

:math:`R_i^{n+1} = C_i^- + \sum_{j \in hp(i)} \left\lceil \frac {R_i^n - J_j - T_j} {T_j} \right\rceil_0 C_j^- \mbox{ for } n = 0, 1, 2, ...`

.. math::

    \mbox{ with } R_i^0 = R_i^+

    \mbox{ where } \left\lceil x \right\rceil_0 = \max(0, \left\lceil x \right\rceil)

The equation is based on RMS (Rate Monotonic Scheduling) which means that static priorities are assigned to tasks according to their period. A task with the shorter period results in a higher task priority.

|

.. _e2e-latency:

**End-to-End Latency**
**********************

The approach and its equations used here are referenced from a yet-unpublished paper, "Model-based Task Chain Latency and Blocking Analysis for Automotive Software" by the same authors who published `CPU-GPU Response Time and Mapping Analysis for High-Performance Automotive Systems <https://www.ecrts.org/forum/viewtopic.php?f=43&t=134&sid=777ff03160a9434451d721748c8a8aea#p264>`_.

* Table of Notation for **End-to-End Latency**
======================  ================
Symbol 					Description
======================  ================
Task 					:math:`\tau`
Response time 			:math:`R`
Execution time  		:math:`C`
Period 					:math:`T`
Task chain 				:math:`\gamma`
Latency 				:math:`\delta`
implicit communication  :math:`\iota`
LET communication 		:math:`\lambda`
Age latency 			:math:`\alpha`
Reaction latency 		:math:`\rho`
Reaction update 		:math:`\upsilon`
======================  ================

|

.. _task-chain-reaction:

**Task Chain Reaction**
=======================

The time between the task chain's first task release to the earliest task response of the last task in the chain.

|

.. _task-chain-reaction-implicit:

**Task Chain Reaction (Implicit Communication Paradigm)**
---------------------------------------------------------

* **Best-case Task-Chain Reaction (Implicit)**

:math:`\delta_{\gamma,\rho,\iota}^- = R_{\gamma_0}^- + \sum_{j=1}^{|\gamma|-1} (R_j^- - ip(\tau_j)^-)`

.. math::
    
    ip(\tau_j)^- = R_j^- - C_j^-

The best-case task chain reaction latency with Implicit communication can be calculated by summing up the first chain element's best-case response time and the rest of all task's best-case response times which are subtracted by each task's best-case initial pending time.
Here, :math:`\gamma` refers to a task chain, :math:`\rho` corresponds the reaction latency, :math:`\iota` is Implicit communication paradigm, and :math:`ip(\tau_j)^-` stands for the best-case initial pending time of :math:`tau_j`.

* **Worst-case Task-Chain Reaction (Implicit)**

:math:`\delta_{\gamma,\rho,\iota}^+ = T_{\gamma_0} + R_{\gamma_0}^+ + \sum_{j=1}^{|\gamma|-1} (R_j^+ + f(j))`

.. math::

    f(j) =  
        \begin{cases}
            T_j - ip(\tau_j)^- & \text{ if } j \ne |\gamma|-1 \\ 
            - ip(\tau_j)^+ & \text{ else }
        \end{cases}

:math:`ip(\tau_j)^+ = f(0, R_{(j-1) \in hp(j)}^+)`

.. math::

    f(k, R) = 
        \begin{cases}
            R & \text{ if } \mbox{ } (k == |hp(j-1)|) \mbox{ } \lor \mbox{ } (R < T_{hp(j-1)_k}) \\ 
            f(k+1, R) & \text{ else if } \mbox{ } (R \mathbin{\%} T_{hp(j-1)_k}) \ne 0 \\
            f(0, R+C_{hp(j-1)_k}^+) & \text{ else }
        \end{cases}

To find relevant methods, see :ref:`method-task-chain-reaction-implicit`.

* **Best-case Task-Chain Initial Reaction (Implicit)**

:math:`\delta_{\gamma,\rho_0,\iota}^- = \delta_{\gamma,\rho,\iota}^-`

The best-case reaction is always equal to the best-case initial reaction of a task chain with Implicit communication.

* **Worst-case Task-Chain Initial Reaction (Implicit)**

:math:`\delta_{\gamma,\rho_0,\iota}^+ = R_{\gamma_0}^+ + \sum_{j=1}^{|\gamma|-1}(R_j^+ + f(j))`

.. math::

    f(j) = 
        \begin{cases}
            T_{j-1} - ip(\gamma_j)^+ & \text{ if } \mbox{ } (T_j \geq T_{j-1}) \land \mbox{ } (|\gamma|-1 < 3) \\
            - ip(\gamma_j)^+ & \text{ else if } \mbox{ } (T_j \geq T_{j-1}) \land \mbox{ } (j == |\gamma|-1) \\
            T_{j-1} + T_j - ip(\gamma_j)^- & \text{ else if } \mbox{ } (T_j \geq T_{j-1}) \mbox{ } \land \mbox{ } (j == 1) \\
            T_j - ip(\gamma_j)^- & \text{ else }
        \end{cases}

|

.. _task-chain-reaction-let:

**Task Chain Reaction (Logical Execution Time Communication Paradigm)**
-----------------------------------------------------------------------

* **Best-case Task Chain Reaction (LET)**

:math:`\delta_{\gamma,\rho,\lambda}^- = \sum_{j=0}^{|\gamma|-1} T_j \mbox{ with } \tau_j \in \gamma`

The best-case task chain reaction latency for LET communication is the sum of all task's periods within task chain :math:`\gamma`.

* **Worst-case Task Chain Reaction (LET)**

:math:`\delta_{\gamma,\rho,\lambda}^+ = \sum_{j=0}^{|\gamma|-2} (2 \cdot T_j) + T_{|\gamma|-1}`

To find relevant methods, see :ref:`method-task-chain-reaction-let`.

* **Best-case Task Chain Initial Reaction (LET)**

:math:`\delta_{\gamma,\rho_0,\lambda}^- = \delta_{\gamma,\rho,\lambda}^-`

The best-case reaction is always the initial reaction of a task chain.

* **Worst-case Task Chain Initial Reaction (LET)**

:math:`\delta_{\gamma,\rho_0,\lambda}^+ = T_0 + \sum_{j=1}^{|\gamma|-1} (T_j + f(j))`

.. math::

    f(j) =  
        \begin{cases}
            T_{j-1} & \text{ if } (T_j > T_{j-1}) \\
            T_j & \text{ else }
        \end{cases}

|

.. _task-chain-age:

**Task Chain Age**
==================

"The time a task chain result is initially available until the next task chain instance's initial results are available. In other words, the task chain age latency is the maximal time a task chain's results based on the same input persist in memory."

* **Best-case Task Chain Age (Implicit)**

:math:`\delta_{\gamma,\alpha,\iota}^- = T_{\gamma_0} + \delta_{\gamma,\rho_0,\iota}^- - \delta_{\gamma,\rho,\iota}^+`

* **Worst-case Task Chain Age (Implicit)**

:math:`\delta_{\gamma,\alpha,\iota}^+ = T_{\gamma_0} + \delta_{\gamma,\rho_0,\iota}^+ - \delta_{\gamma,\rho_0,\iota}^-`

* **Worst-case Task Chain Age (LET)**

:math:`\delta_{\gamma,\alpha,\lambda}^- = T_{|\gamma|-1}`

* **Worst-case Task Chain Age (LET)**

:math:`\delta_{\gamma,\alpha,\lambda}^+ = T_{\gamma_0} + \sum_{j=1}^{|\gamma|-1}f(j)`

.. math::

    f(j) =  
        \begin{cases}
            2 \cdot T_j - f(j-1) & \text{ if } \mbox{ } (T_j == f(j-1)) \\
            T_j - f(j-1) & \text{ else if } \mbox{ } (T_j > f(j-1)) \\
            T_j \cdot \left\lceil \frac{f(j-1)}{T_j} \right\rceil - f(j-1) & \text{ else if } \mbox{ } (T_j < f(j-1)) \mbox{ } \land \\
             & \mbox{ }\mbox{ }\mbox{ }\mbox{ }\mbox{ }\mbox{ }\mbox{ }\mbox{ }\mbox{ }\mbox{ }\mbox{ } (f(j-1) \mathbin{\%} T_j \ne 0) \\
            T_j \cdot (\frac{f(j-1)}{T_j} + 1) - f(j-1) & \text{ else }
        \end{cases}

.. math::
    \mbox{ with } f(0) = T_0

|

.. _task-age:

**Task Age**
============

The Task Age refers to the time from a task instance's result is available until the next instance's result from the same task appears.

* **Best-case Task Age (Implicit)**

:math:`\delta_{j,\alpha}^- = (T_j - R_j^+) + R_j^- = T_j - R_j^+ + R_j^-`

* **Worst-case Task Age (Implicit)**

:math:`\delta_{j,\alpha}^+ = (T_j - R_j^-) + R_j^+ = T_j - R_j^- + R_j^+`

|

.. _data-age:

**Data Age**
============

It describes the longest time some data version persists in memory. 
This is independent of task chains and simply depends on the period of entities writing a particular label (i.e. data).

* **Best-case Data Age**

:math:`\delta_{l,\alpha}^- = \min_{\gamma_l} \delta_{{\gamma_l}_j, \alpha}^-` 
with :math:`\gamma_l` being all tasks that access label :math:`l`.

* **Worst-case Data Age**

:math:`\delta_{l,\alpha}^+ = \min_{\gamma_l}\delta_{{\gamma_l}_j, \alpha}^+` 
with :math:`\gamma_l` being all tasks that access label :math:`l`.

To find relevant methods, see :ref:`method-data-age`.
