******************************************************
(GSoC 2019) CPU-GPU Response Time and Mapping Analysis
******************************************************

Motivation
##########

In one of the subjects in my Master's course, I carried on a project analyzing metrics of Software Models and visualizing it in APP4MC. 

It was quite challenging as I was not very familiar with Amalthea model and APP4MC platform at first. But soon I was able to understand the concepts and started enjoying it. 

The project resulted in completing an application delivering performance metric and reliability metric of the given Software Model. Which is basically my motivation for participating this Eclipse GSoC topic, "CPU-GPU Response Time and Mapping Analysis".

Since this topic’s ultimate goal is to achieve systems' Real-Time determinism with HPC(High Performance Computing), analyzing Response Time is essential and I thought my basic knowledge of things like time constraints, End-to-End Event Chain Latencies according to the different communication paradigms (IC, LET) which I have gotten through the Master's study would be very helpful for this project. 

Now the industry's interest has been moving on to "Heterogeneous Systems", and I do hope that my GSoC project will be helpful for other researchers in this regard and make a contribution to the further development of the platform.

Best regards,
Ki, Junhyung

Contents
########

.. toctree::
   :maxdepth: 3
   
   Intention <contents/intention>   
   Contribution & Benefits for The Community <contents/contribution>
   Milestone with The Goal of Each Phase <contents/milestone>
   Implementation (APP4RTA) <contents/implementation>
   Future Work <contents/future>
   Reference <contents/reference>
   Contact <contents/contact>

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

Random Stuff
============

Here is some text explaining some very complicated stuff.::

	print 'hello'
	>> hello

If :math:`\sigma_{1}` equals :math:`\sigma_{2}` then etc, etc.

:math:`\underline{x}=[  x_{1}, ...,  x_{n}]^{T}`

:math:`R_i = C_i + \sum_{j \in hp(i)} \lceil \frac {R_i} {T_j} \rceil C_j`

Image Test 1)

.. image:: /_images/GSoC_image.png
	:width: 600
	:alt: GSoC Image
	:align: left

Do you like GSoC?


Image Test 2)

.. figure:: /_images/Polar_Express.jpg 
	:width: 600

Do you like Polar Express?


ReadTheDocs Image Test 

Open this :download:`example <_images/GSoC_image.png>` input file to see the following result:


.. _making-a-table:

Making a table
==============

This shows you how to make a table

==================   ============
Name                 Age
==================   ============
John D Hunter        40
Cast of Thousands    41
And Still More       42
==================   ============

.. _making-links:

Making links
============

It is easy to make a link to `yahoo <http://yahoo.com>`_

For example, see the module :mod:`matplotlib.backend_bases` documentation, or the
class :class:`~matplotlib.backend_bases.LocationEvent`, or the method
:meth:`~matplotlib.backend_bases.FigureCanvasBase.mpl_connect`.