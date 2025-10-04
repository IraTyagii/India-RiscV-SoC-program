# Week 2 – BabySoC Fundamentals & Functional Modelling    

---

> **Objective:**  
> Build a solid understanding of SoC design fundamentals and learn how **BabySoC** fits into the overall chip design flow.  
> This includes the role of **functional modelling** using tools like *Icarus Verilog* and *GTKWave* before RTL and physical design.

---

<details>
<summary><strong>📌 What is a System-on-Chip (SoC)?</strong></summary>  



## What is a System-on-Chip (SoC)?

A System-on-Chip (SoC) is an integrated circuit that consolidates the essential components of a complete electronic system on a single silicon substrate. Instead of multiple discrete ICs on a PCB, an SoC integrates:

* CPU (Processor): Executes instructions, performs arithmetic/logic, and controls the system.

* Memory: RAM/ROM/Flash for data and instruction storage.

* Peripherals: Interfaces such as UART, SPI, I²C, GPIO, Timers, ADC/DAC for communication and control.

* Interconnect (Bus System): Links CPU, memory, and peripherals for smooth data transfer.

#### This high level of integration results in better performance, power efficiency, compactness, and cost savings — making SoCs the backbone of smartphones, IoT devices, automotive systems, and embedded platforms.

### How a System-on-Chip Works</strong></summary>

##### A System-on-Chip (SoC) powers up by loading boot instructions from ROM/Flash, and the CPU initializes the system. It **fetches, decodes, and executes instructions**, moves data via the interconnect, and interacts with peripherals using memory-mapped registers. Peripherals like timers or UART can operate semi-independently, while **power and clock management** optimize energy usage by gating unused blocks.
---
</details>   

---

<details>
<summary><strong>📌 Why BabySoC?</strong></summary>  

  
# BabySoC: A Simplified Educational SoC

**BabySoC** is a **miniature, educational System-on-Chip (SoC)** designed for learning and experimentation. Unlike commercial SoCs such as ARM Cortex or Intel processors, BabySoC is **small, modular, and easy to understand**. It is primarily intended for **learning, hands-on training, and experimentation**, rather than for production use.

---

### Core Modules  

#### Processor / Controller: A small RISC core that fetches, decodes, and executes instructions.  

#### Memory Interface: Simplified RAM/ROM to demonstrate read/write operations and bus signaling.  

#### Peripherals: LEDs, switches, counters, UART/timers for I/O interaction.  

#### Clock & Reset Logic: Ensures synchronization and initializes the system to a known state.  

---
### How Baby SoC Works – Step by Step

#### Step 1: Instruction Fetch
The CPU reads an instruction from memory (ROM).  
Each instruction tells the CPU what operation to perform.
#### Step 2: Instruction Decode
The CPU decodes the instruction to understand whether it’s:  
- An arithmetic operation
- Memory read/write  
- Peripheral interaction (like turning on an LED)
#### Step 3: Execute
The CPU executes the instruction:  
- **ALU (Arithmetic Logic Unit)** performs calculations  
- **Registers** hold temporary results
#### Step 4: Memory & Peripheral Access
If the instruction involves memory or a peripheral:  
- The CPU sends an **address** and **control signals** over the bus  
- The memory or peripheral responds with data or performs an action
#### Step 5: Write Back
The result of the operation (from ALU or memory) is stored back in a CPU register.
#### Step 6: Next Instruction
The CPU moves to the next instruction and repeats the cycle.

</details>

---
  
<details>
<summary><strong> 📌 Role of Functional Modelling</strong></summary>

Before RTL and physical design, **functional modelling** plays a crucial role in verifying the SoC’s architecture.  

####  Why it matters
- Detects **logical/architectural issues early**.  
- Reduces time wasted in later design stages.  
- Provides a **high-level behavioural simulation** to ensure that all components work together.

####  Tools Used
- **Icarus Verilog** – for compiling and simulating the design.  
- **GTKWave** – for viewing waveforms and signal interactions over time.

This stage validates the **architecture of BabySoC** and ensures correctness before synthesis and layout.

</details>

---

##  Summary

Understanding SoC fundamentals is the **foundation of chip design**.  
**BabySoC** simplifies complex SoC concepts, allowing students to **focus on learning** rather than wrestling with unnecessary details.  

By performing **functional modelling**, we confirm the logical correctness of the design before moving into RTL implementation and physical design stages — making the entire chip flow **more robust, structured, and efficient**.

---

#### 🌐 References
- [📚 Fundamentals of SoC Design Notes](https://github.com/hemanthkumardm/SFAL-VSD-SoC-Journey/tree/main/11.%20Fundamentals%20of%20SoC%20Design)

---

 *This write-up is part of Week 2 of the India RiscV SoC Journey.*  
 *Tools: Icarus Verilog, GTKWave*

