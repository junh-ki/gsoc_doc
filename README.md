# GSOC_RTA

2019 Google Summer of Code (CPU-GPU Response Time and Mapping Analysis)

## [Table of Contents]
### 1. Milestone with the goal of each phase
### 2. Intention
### 3. Contribution & benefits for the community
### 4. Contents
### 5. Diagram Example
### 6. Instruction
### 7. Remarks
### 8. Updates (Phase 2: June/24 ~ July/21)

# 1. Milestone with the goal of each phase
- Response Time Analysis_CPU Part (Phase 1)
- **Refine Previous Phase & E2E Latency Foundation (EC, IC, LET) (Phase 2: June/24 ~ July/21)**
- Finalize LET, EC, IC and the corresponding UI part (Phase 3)

# 2. Intention
The current APP4MC library does provide several methods which are useful for deriving execution time for a task, a runnable or ticks (pure computation) through the Util package. But methods for response time are still not available. The reason is that response time analysis can be varied depending on the analyzed model so it is hard to be generalized. But since the trends are evolving from homogeneous to heterogeneous platform, the analysis methodology have become much more sophisticated so it is necessary to have CPU response time analysis which can be used for different mapping analysis with a different processing unit type (e.g., GPU).

# 3. Contribution & benefits for the community
In this project, [a standardized response time analysis methodology](https://www.semanticscholar.org/paper/Finding-Response-Times-in-a-Real-Time-System-Joseph-Pandya/574517d6e47cf9b368003a56088651a1941dcda1)(Mathai Joseph and Paritosh Pandya, 1986) which involves a complex algorithm is used. Not only this, but also a class, `CpuRTA` which is designed for Generic Algorithm Mapping is provided. Since a heterogeneous platfrom usually requires a different analysis methodology for a processing unit according to its type(e.g., CPU & GPU), a class that can be used with GA Mapping and has a built-in general analysis methodology would be very helpful and save a lot of time which otherwise would be spent for implementing the same algorithm for those tasks that are mapped to a particular type of processing units (e.g., CPU). Along with these, another class, `RuntimeUtilRTA` which supports `CpuRTA` class provides several ways to calculate execution time of a task is also provided. The execution time calculation methodology can be different depending on an execution case (e.g., Worst Case, Best Case, Average Case), a transmission type (e.g., Synchronous, Asynchronous) or a different mapping model. This class can be modified and reused for other analysis models if only a method which takes care of a Runnable execution time is adjusted.

# 4. Contents
Since the target of implementing heterogeneous platform is to achieve better performance and efficiency, just simply calculating response time is not enough. To realize the optimized response time analysis, different mapping analysis for the same given model according to Generic Algorithm should be taken into account. Generic Algorithm would map tasks to different processing units in the form of integer array so that the total sum of each task’s response time according to the each GA generation can be delivered and compared each other to come up with a better solution. For this reason, a public method which returns the total sum of each task’s response time and the relevant private methods that are used to support this method are needed. The corresponding methods are followed below.          
**Refer to javadoc below for more details.**          
* [CpuRTA](Add Ref here)
* [TimeCompIA](Add Ref here)
* [RuntimeUtilRTA](Add Ref here) 
* [RTApp](Add Ref here)
           
<`CpuRTA.java`>          
**getCPUResponseTimeSum**          
Calculate the total sum of response times of the tasks of the given Amalthea model with a GA mapping model          
**getTaskCPURT**          
Calculate response time of the given task of the given Amalthea model with a GA mapping model          
**taskSorting**          
Sort out the given list of tasks (in order of shorter period first - Rate Monotonic Scheduling)          
**preciseTestCPURT** (Response Time analysis Equation Explanation)          
Calculate response time of the observed task according to the periodic tasks response time analysis algorithm.          
> Ri = Ci + Σj ∈ HP(i) [Ri/Tj]*Cj ([a standardized response time analysis methodology](https://www.semanticscholar.org/paper/Finding-Response-Times-in-a-Real-Time-System-Joseph-Pandya/574517d6e47cf9b368003a56088651a1941dcda1)(Mathai Joseph and Paritosh Pandya, 1986))
           
<`RuntimeUtilRTA.java`>          
**getExecutionTimeforCPUTask**          
Calculate execution time of the given task under one of the several configurations.          
**doesThisTaskTriggerCPUTask**          
Find out whether the given triggering task(that has an InterProcessTrigger) triggers a GPU task which is newly mapped to CPU.          
**syncTypeOperation**          
Calculate execution time of the given runnableList in a synchronous manner.          
**asyncTypeOperation**          
Calculate execution time of the given runnableList in an asynchronous manner.         
**getExecutionTimeForGPUTaskOnCPU**          
Calculate execution time of the given task which was originally designed for GPU but newly mapped to CPU by Generic Algorithm Mapping.          
**getExecutionTimeForRTARunnable**          
Calculate execution time of the given runnable.          
**getTaskMemoryAccessTime**         
Calculate memory access time of the observed task.           
**getRunnableMemoryAccessTime**          
Calculate memory access time of the observed runnable.           
> (Explanation)         
> Read(Write)_Access_Time = Round_UP(Size_of_Read_Labels / 64.0 Bytes) * (Read_Latency / Frequency)        
**isTriggeringTask**         
Identify whether the given task has an InterProcessTrigger or not.          
         
<`RTApp.java`>          
User Interface Window           
[APP4RTA_1.0_Description](Add Ref here)('responseTime-analyzer'>'plugins'>'doc'>'APP4RTA_1.0_Description.pdf')         
            
# 5. Diagram Example           
![Class Diagram](Add Ref here)('responseTime-analyzer'>'plugins'>'doc')            
            
# 6. Instruction            
1. Under 'responseTime-analyzer'>'plugins'>'src'>...>'gsoc_rta' folder, there is 'CpuRTA' class. This is the implementation source file. By running them, one can derive the total sum of response times of the given model.            
2. Under 'responseTime-analyzer'>'plugins'>'src'>...>'gsoc_rta'>'ui' folder, there is 'RTApp_WATERS19' class. This is Java Swing UI source file that corresponds to the 'CpuRTA'. This UI is created based on WATERS19 Project. By running this, one may get more detailed visuals of the result of 'CpuRTA' class.            
   (Refer to 'APP4RTA_1.0_Description.pdf' for more details.)('responseTime-analyzer'>'plugins'>'doc'>'APP4RTA_1.0_Description.pdf')            
            
# 7. Remarks            
1. 'GPU Task on CPU' part has not been implemented yet, so when T10 – T13 Tasks are mapped to CPU, the result would be not accurate for now.            
	=> Done.            
            
# 8. Updates (Phase 2: June/24 ~ July/21)            
### July/16            
            
<`CpuRTA`>            
- In the previous phase, the CPU response time analysis had been done without considering the situation where GPU Tasks are mapped to CPU by the new integer array generation. This was rather inaccurate since a GPU Task contains offloading runnables which are used to copy-in and copy-out the local memory when it is mapped to GPU. Not only should these runnables be omitted, but also the labels from the triggering task should be taken into account for the GPU task that is newly mapped to CPU to access the specified memory. Therefore, a function "setGTCL(final Amalthea model)" that takes needed labels and save to a hashMap for each GPU Task has been made.            
            
<`RuntimeUtilRTA`>            
- getExecutionTimeForGPUTaskOnCPU method which only considers a GPU original task's associated labels and ticks but ignores its offloading runnables.            