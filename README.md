
# Table of Contents

1. [Introduction](#introduction)
2. [Tasks](#tasks)
3. [Performance](#performance)
4. [Analysis](#analysis)

# Introduction
This report provides detailed analysis of the system and outlines task implementation, performance evaluation, and critical analysis. The system aims to achieve responsive and deterministic behaviour using techniques such as scheduling, interrupt handling, and data synchronisation.

# Tasks
## scanKeysTask()
This task is responsible for scanning input keys and updating the state of the system according to the result. 
Method- Thread    
Theoretical minimum initiation -     
Measured maximum execution time - 

## displayUpdateTask()
This task updates the OLED display with the latest state information of the system including octaves, volume, keys and input hex.     
Method- Thread 
Theoretical minimum initiation -     
Measured maximum execution time -  

## CAN_RX_ISR()
This handles the interrupt service routine for receiving CAN messages.    
Method- Interrupt     
Theoretical minimum initiation -     
Measured maximum execution time -    

## decodeTask()
This decodes the received CAN messages and updates system variables accordingly.    
Method- Thread     
Theoretical minimum initiation -     
Measured maximum execution time -    

## CAN_TX_Task()
This transmits CAN messages containing state information.     
Method- Thread     
Theoretical minimum initiation -      
Measured maximum execution time -     

# Performance
## Total CPU Utilisation

## Shared Data Structures and Synchronisation Methods
The system uses shared data structures to facilitate the communication and coordination between tasks.      
The ‘sysState’ structure is used to share system state information. ‘knobRotation’ is a shared variable tracking knob rotation.       Synchronisation is achieved using mutexes to ensure safe access to shared resources.    

# Analysis
## Critical Instance Analysis
This system demonstrates integration of hardware components and software functionalities to achieve real-time audio processing and user interaction. It utilises a STM32 microcontroller platform with FreeRTOS for task scheduling and efficiently manages concurrent tasks such as key scanning, display updating, and CAN message handling. Using interrupt service routines (ISRs) for CAN reception and audio sampling ensures timely response to external events, enhancing system responsiveness. The seamless coordination between tasks is achieved through the use of shared data structures and mutex as it enables inter-task communication and synchronisation. Overall, the system demonstrates embedded system development techniques, including multitasking, interupt handling, and real-time data processing smoothly.
## Inter-task Blocking Dependencies Analysis