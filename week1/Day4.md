# GLS, Blocking vs Non - Blocking and synthesis-simulation mismatch
## 1. GLS, Synthesis-Simulation mismatch and Blocking/Non-blocking statements  
**GLS**  

GLS = Gate-Level Simulation
It means simulating the netlist (after synthesis), not the original RTL.

RTL simulation → Runs on your behavioral Verilog (using Icarus, etc.)

GLS → Runs on synthesized gate-level netlist (using standard cell gates + .lib timing).

Why we do GLS:

1) To check if the netlist works the same as RTL
2) To catch synthesis issues (like missing resets, uninitialized flops)  
3)To verify timing (with SDF back-annotation)

**Synthesis-Simulation mismatch**  

It happens when the behavior of your RTL simulation (before synthesis) ≠  the behavior of the synthesized netlist (after synthesis, during GLS).

This usually means the code was written in a way that the simulator and synthesizer interpret differently.

**Blocking/Non-blocking statements**  
| Feature                 | Blocking (`=`)          | Non-Blocking (`<=`)           |
| ----------------------- | ----------------------- | ----------------------------- |
| Execution order         | One after another       | All at once (parallel)        |
| Best for                | Combinational logic     | Sequential logic (flip-flops) |
| Behavior                | Like normal variables   | Like flip-flops/registers     |
| Race conditions         | More likely if misused  | Safer in sequential blocks    |
| Simulation vs synthesis | May mismatch if misused | Matches hardware behavior     |

### Summary




| Concept                           | Meaning                         | When Used            | Key Point                          |
| --------------------------------- | ------------------------------- | -------------------- | ---------------------------------- |
| **GLS**                           | Simulating gate-level netlist   | After synthesis      | Verifies netlist matches RTL       |
| **Blocking (`=`)**                | Immediate assignment            | Combinational blocks | Order matters                      |
| **Non-Blocking (`<=`)**           | Parallel assignment             | Sequential blocks    | Avoids race conditions             |
| **Synthesis–Simulation Mismatch** | RTL vs Netlist behavior differs | After synthesis      | Caused by bad coding or constructs |

## 2. labs on GLS and synthesis-simulation mismatch  
### Ternary_mux
```bash
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
<img width="858" height="1007" alt="Screenshot 2025-09-27 115932" src="https://github.com/user-attachments/assets/7ef4e1b1-eb46-42af-af2a-330a5537feb5" />


```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr ternary_operator_mux_net.v
show
```

<img width="829" height="702" alt="Screenshot 2025-09-27 120226" src="https://github.com/user-attachments/assets/3c5198c8-72d2-4d83-84d5-125c5ab8cafd" />


### GLS  
```bash
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux_net.v
./a.out
gtkwave  tb_ternary_operator_mux_net.v
```
<img width="892" height="697" alt="image" src="https://github.com/user-attachments/assets/90338d73-da5b-4abc-a0ec-34155c2c12f9" />

we can compare the synthesis simulation mismatch in the waveforms


### Bad_mux

```bash
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```
<img width="854" height="663" alt="Screenshot 2025-09-27 121313" src="https://github.com/user-attachments/assets/c47c559f-0965-4707-b461-b9df03b13680" />

```bash

yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog bad_mux.v
synth -top bad_mux 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr bad_mux_net.v
show
```
<img width="813" height="717" alt="Screenshot 2025-09-27 121435" src="https://github.com/user-attachments/assets/8a7234da-9828-4a05-a2a0-a9054a970128" />

```bash
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux_net.v
./a.out
gtkwave  tb_bad_mux_net.v
```
<img width="882" height="643" alt="image" src="https://github.com/user-attachments/assets/da843762-d976-4b2a-9ff2-2e53969296e1" />

## 3. Labs on synth-sim mismatch for blocking statement

### blocking caveat

```bash
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```
<img width="858" height="743" alt="Screenshot 2025-09-27 121703" src="https://github.com/user-attachments/assets/6dc13930-3995-4cc7-bfa9-2417c8c12333" />


```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr blocking_caveat_net.v
show
```
<img width="865" height="689" alt="Screenshot 2025-09-27 121817" src="https://github.com/user-attachments/assets/4a1ce46f-7a3a-4282-a367-f904e815530f" />


### GLS
```bash
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat_net.v
./a.out
gtkwave tb_blocking_caveat_net.v
```
<img width="888" height="702" alt="image" src="https://github.com/user-attachments/assets/68315b02-814a-4e8a-a7b1-53fac77221d3" />

