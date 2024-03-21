
# Table of Contents

1. [Introduction](#introduction)
2. [Tasks](#tasks)
3. [Advanced Tasks](#advancedtasks)
4. [Performance](#performance)
5. [Analysis](#analysis)

# Introduction
This report provides detailed analysis of the embedded system, which is designed to manage real-time audio processing and user interaction. It outlines task implementation, performance evaluation, and critically analyses the effectiveness of the system design. The system aims to achieve responsive and deterministic behaviour using techniques such as scheduling, interrupt handling, and data synchronisation.

# Tasks
## scanKeysTask()
The 'scanKeysTask()' is a crucial component responsible for scanning input keys and updating the state of the system according to detected inputs. It operates as a thread within the FreeRTOS environment which allows it to run concurrently with other tasks. After its initiation, it iterates through each row of the key matrix and reads the state of the columns to determine which keys are pressed. The state of the system is then updated.     
Theoretical minimum initiation -  20.8 Microseconds   
Measured maximum execution time - 107 Microseconds

## displayUpdateTask()
This task updates the OLED display with the latest state information of the system including octaves, volume, keys and input hex. It is a thread based task and operates independently within the FreeRTOS schedular. Once executed,  it retrieves relevant system parameters and formats them for display. This task plays a vital role in providing user feedback and ensuring a seamless interaction experience.           
Method- Thread 
Theoretical minimum initiation - 2.78 Microseconds      
Measured maximum execution time - 17 Microseconds          

## CAN_RX_ISR()
This handles the interrupt service routine (IRS) for receiving CAN messages. As an interrupt-based method, it operates asynchronously in response to external events. When a CAN message is received, the ISR processes the message and queues it for further decoding by the system.             
Method- Interrupt     
Theoretical minimum initiation - 6.94 Microseconds   
Measured maximum execution time -    

## decodeTask()
This decodes the received CAN messages and updates system variables accordingly. Operating as a thread within the FreeRTOS environment, it continuously monitors the message queue for incoming data. Upon receiving a message, it interprets its content and adjusts system parameters as necessary.          
Method- Thread     
Theoretical minimum initiation - 13.89 Microseconds        
Measured maximum execution time - 11.7 Microseconds
   

## CAN_TX_Task()
This transmits CAN messages containing state information. Similar to other thread-based tasks, it operates within the FreeRTOS scheduler, periodically sending updates over the CAN bus.             
Method- Thread     
Theoretical minimum initiation -      
Measured maximum execution time -   

# Advanced Tasks
East and west detection for extra piano moudles have been implemented here. This allows the piano to determine if it is alone or conjoined. This is important as the initial value for the Can bus initialisation is false meaning a piano module connected to nothing will stall and crash. Before sending a message into the bus it first checks to see if an extra module is detected or not. If there is another it goes through and sends the message as it will recieve an acknowledgement, however if it is alone, it will not and thus does not send the message.

Also in the scan keys task is a method to determine which module acts as the receiver and which modules act as the senders. By clicking down on the third knob, the piano is set to a reciever and the variable ‘S’ is sent down the bus to every other module. As mentioned before if a module receives an S in the decode task its can initialisation is set to False resulting in it becoming a sender. This allows for it to send the notes being played to the receiver module and not reading and playing its own.

## SampleISR()
In SampleISR a method of polyphony has been implemented. By chasing the phase accumulator variable to a list, it can accumulate each individual note separately allowing for all 12 notes to be played simultaneously. For each index in the sysState.inputs variable it checks for a 0 and the corresponding value of the adjStepSizes list is accumulated in the corresponding index in the phase accumulator list. It then adjusts each value by the correct octave determined by the 2nd knob rotation. It generates a waveform through in Vout. Vout is then added to a variable mixed output, for each iteration of the for loop each waveform is added to mixed output generating a full waveform of every note which is being pressed down, finally it scales the waveform down depending on how many notes are being played. This is done by adding an active count which is increased for each active note being played, the final output is then divided by this to scale the volume down appropriately.






# Performance
## Total CPU Utilisation
The total CPU utilisation of the system plays a crucial role in determining its overall performance and responsiveness. It is influenced by the execution patterns of individual tasks, their priorities, and the efficiency of task scheduling mechanisms. By profiling the execution times of each task and analysing their respective CPU utilization rates, it is possible to gauge the system's overall workload distribution and identify potential bottlenecks. Optimizing task priorities, minimizing unnecessary CPU overhead, and adopting efficient scheduling strategies are essential measures to ensure optimal CPU utilisation. Furthermore, real-time operating systems like FreeRTOS offer features such as task prioritisation, preemption, and time slicing, enabling fine-grained control over CPU allocation and maximising system responsiveness. By monitoring and optimizing CPU utilisation, the system can achieve efficient resource utilisation, meet real-time deadlines, and deliver consistent performance across varying workloads.

## Shared Data Structures and Synchronisation Methods
The system uses shared data structures to facilitate the communication and coordination between tasks.      
The ‘sysState’ structure is used to share system state information. ‘knobRotation’ is a shared variable tracking knob rotation.       Synchronisation is achieved using mutexes to ensure safe access to shared resources. This approach minimises the risk of data corruption and race conditions, enhancing system reliability and stability.   

# Analysis
## Critical Instance Analysis
This system demonstrates integration of hardware components and software functionalities to achieve real-time audio processing and user interaction. It utilises a STM32 microcontroller platform with FreeRTOS for task scheduling and efficiently manages concurrent tasks such as key scanning, display updating, and CAN message handling. Using interrupt service routines (ISRs) for CAN reception and audio sampling ensures timely response to external events, enhancing system responsiveness. The seamless coordination between tasks is achieved through the use of shared data structures and mutex as it enables inter-task communication and synchronisation. Overall, the system demonstrates embedded system development techniques, including multitasking, interupt handling, and real-time data processing smoothly.

## Inter-task Blocking Dependencies Analysis

It is important to analyse inter-task blocking dependencies in order to identify potential bottlenecks and also to optimise task scheduling. There are certain scenarios where tasks may become blocked whilst waiting for access to shared resources or preemted higher priority tasks. This is due to the concurrent execution of multiple tasks that all have different priorities and requirements. Such blocking dependencies can lead to delays in critical operations and compromise the system's ability to meet real-time deadlines.

One concern is resource contention, particularly in tasks such as 'scanKeysTask()' and 'displayUpdateTask()', which rely on shared resources such as the sysState structure and mutexes. Without proper synchronisation mechanisms in place, simultaneous access to shared resources by multiple tasks may result in data corruption or race conditions, leading to unpredictable behavior.

Moreover, priority inversion poses a risk to system performance, whereby lower-priority tasks holding essential resources may inadvertently block higher-priority tasks. For instance, if a low-priority task acquires a mutex required by a high-priority task, the high-priority task may be delayed, resulting in priority inversion. Mitigating priority inversion requires the adoption of priority inheritance protocols, ensuring that tasks holding critical resources temporarily inherit the priority of the highest-priority task waiting for access.

Another concern is task preemption, introducing non-deterministic behavior and potential delays in critical task execution. Preemptive scheduling policies tailored to the system's real-time requirements can help mitigate the impact of task preemption on blocking dependencies. By prioritising time-sensitive tasks and minimising preemption-related delays, preemptive scheduling enhances system responsiveness and determinism which mitigates the risk of blocking-induced performance degradation.

In response to these challenges, it is imperative to optimise task priorities, employ real-time scheduling policies, profile and analyse blocking scenarios, and continuously monitor and tune system parameters. By identifying and addressing inter-task blocking dependencies proactively, the system can achieve enhanced responsiveness, reliability, and real-time performance, ensuring the seamless execution of critical tasks within the embedded environment.