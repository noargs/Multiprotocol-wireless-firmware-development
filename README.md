## Multicore STM32, Thread, BLE, ZigBee firmware development     

<img src="images/pinout.png" alt="p-nucleo-wb55 pinout">         

<img src="images/block-diagram.png" alt="Block Diagram of STM32WB55">             
     
     
**On-board User LEDs**    
- Blue LED on PB5 
- Green LED on PB0      
- Red LED on PB1    
      
The STM32WB55XX is a family of multiprotocol wireless, ultra-low- power microcontrollers, offering robust capabilities for Bluetooth® Low Energy (BLE) 5.4 and IEEE 802.15.4-2011 compliance.     

It features a dedicated Arm® Cortex®-M0+ core for managing <ins>real-time low-level wireless protocol operations</ins>        
      
A high-performance Arm® Cortex®-M4 core operating at up to 64 MHz handles the main application logic.    
     
The Cortex®-MO+ core complements the M4 by managing wireless communication protocols, ensuring real-time performance without compromising application efficiency.      

Enhanced inter-core communication is facilitated by the IPCC (Inter-Processor Communication Controller) with six bidirectional channels.     

A Harware Semaphore (HSEM) module ensures seamless resource sharing between the cores    

### Key features   
**1. Wireless Capabilities**     
2.4 GHz RF transceiver supporting Bluetooth® 5.4, IEEE 80215.4, Thread 1.3, and Zigbee 3.0.    
         
- **Enhanced BLE Features:**   
2 Mbps PHY, Extended Advertising (EATT), GATT caching, and secure connection    
     

**2. Dual-Core Architecture**    
- **Cortex®-M4 Core: (CPU1)**    
  - Operates at up to 64 MHz with an integrated FPU
and DSP instructions.     
  - Adaptive real-time accelerator (ART) for zero-wait execution     

- **Cortex®-M0+ Core: (CPU2)**   
  - Optimised for real-time low-layer protocol processing       
     

**3. Peripherals**       
- 12-bit ADC   
- Dual-channel SAI for high quality audio processing.   
- 1x USART supporting ISO7816, IrDA, and Modbus.   
- 1x LPUART for low-power UART communication.   
- 2x SPI and 2x I2C interfaces with SMBus/PMBus support    
- Full-speed USB 2.0 with crystal-less BCD (Battery charging detection) and LPM support  

**4. Harware-Based Security**       
- AES-256 encryption, PKA and RNG.    
- Secure firmware installation (SFI) for BLE and IEEE 80215.4.   
- Integrated cryptographic algorithms: RSA, ECC, and
SHA-2.      
- Proprietary code protection against unauthorised access   

**5. Memory and Expandability**       
- 1 MByte Flash memory with readout protection.   
- 256 KBytes SRAM with hardware parity checks.   
- Quad-SPI interface with Execute-in-Place (XIP) support for external memory       
     
### Wireless protocol stacks of CPU2      
- The STM32WB supports a range of firmware stacks tailored for specific wireless communication protocols.    
- We can select and load the appropriate CPU2 firmware based on our target application        
     
| Stack | Associated Firmware |
|:-------------|:-------------|    
| **Bluetooth Low Energy (BLE)** |   |
|    |  `stm32wb5x_BLE_HCI_Adeca n_fw.bin`  |
|    | `stm32wb5x_BLE_HClLayer_fw.bin`         |  
|    | `stm32wb5x_BLE_HClLayer_fw.bin`  |     
|  | `stm32wb5x_BLE_LLD_fw.bin`  |      
|  |  `stm32wb5x_BLE_Stack_full_fw.bin` |      
|  |  `stm32wb5x_BLE_Stack_light_fw.bin`  |       
| **Thread** |   |       
|  |  `stm32wb5x_Thread_FTD_fw.bin`  |       
|  | `stm32wb5x_Thread_MTD_fw.bin`  |      
| **BLE and Thread** |   |        
|  | `stm32wb5x_BLE_Thread_dynamic_fw.bin`  |       
|  | `stm32wb5x_BLE_Thread_static_fw.bin ` |       
| **BLE and IEE 802.15.4** |   |       
|  |  `stm32wb5x_BLE_Mac_802_15_4_fw.bin` |       
| **BLE and Zigbee** |   |       
|  | `stm32wb5x_BLE_Zigbee_FFD_dynamic_fw.bin`  |       
|  | `stm32wb5x_BLE_Zigbee_FFD_static_fw.bin`  |        
|  | `stm32wb5x_BLE_Zigbee_RFD_dynamic_fw.bin`  |      
|  | `stm32wb5x_BLE_Zigbee_RFD_static_fw.bin`  |     
|  | `stm32wb5x_Zigbee_FFD_fw.bin`  |    
|  | `stm32wb5x_Zigbee_RFD_fw.bin` |    
| **IEEE 802.15.4 MAC** |   |    
|  | `stm32wb5x_Mac_802_15_4_fw.bin`  |    
|  | `stm32wb5x_Phy_802_15_4_fw.bin` |      

