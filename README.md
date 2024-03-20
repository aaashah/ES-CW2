## Introduction

This report provides detailed analysis of the system and outlines task implementation, performance evaluation, and critical analysis. The system aims to achieve responsive and deterministic behaviour using techniques such as scheduling, interrupt handling, and data synchronisation.

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


## Also include advanced tasks here!!


## Critical Instant Analysis

## Total CPU Utilisation

## Shared Data Structures and Synchronisation Methods

The system uses shared data structures to facilitate the communication and coordination between tasks. 
The ‘sysState’ structure is used to share system state information. ‘knobRotation’ is a shared variable tracking knob rotation. Synchronisation is achieved using mutexes to ensure safe access to shared resources.

## Inter-task Blocking Dependencies Analysis






