****************************
**Implementation (APP4RTA)**
****************************

|

**Approached Theories**
#######################

|

**Basic RTA**
*************

|

.. _memory-accessing-cost:

**1. Memory Accessing Cost**
============================

Memory accessing time is different depending upon the target hardware.
In this project, the memory accessing time is defined based on NVIDIA-TX2 platform.

The equation for deriving this is referenced from one of WATERS19 projects, `CPU-GPU Response Time and Mapping Analysis for High-Performance Automotive Systems <https://www.ecrts.org/forum/viewtopic.php?f=43&t=134&sid=777ff03160a9434451d721748c8a8aea#p264>`_

:math:`L_{a,i}^+ = \sum_{x \in \mathcal{R}_i} \left( \left\lceil \frac {\mathcal{S}_x} {64} \right \rceil \right) \cdot \frac {L_{\uparrow m\to l}} {f_m} + \sum_{y \in \mathcal{W}_i} \left(  \left \lceil \frac {\mathcal{S}_y} {64} \right \rceil \right) \cdot \frac {L_{\downarrow m\to l}} {f_m}`

The constant 64 is used here as the baseline derived from the challenge WATERS19.

Here, :math:`ls` denotes the label size and :math:`rl` and :math:`wl` define given read label and write label latencies specified in the given AMALTHEA model.

To find relevant methods, see :ref:`method-task-execution-time`.

|
|

.. _offloading-mechanism:

**2. Synchronous & Asynchronous Mechanism**
===========================================

In the AMALTHEA model, some of the tasks that are mapped to CPU trigger the tasks that are mapped to GPU.

In this case, the execution or response time can be different according to the mechanism.

.. figure:: /_images/offloading.png 
	:width: 400
	:align: center

* **Synchronous**

The triggering task triggers its target GPU task when it reaches `InterProcessTrigger` and waits until it gets the response from the triggered GPU task. Then it finishes the remaining job.

* **Asynchronous**

The triggering task triggers its target GPU task when it reaches `InterProcessTrigger` and does not wait for the response from the triggered GPU task and finishes the remaining job. 

The asynchronous methodology described here can be modified according to the user's interpretation.

This concept is used in two of the four execution cases introduced by a method, :ref:`method-task-execution-time`.

|
|

.. _response-time:

**3. Response Time**
====================

The response time analysis approach implemented here is not only designed for Multi-core Systems but also for Heterogeneous Systems.

Basically, the classic response time analysis equation is used.

:math:`R_i = C_i + \sum_{j \in hp(i)} \lceil \frac {R_i} {T_j} \rceil C_j`

The equation is based on RMS (Rate Monotonic Scheduling) which means the static priorities are assigned according to the period of the task, so a task with the shorter period results in a higher task priority.

Here, R_i denotes the response time of a task with i-th priority in the set of tasks where hp(i) is the set of tasks with priority higher than task i.

To find relevant methods, see :ref:`method-response-time-sum`.

|
|
|

.. _e2e-latency:

**End to End Latency**
**********************

The approach & equations used here are referenced from a yet-unpublished paper, "Model-based Task Chain Latency and Blocking Analysis for Automotive Software" by the same author who published `CPU-GPU Response Time and Mapping Analysis for High-Performance Automotive Systems <https://www.ecrts.org/forum/viewtopic.php?f=43&t=134&sid=777ff03160a9434451d721748c8a8aea#p264>`_.

|
|

.. _task-chain-reaction:

**1. Task Chain Reaction**
==========================

The time between the task chain's first task release to the earliest task response of the last task in the chain.

|

.. _task-chain-reaction-implicit:

**Task Chain Reaction (Implicit)**
----------------------------------

* **Best-case Task-Chain Reaction (Implicit Communication Paradigm)**

:math:`\delta_{\gamma,\rho,\iota} ^-=\sum_j R_{j}^- \text{ with } \tau_j \in \gamma`

The best-case task chain reaction latency for implicit communication can be calculated by considering the sum of all task's best case response times within task chain.

Here, :math:`\gamma` refers to a task chain, :math:`\rho` corresponds the reaction latency, and :math:`\iota` outlines that this latency considers the implicit communication paradigm.

* **Worst-case Task-Chain Reaction (Implicit Communication Paradigm)**