## CPU1 - Cortex-M4 (Application Layer)       
- **Profiles and Services:**     
The Cortex—M4 core executes user applications and interacts with predefined BLE, Zigbee, Thread, or IEEE 802.15.4 profiles and services.    

- **Interfaces:**     
<ins>ACI Interface</ins>: For BLE protocol.   
<ins>Thread Interface</ins>: For Thread stack communication.    
<ins>Zigbee Interface</ins>: For Zigbee stack communication.   
<ins>802.15.4 MAC Interface</ins>: For IEEE 802.15.4 communication    
     
- **Infrastructure Support:**    
CPU1 handles infrastructure tasks such as application logic, scheduling, and higher-level protocol management.    
     
<img src="images/cpu1-cpu2-stack-infrastructure.png" alt="CPU1 and CPU2 Stack">    

## CPU2 - Cortex-M0+ (Wireless Stack Layer)       
- **BLE Stack:**      
<ins>GAP/GATT</ins>: Manage connection roles and attribute profiles.   
<ins>SMP</ins>: Security management for pairing and bonding.    
<ins>ATT</ins>: Attribute protocol for data exchange.    
<ins>L2CAP</ins>: Logical link management.    
<ins>HostCtl Interface</ins>: Communication with BLE hardware layers.    
<ins>BT MAC and Link Layer</ins>: Manage BLE radio and data link        
     
- **Thread Stack:**      
<ins>CoAP</ins>: Application layer protocol for resource constrained devices.    
<ins>UDP + TLS</ins>: Transport layer with security.     
<ins>Routing, IPVS, 6LowPan</ins>: Manage IPv6-based communication over low—power networks.      
     
- **802.15.4 MAC:**      
Handles IEEE 802.15.4 standard communication for low-rate wireless networks.   
     
- **BLE Radio:**      
Dedicated to managing Bluetooth Low Energy RF communication    
    
- **802.15.4 Radio**:     
Dedicated to managing IEEE 802.15.4 RF communication (used by Thread and Zigbee)      
     
### BLE Application Using the HCI Layer interface      
- The CPU2 of the STM32WB series microcontroller can function as a BLE HCI (Host Controller Interface) co-processor, enabling us to integrate our own HCI-based BLE application or leverage an open-source BLE host stack for communication.     
- Most BLE host stacks interact with an HCI co-processor
through a **UART interface**.   
- However, on the STM32WB microcontroller, the equivalent physical communication layer is the **mailbox interface**     
> The BLE HCI layer acts as a bridge between the host stack (running on CPU1) and the controller stack (running on CPU2).    
    
*The mailbox interface support both*:    
- **BLE Channel** - Handles BLE communication    
- **System Channel** - Manages system level commands and events.    

*To communicate over the mailbox interface, the BLE host stack must*:  
- Construct *command buffers* to be transmitted via the BLE channel.   
- Implement an interface for *event reporting* when
data is received over the mailbox.   
- Notify the mailbox driver when an asynchronous packet is ready for release       

The STM32WB series microcontrollers support a **Static Concurrent Mode** (also known as **Switched Mode**), which allows the system to *alternate* between BLE and Thread protocols.     

- Both BLE and Thread stacks are embedded within the `stm32wb5x_BLE_Thread_fw` firmware.    
- Switching between BLE and Thread is controlled via system application commands.     
- The system disables the active protocol before enabling the other, ensuring protocol integrity     

**Protocol switching latency**     
- Transitioning from BLE to Thread requires completely stopping the BLE stack before the Thread stack can be activated, and vice versa.     
- This switching process can take several seconds, as
the network must reattach after each transition.            

### Thread Application     
The OpenThread stack runs on CPU2 of the STM32WB microcontroller and provides a set of APIs on CPU1, allowing us to build full-featured Thread applications.      

