Contribution & Benefits for The Community
=========================================

In this project, `a standardized response time analysis methodology <https://academic.oup.com/comjnl/article/29/5/390/486162>`_ (Mathai Joseph and Paritosh Pandya, 1986) is used. Not only this, but also a class, `CpuRTA` which is designed for Generic Algorithm Mapping is provided. 

Since a heterogeneous platfrom requires a different analysis methodology for a processing unit which is not a CPU, a class that has a built-in response-time calculation algorithm which can be used with GA Mapping would be very helpful and make the entire developing circle quicker. 

Another class, `RuntimeUtilRTA` supports `CpuRTA` class by providing several ways to calculate execution time of a task. The methodology for deriving execution time changes depending upon the execution case (e.g., Worst Case, Best Case, Average Case), the transmission type (e.g., Synchronous, Asynchronous) or the different mapping model. 

This class can be modified & reused for other analysis models simply by adjusting a method which takes care of memory accessing time (because memory accessing time is different according to the target hardware).

Not only this, a visually described mapping model with information about schedulability, the corresponding response time for each task and E2E latency analysis according to each task-chain model are provided through an User Interface window.