:math:`\delta_{\gamma,\rho,\iota}^+ = \sum_{j=0}^{j=|\gamma|-2} \left(2\cdot T_{j}\right) +R_{j = |\gamma|-1}^+ \text{ with } \tau_j \in \gamma`

To find relevant methods, see :ref:`method-task-chain-reaction-implicit`.

|

.. _task-chain-reaction-let:

**Task Chain Reaction (LET)**
-----------------------------

* **Best-case Task-Chain Reaction (Logical Execution Time)**

:math:`\delta_{\gamma,\rho,\lambda} ^- = \sum_j T_{j} \text{ with } \tau_j \in \gamma`

The best-case task chain reaction latency for LET communication is the sum of all task's periods within task chain :math:`\gamma`.

* **Worst-case Task-Chain Reaction (Logical Execution Time)**

:math:`\delta_{\gamma,\rho, \lambda}^+= T_{j=0}+\sum_{j=1}^{j=|\gamma|-1} \left(2\cdot T_{j}\right) \text{ with } \tau_j \in \gamma`

To find relevant methods, see :ref:`method-task-chain-reaction-let`.

|
|

.. _task-chain-age:

**2. Task Chain Age**
=====================

The time a task chain result is initially available until the next task chain instance's initial results are available.

A task chain age latency equals the chain's last (response) task age latency, i.e. :math:`\delta_{\gamma,\alpha} = \delta_{i,\alpha}` with :math:`\tau_i` being the last task of the task chain :math:`\gamma`, i.e. :math:`i=|\gamma|-1`.

* **Best-case Task-Chain Age**

:math:`\delta_{i, \alpha}^- = T_i - R_i^+ + R_i^-`

* **Worst-case Task-Chain Age**

:math:`\delta_{i,\alpha}^+ = 2 \cdot T_i - R_i^- - (T_i - R_i^+) = T_i - R_i^- + R_i^+`

To find relevant methods, see :ref:`method-task-chain-age`.

|
|

.. _reaction-update:

**3. Reaction Update**
======================

Due to the fact that tasks can have varying periods across the task chain, propagation between task chain entities can be over or under sampled such that a task X's result (a) serves as an input for several subsequent task chain entity instances or (b) does not serve as an input at all due to the fact that the subsequent task can already work with newer results produced by X's next instance.

|

.. _early-reaction:

**Early Reaction**
------------------

:math:`\delta_{\gamma, \rho 0, \iota}^+ = R_{\gamma0} + \sum_{j=0}^{j = |\gamma|-2} T_{j+1} + \min(T_{j+1}, \epsilon_j + R_{j+1})`

:math:`\epsilon_j = 2\cdot T_{j} - R_{j} - T_{j+1} - \epsilon_{j-1}`

:math:`\epsilon_{-1} = 0`

To find relevant methods, see :ref:`method-task-chain-early-reaction`.

|

.. _reaction-update-equation:

**Reaction Update**
-------------------

Accordingly, the reaction update is the subtraction of two consecutive task chains instances best case early reaction and worst case early reaction.

:math:`\delta_{\gamma, \upsilon, \iota}^+ = \max_{k} \left(T_{j=0} + \delta_{\gamma, \rho 0, \iota, k+1}^+ - \delta_{\gamma, \rho , \iota, k}^- \right)`

|
|

.. _data-age:

**4. Data Age**
===============

It describes the longest time some data version persists in memory. 
This is independent of task chains and simply depends on the period of entities writing a particular label (i.e. data).

* **Best-case Data Age**

:math:`\delta_{l,\alpha}^+ = \min_i \delta_{i,\alpha}^+` 
with :math:`\tau_i` being any task that accesses label :math:`l`.

* **Worst-case Data Age**

:math:`\delta_{l,\alpha}^- = \min_i \delta_{i,\alpha}^- %R_i^- + (T_i - R_i^+)` 
with :math:`\tau_i` being any task that accesses label :math:`l`.

To find relevant methods, see :ref:`method-data-age`.

|
|
|
|

**Class Tree with Implemented Methods**
#######################################

.. image:: /_images/Class_Diagram.png
	:width: 800
	:alt: Class Diagram

The above class diagram describes the entire project in a hierarchical way.

|
|
|

**Key Classes**
***************