*The STM32WB supports three firmware configurations for the Thread
protocol*:
1. **Full Thread Device (FTD) Firmware** (`stm32wb5x_Thread_FTD_fw.bin`)   
    - Enables the microcontroller to support all Thread roles except for the Border Router.   
    - Supported roles include Leader, Router, End Device and Sleepy End Device    
    - Ideal for applications that require robust Thread networking capabilities.    

2. **Minimal Thread Device (MTD) Firmware** (`stm32wb5x_Thread_MTD_fw.bin`)      
    - Supports only End Device and Sleepy End Device roles.   
    - Consumes less memory compared to the FTD firmware, making it suitable for resource-constrained applications.     

3. **BLE + Thread Concurrent Firmware** (`stm32wb5x_BLE_Thread_fw.bin`)        
    - Provides simultaneous support for both Thread (FTD) and BLE in a static concurrent mode   
    - Allows applications to leverage both wireless technologies based on network requirements.   

### 802.15.4 MAC Application        
  - When using the STM32WB5x MAC 802.15.4 firmware (`stm32wb5x_Mac_802_15_4_fw.bin`), CPU1 gains direct access to the IEEE 802.15.4 MAC layer, enabling us to build custom applications on top of it.      

  - This approach allows for greater flexibility in implementing low-power mesh networking protocols such as 6LOWPAN or Zigbee, without relying on predefined protocol stacks.    
       
> ### BLE and Thread Concurrent Operation    
> The STM32WB series microcontrollers support a "Static Concurrent Mode" (also known as "Switched Mode"), which allows the system to alternate between BLE and Thread protocols.   
> - Both BLE and Thread stacks are embedded within the `stm32wb5x_BLE_Thread_fw` firmware.
> - Switching between BLE and Thread is controlled via system
application commands.    
> - The system disables the active protocol before enabling the other, ensuring protocol integrity

> *Protocol switching latency*:    
> - Transitioning from BLE to Thread requires completely
stopping the BLE stack before the Th read stack can be
activated, and vice versa.   
> - This switching process can take several seconds, as the network must reattach after each transition.       

## Core Principles and System integration     
The STM32WB microcontroller follows a dual—core software
architecture, where each core has distinct roles and access
privileges:

**CPU2 (Cortex-M0+) Firmware**:   
  - Delivered as an encrypted binary, ensuring security and integrity.  
  - Functions as a black box from the developer's perspective preventing modifications.   

**CPU1 (Cortex-M4) Firmware**:   
  - Provided as source code, allowing full customization and application development     

**Inter-Core Communication**:   
  - Uses a mailbox mechanism for efficient data exchange between CPU1 and CPU2.     

   
*To enhance system efficiency, STM32WB software includes key
application-level utilities*:   
   
**Task Sequencer**:    
- Manages background task execution.   
- Enables automatic entry into Low-Power Mode when no tasks are active.   
   
**Time Server**:    
- Provides virtual timers based on the Real-Time Clock (RTC).   
- Supports Stop and Standby modes, ensuring power-efficient
timekeeping.    
    
**AES Cryptographic Unit Allocation**   
- AES2: Exclusively assigned to CPU2 and must never be
accessed or used by CPU1.   
- AES1: Dedicated to CPU1 and is not utilized by CPU2.     
> **Exception**: CPU2 may access AES1 only when CPU1 initiates a request to write a user key into the customer key storage area.    


## Shared peripheral access (Concurrent peripheral access managment)    
- Peripherals accessible by both CPU1 and CPU2 are protected
using hardware semaphores.     
- Before a CPU accesses a shared peripheral, it must:
  1. Acquire the corresponding hardware semaphore.
  2. Perform the required operations on the peripheral.
  3. Release the semaphore once the operation is complete.    

- This mechanism ensures synchronization and data integrity when both cores interact with shared peripherals, preventing conflicts and unintended access.      

**General Recommendations for Semaphore Usage**

- To maintain compatibility with future firmware updates, it is advised that applications use semaphores starting from Sem31 downwards.

- This ensures that future wireless firmware updates for CPU1—which may introduce additional semaphores— do not conflict with existing implementations.      
     
### **Sem0** - RNG (Random Number Generator) Access Control
- Used to share the RNG between the two CPUs.
- CPU2 takes SemO for a duration dependent on:
  - The number of random numbers generated.
  - The RNG source clock speed.        

- **Optimization Tip**: To minimize latency, a pre-generated pool of random numbers should be maintained, with refilling handled by a low-priority task.     
- **USB Usage Consideration**: When disabling the USB interface, CPU1 must first acquire SemO before turning off the CLK48 clock, as USB and RNG share the same clock source.      
     
