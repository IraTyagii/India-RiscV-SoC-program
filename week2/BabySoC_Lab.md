# Week 2 – BabySoC Fundamentals & Functional Modelling

## 📑 Table of Contents
- [Objective](#objective)
- [Tools Used](#tools-used)
- [Steps Performed](#steps-performed)
- [Results & Observations](#results--observations)
- [Conclusion](#conclusion)


## Objective
To build a solid understanding of SoC fundamentals and practice functional modelling of the BabySoC using simulation tools (Icarus Verilog & GTKWave).

This lab covers:
- Reset operation  
- Clocking    
- Dataflow between Modules  

---

## Tools Used
- **Icarus Verilog (iverilog)** – Verilog simulation  
- **GTKWave** – Waveform analysis  

---

## Steps Performed
### 1. Cloned the VSDBabySoC GitHub repository.  

```bash
git clone https://github.com/manili/VSDBabySoC.git
cd VSDBabySoC
```
make a directory
```bash
mkdir -p simulation
```


### 2. Installed Icarus Verilog and GTKWave.  

(*Already done in week1 task2*)

### 3. Compiled the design using `iverilog`.  

Install sandpiper to compile rvmyth.tlv file
```bash
pip3 install pyyaml click sandpiper-saas
sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```

compile in iverilog
```bash
iverilog -o simulation/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module
```


### 4. Simulated to generate `.vcd` waveform file.  

```bash
cd simulation
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd
```


### 5. Opened `.vcd` in GTKWave and analyzed signals.  
<img width="803" height="272" alt="image" src="https://github.com/user-attachments/assets/c3f7c63e-ef6e-435b-b717-efa7ba1fa005" />

<details><summary>👉 OUTPUT SIGNALS </summary>


<img width="931" height="648" alt="Screenshot 2025-10-04 203949" src="https://github.com/user-attachments/assets/65fcdd09-e454-44bf-b96d-33e72dc894d4" />

<img width="1217" height="793" alt="Screenshot 2025-10-04 205448" src="https://github.com/user-attachments/assets/6d1eb6c3-cedf-4c3e-bcfc-029dbce4e074" />


<img width="1223" height="816" alt="Screenshot 2025-10-04 210207" src="https://github.com/user-attachments/assets/e388af81-26fb-4de4-9ce4-b1d264bf087d" />

<img width="1006" height="548" alt="Screenshot 2025-10-04 210808" src="https://github.com/user-attachments/assets/6d887d2b-ee6a-440b-8b33-6be35cb4997e" />
</details>

---

## Results & Observations

### 1. Reset Operation
- **Signals observed:** `reset` (top), CPU bus value  
- **Observation:**  
  - During reset (asserted), outputs remain idle.  
  - After reset (deasserted), CPU begins execution.

 

<details><summary> 👉 screenshot 1</summary>  

  *This waveform shows the system reset being asserted at the start and then released.*  
*Once released, the `RV_TO_DAC[9:0]` bus begins updating with valid data (`011, 000, 001, 003, 006, 00A`).*  
*This verifies both the correct reset functionality and the dataflow from internal registers to the DAC interface.*  
*The `clk` signal is stable with a 10ns period, ensuring synchronous operation.*


<img width="928" height="679" alt="Screenshot 2025-10-04 203857" src="https://github.com/user-attachments/assets/c698f579-d1ed-4bb6-aef7-0cbf308332d1" />
  
*Figure 1: Reset asserted at start; system begins operation after deassertion.*
</details>  

---

### 2. PLL Clocking
- **Signals observed:** `REF` (PLL input), `CLK` (PLL output), `ENb_CP`, `ENb_VCO`  
- **Observation:**  
  - `REF` toggles at base frequency.  
  - PLL output `CLK` becomes active when enabled.  
  - `ENb_CP` and `ENb_VCO` held low → PLL enabled.
  - PLL generates stable system clock from reference input, enabling the core to run  

<details><summary> 👉 screenshot 2</summary>
<img width="1009" height="640" alt="Screenshot 2025-10-04 210841" src="https://github.com/user-attachments/assets/b7f53f2a-4d78-496f-a855-80a09cea87eb" />
<img width="991" height="301" alt="Screenshot 2025-10-04 210917" src="https://github.com/user-attachments/assets/28839601-d031-415e-a5c6-db57dab91b5d" />

*Figure 2: PLL output clock generated from reference clock.*
</details>

---

### 3. Core (RVMYTH) Bus Activity Data Flow
- **Signals observed:** CPU register / bus driving DAC input (`D[9:0]`)  
- **Observation:**  
  - Bus values remain zero during reset.  
  - After reset, bus values change as CPU executes instructions.
  - Core produces digital values that will be sent to DAC.



<details><summary> 👉 screenshot 3</summary>  

  - (*This waveform shows activity inside the BabySoC CPU core:*)
- `CPU_reset_a1` is asserted initially and then deasserted, bringing the CPU into normal operation.
- The program counter (`CPU_pc_a2`) increments sequentially, confirming instruction fetch cycles.
- Register file read enables (`CPU_rf_rd_en1_a2`, `CPU_rf_rd_en2_a2`) toggle, activating register reads.
- Data lines (`CPU_result_a3`, `CPU_rf_rd_data1_a2`, `CPU_rf_rd_data2_a2`) change values, showing data transfer between the ALU and register file.
- Control signals such as `CPU_is_sw_a3` and `CPU_is_xor_a1` activate during specific instructions, verifying proper instruction decoding and execution.

- 
<img width="1205" height="478" alt="Screenshot 2025-10-04 205650" src="https://github.com/user-attachments/assets/48b8db7f-30de-4f70-a4ed-6a28f77f74c1" />

*Figure 3: Core generating digital data on DAC input bus. This waveform focuses on internal CPU signals.*
</details>

---

### 4. DAC Input vs Output
- **Signals observed:** `D[9:0]` (digital input), `VREFH`, `OUT` (analog output)  
- **Observation:**  
  - Digital bus `D[9:0]` drives proportional analog output `OUT`. (*DAC converts 10-bit digital input into proportional analog output*)
  - For example:  
    - `D=0` → `OUT=0 V`  
    - `D=512` → `OUT≈0.9 V` (for VREFH=1.8 V)  
    - `D=1023` → `OUT≈1.8 V`  

<details><summary> 👉 screenshot 4</summary>  
  
  *The digital input D[9:0] changes sequentially, and the OUT analog signal responds proportionally, proving proper DAC conversion.*
*CLK is stable and reset is low, indicating normal post-reset operation.*
  
<img width="1204" height="254" alt="Screenshot 2025-10-04 210230" src="https://github.com/user-attachments/assets/b55d6299-b91c-4752-bdf5-f8ec0ffd3f1f" />
 
*Figure 4: DAC converting 10-bit digital input to analog output.*
</details>

---

## Conclusion
By simulating the BabySoC design:
- Reset behavior was verified.  
- PLL successfully generated the system clock from a reference input.  
- Core was observed producing data flow on the DAC bus.  
- DAC output showed correct digital-to-analog conversion.  

This confirms that the BabySoC modules (Core + PLL + DAC) integrate and function together as expected.

---
*END of LAB*