|
|

**1. E2ELatency**
=================
The top layer, it takes care of End-to-End latency of the observed task-chain based on the analyzed response time from CPURta.

Being responsible for calculating E2E latency according to the concepts stated in the theory part (e.g., Reaction, Age).

|

.. _method-task-chain-reaction-implicit:

**Task Chain Reaction (Implicit Communication Paradigm)**
---------------------------------------------------------

.. code-block:: java

	public Time getImplicitReactionBC(final EventChain ec, final CPURta cpurta)

This method derives the given event-chain's best-case end-to-end latency based on the reaction concept in Implicit communication.

.. code-block:: java

	public Time getImplicitReactionWC(final EventChain ec, final CPURta cpurta)

This method derives the given event-chain's worst-case end-to-end latency based on the reaction concept in Implicit communication.

For the details, see :ref:`task-chain-reaction-implicit`

|

.. _method-task-chain-reaction-let:

**Task Chain Reaction (Logical Execution Time Communication Paradigm)**
-----------------------------------------------------------------------

.. code-block:: java

	public Time getLetReactionBC(final EventChain ec, final CPURta cpurta)

This method derives the given event-chain's best-case end-to-end latency based on the reaction concept in LET communication.

.. code-block:: java

	public Time getLetReactionWC(final EventChain ec, final CPURta cpurta)

This method derives the given event-chain's worst-case end-to-end latency based on the reaction concept in LET communication.

|

.. _method-task-chain-age:

**Task Chain Age**
------------------

.. code-block:: java

	public Time getTaskChainAge(final EventChain ec, final TimeType executionCase, final CPURta cpurta)

This method derives the given event-chain latency based on the age concept.

By changing `TimeType executionCase` parameter, the latency in the best-case or the worst-case can be derived.

For the details, see :ref:`task-chain-age`

|

.. _method-task-chain-early-reaction:

**Task Chain Early Reaction**
-----------------------------

.. code-block:: java

	public Time getEarlyReaction(final EventChain ec, final TimeType executionCase, final CPURta cpurta)

This is a method to be pre-executed for getting the reaction-update latency. 

The best-case and worst-case early-reaction latency values should be derived first and then the reaction update latency can be calculated.

By changing `TimeType executionCase` parameter, the latency in the best-case or the worst-case can be derived.

For the details, see :ref:`early-reaction`

|

.. _method-data-age:

**Data Age**
------------

.. code-block:: java

	public Time getDataAge(final Label label, final EventChain ec, final TimeType executionCase, final CPURta cpurta)

This method derives the given label's age latency.

If the passed event-chain does not contain the observed label, `null` value is returned.

By changing `TimeType executionCase` parameter, the latency in the best-case or the worst-case can be derived.

For the details, see :ref:`data-age`

|
|

**2. CPURta**
=============
The middle layer, it takes care of analyzing task response time.

Being responsible for calculating response time according to the communication paradigm (Direct or Implicit communication paradigm). 

|

.. _method-response-time-sum:

**Response Time Sum**
---------------------

.. code-block:: java

	public Time getCPUResponseTimeSum(final TimeType executionCase)

This method derives the sum of all the tasks' response times according to the given mapping model (which described as an integer array).

It is designed for Generic Algorithm mapping so that GA would filter out all mapping models with a relatively longer RT sum value and take the shortest one which is the optimized mapping model.

|

.. _method-response-time-direct:

**Response Time (Direct Communication Paradigm)**
-------------------------------------------------

.. code-block:: java

	public Time preciseTestCPURT(final Task task, final List<Task> taskList, final TimeType executionCase, final ProcessingUnit pu)

This method derives response time of the observed task according to the classic response time equation.

The response time can be different depending upon the passed taskList which is decided according to the mapping model.

Here, we are getting response time with RMS (Rate Monotonic Scheduling).
It means that a task with the shorter period take the higher priority and vice versa.

So before the taskList is passed to the method, it should be sorted in the order of shortest to longest and this job is done by `taskSorting(List<Task> taskList)` which is a private method.

|

.. _method-response-time-implicit:

**Response Time (Implicit Communication Paradigm)**
---------------------------------------------------

.. code-block:: java

	public Time implicitPreciseTest(final Task task, final List<Task> taskList, final TimeType executionCase, final ProcessingUnit pu, final CPURta cpurta)