### **Sem1** — PKA (Public Key Accelerator) Access Control    
-  Dedicated to managing cryptographic operations between CPU1 and CPU2.       
     
### **Sem2** — Flash Memory Access Control    
- Sem2 is used to share between the two CPUs the capability to write/erase data in FLASH.   
- Sem2 must be taken before starting more than a single
write/erase procedure and released when they are completed.    
    
> BLE stack writes to flash memory the pairing information (when bonding is enabled) and the GATT attribute cache.    
     
### **Sem3** — Low-Power Mode Management    
- Used for the low power management.    
- It must not be locked for more than **500 us** by the CPU1 when there is BLE RF activity     
     
### **Sem4** — System Clock Synchronization During Low-Power Mode    
- Used to handle race condition on the switch of the system clock when a CPU exits low power mode while the other one enter low power mode     


## The Squencer    
In order to efficiently manage tasks across the cores and enable ultra-low-power operation, the firmware is built in layers that include a utility called the **sequencer**.     

The sequencer is responsible for executing registered functions sequentially in a structured and efficient manner. It offers the following key capabilities:    
- Supports the execution of up to 32 functions.    
- Allows functions to be requested for execution
asneeded.    
- Provides the ability to enable or disable function execution dynamically.    
- Includes a blocking interface that halts execution
until a specific event is received.    
- Tasks are registered by associating them with an
identifier and a callback function.   
- Later, when an event occurs or a time delay expires,
the task is "set" so that it will run in the main loop
context    

> **Squencer is NOT an RTOS**   
> **It not intended to function as an RTOS**     

- The typical API functions include registering a task and setting a task (using `UTIL_SEQ_SetTask()`).    

*For example, a task can be registered and later set as follows*:     
```c
// Register a task with a specific ID and callback
UTIL_SEQ_RegTask(1 << CFG_TASK_MY_TASK_ID, 6, MyTaskCallback);

// Later, set the task to be executed with a defined priority
UTIL_SEQ_SetTask(1 << CFG_TASK_MY_TASK_ID, CFG_SCH_PRIO_0);
```     

- Once set, the sequencer's main loop will eventually call the MyTaskCallback function in a background (non-interrupt) context.     
   
- This design makes it easier to write predictable, power-efficient code without resorting to preemptive multitasking.    

### Key freatures and functionalities of squencer      
- **Background Task Scheduling**    
    The sequencer manages tasks in the background, ensuring smooth function execution without requiring continuous CPU intervention.    

- **Optimized Low-Power Mode Handling**   
  - Implements a secure mechanism for entering Low—Power Mode while preventing event loss.
  - When no tasks are pending, the sequencer enables the system to enter low-power mode safely, improving energy efficiency.    

- **Event-Driven Execution**    
  - Provides an efficient mechanism for applications to pause execution until a specific event occurs.    
    
 During this waiting period, the system can:    
 - Enter low-power mode to reduce energy consumption.   
 - Execute other operations to optimize CPU usage.         


## Timer server      
- The Timer Server provides a flexible and efficient virtual timing mechanism for applications requiring precise event scheduling    
- It enables multiple virtual timers to operate using a shared RTC wake-up timer, optimizing resource usage.    

### Key features    
- Supports up to 255 virtual timers (subject to available RAM).  
- Configurable as either single-shot or repeated timers.   
- Allows dynamic modification of timeouts for active timers.   
- Enables stopping, restarting, and deleting timers when no longer needed.    
- Provides a timeout range from **1 to 2^32 - 1 ticks**.    
- Operates concurrently with the calendar without conflicts.        

*Each virtual timer can function in one of the following modes*:   
    
**Signle-Shot Mode**:   
- Executes once and notifies the application when the timer expires.     
- Remains registered in the system and can be restarted as needed.      

**Repeated Mode:**    
- Automatically restarts upon expiration with the same timeout value.    
- Notifies the application at the end of each cycle.         

### Implementation Steps      
- Configure the RTC (Real-Time Clock)     
- Initialize the Timer Server     
- Handle RTC Interrupts (Optional)     
- Create a Virtual Timer     

**Control Virtual Timers**:     
- Start/stop timers dynamically using their respective functions.   
- Modify timer timeouts if needed.    

**Delete Unused Timers**     
- Remove timers and free memory resources.                  









    




     


