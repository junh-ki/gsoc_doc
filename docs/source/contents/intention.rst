Intention
=========

The current APP4MC APIs provide several methods for getting execution time for a task, a runnable or ticks (pure computation) through the util package. 

However, APIs for response time analysis do not exist yet. The reason why is that response time analysis results highly vary depending on the analyzed model properties such as the scheduling, the mapping, and others. 

Since the trends are evolving from homogeneous to heterogeneous platforms, the analysis methodologies have become much more sophisticated. A generic form of CPU response time analysis, which can be used for different mapping models with different types of processing units (e.g., GPU), is though reasonable across modern analysis techniques.

Additionally, this project also aims to offer end-to-end event-chain latency analyses that incorporate a distinct concepts such as reaction & age which will be outlined in this documentation. Such analyses are intended to help users to analyze how much time would be taken for some data to be propagated from the beginning to the end of a given chain of tasks. 
