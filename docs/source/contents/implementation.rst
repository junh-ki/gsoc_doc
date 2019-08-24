**Class Tree with Implemented Methods**
#######################################

.. image:: /_images/Class_Diagram.png
	:width: 800
	:alt: Class Diagram

The above UML class diagram describes the project's implementation in a hierarchical way.

|

**Key Classes**
***************

|

**E2ELatency**
==============
The top layer takes care of the end-to-end latency calculation of the observed task-chain based on the analyzed response time from the CPURta class.
It includes calculating E2E latency values according to the concepts stated in the theory part (e.g., Reaction, Age).

|

.. _method-task-chain-reaction-implicit:

**Task Chain Reaction (Implicit Communication Paradigm)**
---------------------------------------------------------

.. code-block:: java

	public Time getTCReactionBC(final EventChain ec, final ComParadigm paradigm, final CPURta cpurta)

This method derives the given event-chain's best-case end-to-end latency based on the reaction concept for the direct and implicit communication paradigms.

`Code Reference <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/E2ELatency.java?h=gsoc19RTAFinal#n147>`_

.. code-block:: java

	public Time getTCReactionWC(final EventChain ec, final ComParadigm paradigm, final CPURta cpurta)

This method derives the given event-chain's worst-case end-to-end latency value based on the reaction concept for the direct and implicit communication paradigms.

`Code Reference <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/E2ELatency.java?h=gsoc19RTAFinal#n196>`_

For the details, see :ref:`task-chain-reaction-implicit` and :ref:`features-e2elatency` (UI reference).

|

.. _method-task-chain-reaction-let:

**Task Chain Reaction (Logical Execution Time Communication Paradigm)**
-----------------------------------------------------------------------

.. code-block:: java

	public Time getLetReactionBC(final EventChain ec, final CPURta cpurta)

This method derives the given event-chain's best-case end-to-end latency value based on the reaction concept for LET communication.

`Code Reference <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/E2ELatency.java?h=gsoc19RTAFinal#n246>`_

.. code-block:: java

	public Time getLetReactionWC(final EventChain ec, final CPURta cpurta)

This method derives the given event-chain's worst-case end-to-end latency based on the reaction concept for LET communication.

`Code Reference <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/E2ELatency.java?h=gsoc19RTAFinal#n274>`_

For the details, see :ref:`task-chain-reaction-let` and :ref:`features-e2elatency` (UI reference).

|

.. _method-task-chain-age:

**Task Chain Age**
------------------

.. code-block:: java

	public Time getTaskChainAge(final EventChain ec, final TimeType executionCase, final ComParadigm paradigm, final CPURta cpurta)

This method derives the given event-chain latency based on the age concept.
By changing `TimeType executionCase` parameter, the latency in the best-case or the worst-case can be derived.

`Code Reference <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/E2ELatency.java?h=gsoc19RTAFinal#n304>`_

For the details, see :ref:`task-chain-age` and :ref:`features-e2elatency` (UI reference).

|

.. _method-task-chain-early-reaction:

**Task Chain Early Reaction**
-----------------------------

.. code-block:: java

	public Time getEarlyReaction(final EventChain ec, final TimeType executionCase, final ComParadigm paradigm, final CPURta cpurta)

This is a method to be pre-executed for getting the reaction-update latency values. 
The best-case and worst-case early-reaction latency values should be derived first and then the reaction update latency can be calculated.
By changing `TimeType executionCase` parameter, the latency in the best-case or the worst-case can be derived.

`Code Reference <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/E2ELatency.java?h=gsoc19RTAFinal#n366>`_

For the details, see :ref:`early-reaction` and :ref:`features-e2elatency` (UI reference).

|

.. _method-data-age:

**Data Age**
------------

.. code-block:: java

	public Time getDataAge(final Label label, final EventChain ec, final TimeType executionCase, final ComParadigm paradigm, final CPURta cpurta)

This method derives the given label's age latency.
If the passed event-chain does not contain the observed label, `null` is returned.
By changing `TimeType executionCase` parameter, the latency in the best-case or the worst-case can be derived.

`Code Reference <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/E2ELatency.java?h=gsoc19RTAFinal#n467>`_

For the details, see :ref:`data-age` and :ref:`features-e2elatency` (UI reference).

