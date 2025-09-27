# Optimization in synthesis
## 1. If case constructs

If is a priority logic.  
| Feature        | `if` Statement                                        |
| -------------- | ----------------------------------------------------- |
| Function       | Makes decisions based on conditions                   |
| Synthesized as | Multiplexers / control logic                          |
| Used in        | Combinational & sequential always blocks              |
| Important      | Include `else` or default assignment to avoid latches |




| Mistake                                          | What happens                                                                    |
| ------------------------------------------------ | ------------------------------------------------------------------------------- |
| ❌ Missing `else`                                 | Synthesis **infers a latch**, because output needs to “remember” previous value |
| ❌ Using blocking `=` in sequential always blocks | Can cause simulation–synthesis mismatches                                       |
| ❌ Multiple always blocks driving same signal     | Causes conflicts                                                                |


## 2. Labs on "Incomplete If Case"


In the same directory
```bash
iverilog incomp_if.v tb_incomp_if.v
./a.out
gtkwave tb_incomp_if.vcd
```
then in yosys
```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog incomp_if.v
synth -top incomp_if
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```


### the output latches as shown the the gtkwave output  


<img width="894" height="645" alt="Screenshot 2025-09-27 123047" src="https://github.com/user-attachments/assets/f692ccd7-8349-4a21-ae0d-1099354761d7" />
<img width="793" height="694" alt="Screenshot 2025-09-27 123242" src="https://github.com/user-attachments/assets/2219f09b-d2cc-44d5-a64e-6f7b55bceda2" />

### for second case we get output latched on input

<img width="896" height="652" alt="Screenshot 2025-09-27 123455" src="https://github.com/user-attachments/assets/acb8703f-39a7-4368-921c-809014ca0e24" />

## 3. Labs on Incomplete overlapping case

### Latches are not formed in these cases


### lab1: 

```bash
iverilog comp_case.v tb_comp_case.v
./a.out
gtkwave tb_comp_case.vcd
```
output  
<img width="895" height="773" alt="Screenshot 2025-09-27 230337" src="https://github.com/user-attachments/assets/3020f18d-6d0c-4846-b12b-fb15e39dfcf1" />



```bash

yosys
read_verilog comp_case.v
synth -top comp_case
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
output:-<img width="1050" height="743" alt="Screenshot 2025-09-27 230459" src="https://github.com/user-attachments/assets/0759839c-4c34-485c-aea7-f89a60fd8065" />

### Lab2

```bash
yosys
read_verilog partial_case_assign.v
synth -top partial_case_assign
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
output
<img width="1195" height="708" alt="Screenshot 2025-09-27 231027" src="https://github.com/user-attachments/assets/ba73079e-4010-40a4-9db3-df652a0658b2" />





## 4. For loop and for generate

| Feature           | `for` Loop                     | `for-generate` Loop                       |
| ----------------- | ------------------------------ | ----------------------------------------- |
| Used in           | Procedural blocks (`always`)   | Generate blocks (outside always)          |
| Purpose           | Repeat operations (behavioral) | Create multiple instances (structural)    |
| When it runs      | During simulation              | During elaboration (compile time)         |
| Variable type     | `integer`                      | `genvar`                                  |
| Creates hardware? | No (synthesizer unrolls logic) | Yes (instantiates multiple modules/gates) |
| Common use        | Loops over bits in a vector    | Arrays of modules, gates, registers       |




## 5. Labs on "for loop" and "for generate"


### Using the similar steps, we got the outputs for  
1. MUX

  output
  <img width="1004" height="647" alt="Screenshot 2025-09-27 231125" src="https://github.com/user-attachments/assets/f6d6041d-d093-49a7-b86d-41e6a9c43499" />


2. DE-MUX

   output

   <img width="1165" height="660" alt="Screenshot 2025-09-27 231231" src="https://github.com/user-attachments/assets/d30267a5-4221-40b9-9ebc-d4013bb79924" />

   
3. Ripple Carry Adder

   output
   <img width="1143" height="647" alt="Screenshot 2025-09-27 231450" src="https://github.com/user-attachments/assets/3e91c313-8f91-469c-ad45-93aab6d2f1d3" />


