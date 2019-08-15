Intention
=========

The current APP4MC library provides several methods for getting execution time for a task, a runnable or ticks (pure computation) through the util package. 

However, libraries for response time analysis do not exist yet. The reason why is that response time analysis can be varied depending on the analyzed model that it is hard to be generalized. 

Since the trends are evolving from homogeneous to heterogeneous platform, the analysis methodology have become much more sophisticated that it is necessary to have a generic CPU response time analysis which can be used for different mapping models with different types of processing unit (e.g., GPU). 

The project also aims to offer End-to-End Latency analysis with some newly defined concepts such as Reaction & Age. This is intended to help users to analyze how much time would be taken for some data to be propagated from the beginning to the end of a given chain of tasks. 
