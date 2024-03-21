
# Table of Contents

1. [Introduction](#introduction)
2. [Tasks](#tasks)
3. [Performance](#performance)
4. [Analysis](#analysis)

# Introduction
This report provides detailed analysis of the embedded system, which is designed to manage real-time audio processing and user interaction. It outlines task implementation, performance evaluation, and critically analyses the effectiveness of the system design. The system aims to achieve responsive and deterministic behaviour using techniques such as scheduling, interrupt handling, and data synchronisation.

# Tasks
## scanKeysTask()
The 'scanKeysTask()' is a crucial component responsible for scanning input keys and updating the state of the system according to detected inputs. It operates as a thread within the FreeRTOS environment which allows it to run concurrently with other tasks. After its initiation, it iterates through each row of the key matrix and reads the state of the columns to determine which keys are pressed. The state of the system is then updated.     
Theoretical minimum initiation -     
Measured maximum execution time - 

## displayUpdateTask()
This task updates the OLED display with the latest state information of the system including octaves, volume, keys and input hex. It is a thread based task and operates independently within the FreeRTOS schedular. Once executed,  it retrieves relevant system parameters and formats them for display. This task plays a vital role in providing user feedback and ensuring a seamless interaction experience.           
Method- Thread 
Theoretical minimum initiation -     
Measured maximum execution time -  

## CAN_RX_ISR()
This handles the interrupt service routine (IRS) for receiving CAN messages. As an interrupt-based method, it operates asynchronously in response to external events. When a CAN message is received, the ISR processes the message and queues it for further decoding by the system.             
Method- Interrupt     
Theoretical minimum initiation -     
Measured maximum execution time -    

## decodeTask()
This decodes the received CAN messages and updates system variables accordingly. Operating as a thread within the FreeRTOS environment, it continuously monitors the message queue for incoming data. Upon receiving a message, it interprets its content and adjusts system parameters as necessary.          
Method- Thread     
Theoretical minimum initiation -     
Measured maximum execution time -    

## CAN_TX_Task()
This transmits CAN messages containing state information. Similar to other thread-based tasks, it operates within the FreeRTOS scheduler, periodically sending updates over the CAN bus.             
Method- Thread     
Theoretical minimum initiation -      
Measured maximum execution time -     

# Performance
## Total CPU Utilisation

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