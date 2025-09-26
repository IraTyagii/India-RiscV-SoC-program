# INTRODUCTION TO RTL DESIGN AND SYNTHESIS
## Introduction to open source simulator iverilog
**What is a simulator?** 

A simulator is a tool that imitates the behavior of a real circuit or system before actually building it in hardware.  
RTL design is checked to follow a specific tech by simulating the design

**What is a Design?**  

The actual set of codes which have the funtions of the logics, planning and creating a circuit that does a specific job.

**What is a testbench?**  

A testbench is just a Verilog file that gives inputs to your design and checks the outputs.
It’s like a virtual lab setup where you “test” your circuit before building it.  

**How a simulator is used?**  
*Iverilog based simulation flow*

Verilog Codes and testbenches -> verilog Compiler (iverilog) → VCD(value change dump file) Output → GTKWave Viewer  

## Labs using iverilog and gtkwave  


### Make a directory
```bash
cd Documents
mkdir vsd
cd vsd
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd vlsi/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
<img width="889" height="354" alt="Screenshot 2025-09-26 142215" src="https://github.com/user-attachments/assets/0468221a-5fe8-4b62-8bb2-a835027b07a7" />




```bash
iverilog good_mux.v tb_good_mux.v
```
<img width="895" height="842" alt="Screenshot 2025-09-26 142557" src="https://github.com/user-attachments/assets/84b91208-88d1-48b7-a523-af3e60231b7d" />

a.out file is made




```bash
./a.out
```
<img width="880" height="96" alt="Screenshot 2025-09-26 142646" src="https://github.com/user-attachments/assets/85bd21ed-a017-493a-b0d7-bb63850c333f" />
dumped into tb_good_mux.vcd  



```bash
gtkwave tb_good_mux.vcd
```
<img width="997" height="644" alt="Screenshot 2025-09-26 143036" src="https://github.com/user-attachments/assets/1ec241d5-b5b3-4ab9-9ab8-7ff7b792f463" />




## Introduction to yosys and logic synthesis  
**synthesizer**  
**verify the synthesize**  
**RTL design**
**.Lib**
## Labs using yosys and sky130

```bash
cd Documents/vsd/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys
```

<img width="1055" height="258" alt="Screenshot 2025-09-26 232949" src="https://github.com/user-attachments/assets/bf2d2ebf-6227-449a-b2b3-5675b71a35cd" />

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
```

<img width="1028" height="302" alt="Screenshot 2025-09-26 232934" src="https://github.com/user-attachments/assets/7b536b75-5887-401e-8367-24085a922224" />

```bash
synth -top good_mux
```

<img width="1058" height="869" alt="Screenshot 2025-09-26 233145" src="https://github.com/user-attachments/assets/676877fd-dca6-4e42-903e-c7b6ddbc89d1" />
<img width="1048" height="913" alt="Screenshot 2025-09-26 233158" src="https://github.com/user-attachments/assets/49dbe8ea-1151-41b3-9eeb-711a5215ad4e" />

```bash
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="1054" height="847" alt="Screenshot 2025-09-26 233319" src="https://github.com/user-attachments/assets/e82519ed-ae44-4464-b0ca-ad5b488f304a" />

```bash
show
```
<img width="1060" height="962" alt="Screenshot 2025-09-26 233440" src="https://github.com/user-attachments/assets/43e26e3a-8108-4ca8-a285-48244100514c" />

```bash
write_verilog good_mux_netlist.v
!gvim good_mux_netlist.v
```
<img width="1037" height="920" alt="Screenshot 2025-09-26 234352" src="https://github.com/user-attachments/assets/c34b2cb3-c3b7-4841-8168-03e19bdc4010" />

```bash
write_verilog -noattr good_mux_netlist.v
!gvim good_mux_netlist.v
```
<img width="1046" height="932" alt="Screenshot 2025-09-26 234534" src="https://github.com/user-attachments/assets/172e1f2f-be11-4f42-a317-3216d75aaa6d" />




