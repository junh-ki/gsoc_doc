**Milestone with The Goal of Each Phase**
=========================================

|

**Phase 1 (May 27 - June 24)**
------------------------------

1. Structuring classes based on the abstraction layers (Top: End-to-End latency / Mid Layer: Response Time / Low Layer: Task & Runnable Execution Time) 

=====   ==============================
Layer   Responsibility
=====   ==============================
Top		End-to-End Latency
Mid		Task Response Time
Low		Task & Runnable Execution Time
=====   ==============================

2. Developing task and runnable level execution time methods with taking memory access cost and offloading mechanisms into account

3. Testing

4. Documenting

The main focus of phase 1 is to implement the basis framework and map each and every functionality to the classes. 
In this way, the entire system becomes organized which eases refactoring and debugging.

|

**Phase 2 (June 25 - July 22)**
-------------------------------

1. Developing interfaces between classes

2. Implementation of response time analysis algorithms according to different communication paradigms, i.e., direct and implicit communication)

3. Structuring and developing basic user interface class

4. Testing

5. Documenting

The main focus of phase 2 is to provide a stable response time method which can be used for several models under various configuration settings.

|

**Phase 3 (July 23 - August 19)**
---------------------------------

Refine Previous Phase and E2E Latency Foundation (IC, LET) / Documenting

1. Implementation of E2E latency analysis methodologies according to the concepts such as age, reaction, and propagation under different communication paradigms such as direct, implicit, and LET = Logical Execution Time.

2. Extend and finalize the UI part

3. Testing

4. Final documenting (Through Sphinx & readthedocs)

The main focus of phase 3 is to implement newly defined concepts of end-to-end latency methodologies in line with the implicit and LET communication paradigms.
As a consequence, users gain much more task chain metrics in addition to data propagation only.

Moreover, by using the provided GUI, user can investigate mapping scenarios and analyze response times & E2E latency metrics without diving into java implementations.
