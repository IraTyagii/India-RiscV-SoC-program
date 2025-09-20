# Task 2
### System Setup and Environment Configuration
<details>
<summary> System Configuration </summary>
   
### SYSTEM INFO:  



1. SYSTEM INFO:
   OS: Ubuntu 24.04.2 LTS  
   Kernel: 6.8.0-83-generic  
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
#### we verified the installation by using 
```bash
yosys --version
```
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
</details>  

<details><summary> GTKWave (Waveform Viewer) </summary></details>

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


</details>