|

**CPURta**
==========

The middle layer takes care of analyzing task response times.
It is responsible for calculating response times according to the communication paradigm (Direct or Implicit communication paradigm). 

|

.. _method-response-time-sum:

**Response Time Sum**
---------------------

.. code-block:: java

	public Time getCPUResponseTimeSum(final TimeType executionCase)

This method derives the sum of all the tasks' response times according to the given mapping model (which is described as an integer array).
The method can be used as a metric to assess a mapping model.

`Code Reference <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/CPURta.java?h=gsoc19RTAFinal#n411>`_

|

.. _method-response-time-direct:

**Response Time (Direct Communication Paradigm)**
-------------------------------------------------

.. code-block:: java

	public Time preciseTestCPURT(final Task task, final List<Task> taskList, final TimeType executionCase, final ProcessingUnit pu)

This method derives the response time of the observed task according to the classic response time equation.
The response time can be different depending on the passed taskList which is derived from the mapping model.
Here, we are concerning response time for RMS (Rate Monotonic Scheduling).
It means that a task with the shorter period obtains a higher priority.
Before the taskList is passed to the method, it should be sorted in the order of shortest to longest and this job is done by `taskSorting(List<Task> taskList)` which is a private method.

`Code Reference <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/CPURta.java?h=gsoc19RTAFinal#n502>`_

|

.. _method-response-time-implicit:

**Response Time (Implicit Communication Paradigm)**
---------------------------------------------------

.. code-block:: java

	public Time implicitPreciseTest(final Task task, final List<Task> taskList, final TimeType executionCase, final ProcessingUnit pu, final CPURta cpurta)

This method derives the response time of the task parameter according to the classic response time equation but in the implicit communication paradigm.
In the implicit communication paradigm which is introduced by AUTOSAR. A task copies in its required data (labels) to its local memory at the beginning of its execution, computes in the local memory and finally copies out the result to the shared memory.
Due to these copy-in & copy-out costs, extra time must be added to the task's execution time which is done by `getLocalCopyTimeArray` (for the details, see :ref:`method-local-copy-implicit`) which is a method from the `RTARuntimeUtil` class.
As a result, the task's execution time gets longer while its period should stays the same.
Once the local-copy cost is taken into account, the remaining process is the same as :ref:`method-response-time-direct`

`Code Reference <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/CPURta.java?h=gsoc19RTAFinal#n618>`_

For the details, see :ref:`response-time` and :ref:`features-rta` (UI reference).

|

**RTARuntimeUtil**
==================
The bottom layer takes care of task and runnable execution time. It is responsible for calculating memory access costs, execution ticks or execution needs, and computation time.

|

.. _method-task-execution-time:

**CPU Task Execution Time**
---------------------------

.. code-block:: java

	public Time getExecutionTimeforCPUTask(final Task task, final ProcessingUnit pu, final TimeType executionCase, final CPURta cpurta)

This method derives the execution time of the task parameter under one of the  following cases:

* The CPU task triggers a GPU task in the synchronous offloading mode

* The CPU task triggers a GPU task in the asynchronous offloading mode

(For the details, see :ref:`offloading-mechanism`.)

* The GPU task is mapped to a CPU

According to the WATERS challenge, a triggering task (`PRE_..._POST`) can be ignored if the triggered task is mapped to a CPU.

For example, the following Figure shows the `SFM` task which is mapped to the GPU by default.

.. image:: /_images/GPUTask_SFM.PNG 
	:align: center

If the task is mapped to CPU, the offloading runnables (`SFM_host_to_device`, `SFM_device_to_host`) which are in charge of offloading workload to GPU and copying back to CPU are obsolete.

.. image:: /_images/offloading.PNG 
	:align: center

Instead, the labels from runnables before (`Pre-processing`) & after (`Post-processing`) the `InterProcessTrigger` are considered.
For the runnable, `Pre-processing`, read labels and read latency values are taken into account.
For the runnable, `Post-processing`, write labels and write latency values are taken into account.
This job is done by the private method `getExecutionTimeForGPUTaskOnCPU()`.

* Task with only Ticks (pure computation)

When a CPU task without any triggering behavior is passed, only the execution time that corresponds to the task's ticks is considered.

