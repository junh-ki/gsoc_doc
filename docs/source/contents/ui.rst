**User Interface (APP4RTA)**
############################

.. _set-up:

**Set Up**
----------

For analyzing response time & end-to-end event-chain latency

.. image:: /_images/app4rta/main.png
	:width: 800
	:alt: main

Before executing the code, please install the Java GUI softwares.

* To install Java GUI softwares:
1. Eclipse > `Help`
2. `Install New Software` > Work with: Eclipse Repository (http://download.eclipse.org/releases/oxygen)
3. `General Purpose Tools` > all click from `Swing Designer` to `WindowBuilder XML Core (requires Eclipse WTP/WST)`
4. `Next` > `Next` > `accept` > `Finish`

.. _search-amalthea:

**Search Amalthea**
-------------------

.. image:: /_images/app4rta/0.png
	:width: 800
	:alt: 0

Run `APP4RTA.java` in `org.eclipse.app4mc.gsoc_rta.ui` package, then this window will show up.
Based on the horizontal line on the middle, the upper part is for response time & mapping analysis, and the lower part is for end-to-end event-chain latency analysis.
The first thing to do is deciding a target Amalthea model.

**1.** The window browser for searching Amalthea models shows up when the `Search Amalthea` button clicked.

.. _direct-select-amalthea:

**Direct & Select Amalthea**
----------------------------

.. image:: /_images/app4rta/1.png
	:width: 400
	:alt: 1

**2.** When the search browser shows up, direct to the path where the target Amalthea model file is located and select the model file.

**3.** Click the `Open` button.

.. _features-rta:

**UI Features (RTA)**
-----------------------------------------------

.. image:: /_images/app4rta/2.png
	:width: 600
	:alt: 2

Then the empty space will be filled with the the tasks and processing units of the selected model.
On the left-hand side, tasks' names with empty boxes can be found.
On the right-hand side, seven pairs of lists are seen (It means the selected model has seven processing units).
The list on the left side of each pair is for listing names of the tasks which are mapped to the corresponding processing unit while one on the right side is for listing response times of the corresponding tasks. 
Basically, we can map the tasks with these boxes by entering the number of each processing unit which is stated on the top of the lists on the left-side.

**4.** The user can either manually type numbers for every box or simply click the `Default IA` button which would automatically fill up every box with the pre-defined integer array values.

**5.** Once every `PU Num` box is filled, click `Enter IA` button to assign tasks to processing units according to each integer value. Once this is done, the mapped tasks would appear on the left-side lists.

**6.** Choose the offloading mode between `Synchronous` case and `Asynchronous` case.

**7.** Choose the execution case between `Worst` case and `Average` case and `Best` case.

**8.** By clicking the `Calculate` button, all calculation results will be printed out on the text-fields (`Schedulability`, `Cumulated Memory-Access Cost`, `Cumulated Contention`, `Computation`).

For the implementation details, see :ref:`CPURta-reference`.

.. _select-event-chain:

**Select an Event-Chain**
-------------------------

.. image:: /_images/app4rta/3.png
	:width: 800
	:alt: 3

The event-chain combo-box becomes visible once the user clicks `Enter IA` to assign tasks to processing units according to each integer value in the boxes.

**9.** To analyze end-to-end event-chain latency, an event-chain in the combo-box should be selected first.

.. _features-e2elatency:

**UI Features (E2ELatency)**
---------------------------------------------

.. image:: /_images/app4rta/4.png
	:width: 600
	:alt: 4

**10.** Select the communication paradigm between direct Communication and implicit communication.

**11.** Finally, click the `Calculate` button.

Then all calculation results regarding reaction, age of data, task-chain in the worst and best cases will be printed out to the corresponding text fields or lists.

For the implementation details, see :ref:`E2ELatency-reference`.

|

**Download** :download:`PDF <../contents/inst/app4rta_instruction.pdf>` file to see offline.
