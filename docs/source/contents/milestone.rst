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

2. Developing task & runnable level execution time methods with taking memory access cost & offloading mechanism into account

3. Testing

4. Documenting

The main focus of phase 1 is to do the framework and map each and every functionality to the classes. 
In this way, the entire system becomes organized that makes it easier for error or bug tracking.

|

**Phase 2 (June 25 - July 22)**
-------------------------------

1. Developing interfaces between classes

2. Implementing response time analysis algorithm according to the communication paradigm (Direct Communcation or Implicit Communication)

3. Structuring & developing basic user interface class

4. Testing

5. Documenting

The main focus of phase 2 is to make a stable response time method which can be applicable for several cases that does not return error in every configuration setting.
If the method can handle every exception and error, it would make GA mapping process much more stable and smooth.

|

**Phase 3 (July 23 - August 19)**
---------------------------------

Refine Previous Phase & E2E Latency Foundation (IC, LET) / Documenting

1. Implementing E2E latency analysis methodologies according to the concepts such as age, reaction, etc., with communication paradigms (Direct, Implicit, Logical Execution Time)

2. Extend & finalize the UI part

3. Testing

4. Final documenting (Through Sphinx & readthedocs)

The main focus of phase 3 is to implement newly defined concepts of End-to-End latency mainly with the Implicit & LET communication paradigms.
This is intended to provide the user further information than just data propagation; task chain age & reaction in the observed event chain.
In that way, determinism metric of real-time systems can be improved.

Not only this, the user also get the better visual information through the user interface class.
This would enable users to map the Amalthea model and analyze response time & E2E latency information in a visual way.