`Code Reference for getExecutionTimeforCPUTask <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/RTARuntimeUtil.java?h=gsoc19RTAFinal#n55>`_

Except for the very last case (Task with only Ticks), the task execution time calculation always includes memory accessing costs.
Calculating memory accessing costs is taken care of by methods such as `getExecutionTimeForRTARunnable`, `getRunnableMemoryAccessTime` which are defined as private.

`Code Reference for getExecutionTimeForRTARunnable <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/RTARuntimeUtil.java?h=gsoc19RTAFinal#n335>`_
`Code Reference for getRunnableMemoryAccessTime <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/RTARuntimeUtil.java?h=gsoc19RTAFinal#n414>`_

For the details, see :ref:`memory-accessing-cost`.

|

.. _method-local-copy-implicit:

**Local Copy Cost for the Implicit Communication Paradigm**
-----------------------------------------------------------

.. code-block:: java

	public Time[] getLocalCopyTimeArray(final Task task, final ProcessingUnit pu, final TimeType executionCase, final CPURta cpurta)

As it is introduced in :ref:`method-response-time-implicit`, label copy-in and copy-out costs should be calculated and added to the total execution time of the target task.

The following equation from `End-To-End Latency Characterization of Implicit and LET Communication Models <https://www.ecrts.org/forum/viewtopic.php?f=32&t=91>`_ is used to calculate these costs.

:math:`C_{i}^0 = \sum_{l \in I_i} \xi_l (x)`

Where :math:`C_{i}^0` denotes the execution time of the runnable `\tau_0`, :math:`I_i` represents the inputs (read labels) of the considered task and :math:`\xi_l (x)` denotes the time it takes to access a shared label :math:`l` from memory :math:`x`.

:math:`C_{i}^last = \sum_{l \in O_i} \xi_l (x)`

Where :math:`C_{i}^last` denotes the execution time of the runnable `\tau_last`, :math:`O_i` represents the outputs (write labels) of the considered task and :math:`\xi_l (x)` denotes the time it takes to access a shared label :math:`l` from memory :math:`x`.

For the copy-in cost, only read labels should be taken into account.
The copy-in cost time is stored on index 0 of the return array.
This will later be considered as the execution time of the copy-in runnable which is added to the beginning of the task execution.

For the copy-in cost, only write labels should be taken into account.
The copy-in cost time is stored on index 1 of the return array.
This will later be considered as the execution time of the copy-out runnable which is added to the end of the task execution.

`Code Reference <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/RTARuntimeUtil.java?h=gsoc19RTAFinal#n474>`_

|

**Supplementary Classes (Out of scope)**
****************************************

|

**SharedConsts**
================

This class is in charge of setting configuration variables.
The user can set the offloading mechanism and the execution case (WC, AC, BC) by changing `synchronousOffloading` and `timeType` respectively.
Also, all file paths for every Amalthea model can be saved as `String` type constants here so that the user can change the target Amalthea model by switching these constants.

|

**CommonUtils**
===============

.. code-block:: java

	public static List<ProcessingUnit> getPUs(final Amalthea amalthea)

This method derives a list of processing units of the target `Amalthea` model. 
It places CPU type processing units in the front and that of GPU type in the tail (end) of the list.

`Code Reference <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/CommonUtils.java#n75>`_

|

.. code-block:: java

	public static Time getStimInTime(final Task t)

This method returns the periodic recurrence time of the target task.
If the passed task is not a periodic task (e.g., GPU task), the recurrence time of a task which is periodic and triggers the target task is returned.
Otherwise time 0 is returned.

`Code Reference <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/CommonUtils.java#n452>`_

|

**Contention**
==============

.. code-block:: java

	public Time contentionForTask(final Task task)

This method derives a memory contention time which represents the delay when more than one CPU core and/or the GPU is accessing memory at the same time.

`Code Reference <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.tools.git/tree/eclipse-tools/responseTime-analyzer/plugins/org.eclipse.app4mc.gsoc_rta/src/org/eclipse/app4mc/gsoc_rta/Contention.java#n152>`_

For the details, see `Memory Contention Model <https://www.ecrts.org/forum/viewtopic.php?f=43&t=125&sid=0d17da7eba5419d1dc41d6d81dace278>`_.