This method derives response time of the observed task according to the classic response time equation but in the implicit communication paradigm.

In the implicit communication paradigm which is introduced by AUTOSAR, a task copies in its required data (labels) to its local memory in the beginning of execution, computes in the local memory and finally copies out the result to the shared memory.

Due to these copy-in & copy-out costs, extra time should be added up to the task's execution time and this is done by `getLocalCopyTimeArray` (for the details, see :ref:`method-local-copy-implicit`) which is a method from `RTARuntimeUtil` class.

As a result, the task's execution time gets longer but its period should be the same as before.

Once the local-copy cost is taken into account, the remaining process is the same as :ref:`method-response-time-direct`

For the details, see :ref:`response-time`

|
|

**3. RTARuntimeUtil**
=====================
The botton layer, it takes care of task & runnable execution time. Being responsible for calculating memory access cost, ticks (a.k.a execution need) computation time.

|

.. _method-task-execution-time:

**CPU Task Execution Time**
---------------------------

.. code-block:: java

	public Time getExecutionTimeforCPUTask(final Task task, final ProcessingUnit pu, final TimeType executionCase, final CPURta cpurta)

This method derives execution time of the observed task under one of the four following cases:

* CPU task that triggers GPU task in the synchronous offloading mode.

* CPU task that triggers GPU task in the asynchronous offloading mode.

(For the details, see :ref:`offloading-mechanism`)

* GPU task which is mapped to CPU

Execution time of the given task which was originally designed for GPU but mapped to CPU by GA(Generic Algorithm) Mapping.

It should ignore offloading runnables and take the required labels(read from pre-processing, write from post-processing) into account.

For example, here is a task named SFM which is originally a GPU Task.

.. figure:: /_images/GPUTask_SFM.png 
	:align: center

Since the task is newly mapped to CPU, the offloading runnables (`SFM_host_to_device`, `SFM_device_to_host`) which are in charge of offloading workload to GPU and copying back to CPU are not needed anymore.

.. figure:: /_images/offloading.png 
	:align: center

Instead, the labels from runnables before (`Pre-processing`) & after (`Post-processing`) the `InterProcessTrigger` are considered.

For the runnable, `Pre-processing`, read labels & read latency value are taken into account.

For the runnable, `Post-processing`, write labels & write latency value are taken into account.

This job is done by a private method, `getExecutionTimeForGPUTaskOnCPU()`.

* Task with only Ticks (pure computation)

When a CPU task without any triggering behavior is passed, only execution time that corresponds to the task's Ticks would be calculated.

|

Except for the very last case (Task with only Ticks), the task execution time calculation always involves with memory accessing cost.

Calculating memory accessing cost is taken care by methods such as `getExecutionTimeForRTARunnable`, `getRunnableMemoryAccessTime` which are defined as private.

For the details, see :ref:`memory-accessing-cost`.

|

.. _method-local-copy-implicit:

**Local Copy Cost for the Implicit Communication Paradigm**
-----------------------------------------------------------

.. code-block:: java

	public Time[] getLocalCopyTimeArray(final Task task, final ProcessingUnit pu, final TimeType executionCase, final CPURta cpurta)



referenced paper

`End-To-End Latency Characterization of Implicit and LET Communication Models <https://www.ecrts.org/forum/viewtopic.php?f=32&t=91>`_ by Jorge Martinez



referenced equation

:math:`C_{i}^0, C_{i}^last = \sum_{l \in I_i} \xi_l (x)`



|
|
|


**Supplementary Classes (Out of scope)**
****************************************

|
|

**1. SharedConsts**
===================

|
|

**2. CommonUtils**
==================

.. code-block:: java

	public static List<ProcessingUnit> getPUs(final Amalthea amalthea)

|

.. code-block:: java

	public static Time getStimInTime(final Task t)

|
|

**3. Contention**
=================

.. code-block:: java

	public Time contentionForTask(final Task task)

|
|
|
|

**APP4RTA User Interface**
##########################

|
|
|
|

**Git Repository**
##################


`yahoo <http://yahoo.com>`_
.. https://www.ecrts.org/forum/viewtopic.php?f=43&t=134&sid=777ff03160a9434451d721748c8a8aea#p264