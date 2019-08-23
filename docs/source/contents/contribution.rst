Contribution & Benefits for The Community
=========================================

In this project, `a standardized response time analysis methodology <https://academic.oup.com/comjnl/article/29/5/390/486162>`_ (Mathai Joseph and Paritosh Pandya, 1986) is used. Not only this, but a class, `CPURta` which can be used with various implementations (e.g. a Genetic Mapping Algorithm), is also provided.

Since a heterogeneous platform requires different analysis methodologies for processing units, a class that has a built-in response-time calculation algorithm is very helpful and makes the entire developing circle quicker. 

Another class, `RTARuntimeUtil` supports the `CPURta` class by providing several ways to calculate the execution time of a task. The methodology for deriving execution time changes depending on the execution case (e.g., Worst Case, Best Case, Average Case), the offloading mechanism (e.g., Synchronous, Asynchronous), and the  mapping model. 
This class can be modified and reused for other models under analysis simply by adjusting a single method which takes care of memory accessing time (because memory accessing time can be different according to the target hardware). 

Furthermore, this GSoC project provides a small GUI implementation, which visually describes the mapping model with information about schedulability, the corresponding response times for each task, and E2E latency analysis results (`E2ELatency`) according to each task chain.
