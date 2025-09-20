# Task 2
## System Setup and Environment Configuration
<details>
<summary> System Configuration </summary>
   
### SYSTEM REQUIREMENTS AND CONFIGURATION:  



1. SYSTEM INFO:  
   OS: Ubuntu 24.04.2 LTS   
   Architecture: x86_64

2. CPU INFO:  
   Cores: 8
   
3. MEMORY INFO:  
   Total RAM: 6032 mb or 6 gb  
   Available storage: 50 gb
   </details>
<details><summary> Tools Installed </summary> 
   
<details><summary> Yosys (Synthesis)  </summary>
   
   # YOSYS SETUP
   #### Installation
```bash
sudo apt-get update
sudo apt install git
git clone https://github.com/YosysHQ/yosys.git
cd yosys
sudo apt install make
sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
git submodule update --init --recursive
make config-gcc
make
sudo make install
```
<img width="738" height="481" alt="Screenshot 2025-09-20 180800" src="https://github.com/user-attachments/assets/812298c2-6072-44b7-aa1f-522caea61197" />


<img width="737" height="487" alt="Screenshot 2025-09-20 180735" src="https://github.com/user-attachments/assets/a5d54693-bd3a-444a-9827-0dde849e1c36" />



<img width="822" height="577" alt="Screenshot 2025-09-20 212101" src="https://github.com/user-attachments/assets/ae0e55fe-bb49-47fb-ac97-d5871ffe1c31" />


#### we verified the installation by using 
```bash
yosys --version
```
<img width="787" height="74" alt="Screenshot 2025-09-20 215142" src="https://github.com/user-attachments/assets/570af2ec-d1c2-4038-8280-73555da3c13c" />

</details>

<details><summary> Icarus Verilog (Simulation) </summary>

   
   # IVERILOG SETUP
   #### INSTALLATION
   
   ```bash
sudo apt-get update
sudo apt-get install iverilog
```
#### For verification 
   
```bash
iverilog -V
```
<img width="815" height="584" alt="Screenshot 2025-09-20 214700" src="https://github.com/user-attachments/assets/86f76f63-2c3a-4b59-a5b1-52930395ed72" />

</details>  

<details><summary> GTKWave (Waveform Viewer) </summary>

# GTKWAVE SETUP
#### Installation
``` bash
sudo apt-get update
sudo apt install gtkwave
```
#### Verified using
```bash
gtkwave --version
```
<img width="816" height="579" alt="Screenshot 2025-09-20 215101" src="https://github.com/user-attachments/assets/bb67e459-8439-4683-b794-e70189b70a61" />

</details>


</details>



