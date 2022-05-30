### ABOUT THE WORKSHOP
Mixed Signal SoC (RISC-V based Core + PLL) on FPGA
![bannerMixFPGA](https://user-images.githubusercontent.com/93269547/171051379-d18b3912-805b-4751-a3b7-9ab672790aef.png)

### AUTHOR OF THE WORKSHOP
#### Shivani Shah
#### Mr. Kunal Ghosh
Co-founder of VLSI System Design (VSD) Corporation Private Limited
#### Mr. Steve Hoover
Founder of RedwoodEDA Inc.
### AGENDA
 [Day 1 : ](#Day1)
  * [L1 - Introduction](#L1-Introduction)
  * [L2 - RVMYTH RISC-V Core](#L2-RVMYTH-RISC-V-Core)
  * [L3 - TL Verilog](#L3-TL-Verilog)
  * [L4 Why FPGAs?](#L4-Why-FPGAs?)
  * [L5 - Makerchip](#L5-Makerchip)
  * [L6 - TLV to RTL](#L6-TLV-to-RTL)
  * [L7 - iverilog Simulation](#L7-iverilog-Simulation)
  * [L8 - FPGA - Steps To Create project](#L8-FPGA-Steps-To-Create-project)
  * [L9 - FPGA - Steps To Generate IPs](#L9-FPGA-Steps-To-Generate-IPs)
  * [L10 - FPGA - RTL Simulation](#L10-FPGA-RTL-Simulation)
  * [L11 - FPGA - Synthesis](#L11-FPGA-Synthesis)
  * [L12 - FPGA - Implementation And Timing Analysis](#L12-FPGA-Implementation-And-Timing-Analysis)
  * [L13 - FPGA - Bit Stream Generation FPGA Programming And ILA](#L13-FPGA-Bit-Stream-Generation-FPGA-Programming-And-ILA)
  * [Acknowledgement](#Acknowledgement)
## L1 Introduction 
![](mixfpga/mixfpga.png)
## L2 RVMYTH RISC-V Core
Here we are using the RISC-V core that we made during RISC-V Myth workshop.
RISC-V featutes are:
- RISC-V ISA - Base Integer RV32I
- Written in TL-Verilog
- 5 Staged Pipelined Processor
- Register File Bypassed technique is used to mitigate data hazards
- B-type and J-tpe instruction takes extra 2 cycle to operate
- RVMYTH currently supports only the cpu core itself with a small instruction memory and data memory.
## L3 TL Verilog
TL-Verilog is a Verilog implementation of TL-X, a language extension defined as a wrapper to any HDL to extend it with transactional level modelling.
Simple: You will never be trip up by blocking vs nonblocking assignments, packed vs unpacked datatypes, generate and always blocks, reg vs wire vs logic vs bit.
Powerful: Timing Abstract to pipeline which make retiming easy and safe.
## L4 Why FPGAs?
FPGA is reconfigurable unlike ASIC which is application specific therefore not reconfigurable.
FPGA prototyping - used for verificaiton. While ASIC respin is highly expensive.
FPGA time taken to design is comparatively less than ASIC like about 1 month. ASIC time taken to design complex SoC nearly requires 1 year.
##### FPGA board used:
Zedboard Zynq-7000 ARM/FPGA SoC Development Board
## L5 Makerchip
##### Tools used:
Makerchip:Makerchip is a free web-based IDE as well as available as makerchip-app, a virtual desktop application for developing high-quality integrated circuits. You can code, compile, simulate, and debug Verilog designs, all from your browser. Your code, block diagrams, and waveforms are tightly integrated.

Icarus Verilog:Icarus Verilog is a Verilog simulation and synthesis tool.

GTKWave:GTKWave is a waveform viewer.

Xilinx Vivado:Xilixn Vivado provides complete SoC-strength, IP-centric and system-centric, next generation development environment. Currently, this project is done using Vivado HL Design Edition 2019.1.

## L6 TLV to RTL
Installtion and Overview of Sandpiper
SandPiper is a code generator that generates readable, well-structured, Verilog or SystemVerilog code from the given TL-Verilog code.
SandPiper SaaS Edition runs as a microservice in the cloud to support easy open-source development. Install Sanpiper SaaS Edition for this project.
To run locally, SandPiper Education Edition can be requested from RedwoodEDA
![](mixfpga/mixfpga_vfiles.png)
##### Steps to convert TL-Verilog to Verilog or SystemVerilog
- Install Sandpiper SaaS (https://pypi.org/project/sandpiper-saas/)
- git clone https://github.com/shivanishah269/vsdfpga.git
- cd vsdfpga/verilog
- sandpiper-saas -i rvmyth.tlv -o rvmyth.v --iArgs
![](mixfpga/mixfpga_vfiles_rvmyth.png)
## L7 iverilog Simulation
##### Steps for RTL Simulation of RVMYTH+PLL using iverilog
- iverilog rvmyth_pll_tb.v rvmyth_pll.v clk_gate.v
- ./a.out
- gtkwave rvmyth_pll.vcd
Here the simulation is generated using gtkwave
![](mixfpga/mixfpga_vfiles_gtkwave.png)
Looking at the value of DAC signal the value increases, then peaks at 255 and deacreases again.
![](mixfpga/mixfpga_gtkwave_decimal.png)
We can view this waveform in analog form as shown.
![](mixfpga/mixfpga_gtkwave_analog.png)
## L8 FPGA Steps To Create project Block Design
FPGA Implementation: Block Design
Step 1: Create the project with Zedboard as board option
![](mixfpga/mixfpga_vivado_project.png)
Step 2: Add rvmyth.v, clk_gate.v and top_SoC.v as design resources
Here I add the rvmyth.v and clk_gate.v files
![](mixfpga/mixfpga_vivado_addfiles.png)
Here I add to top_SoC.v file as design resource
![](mixfpga/mixfpga_vivado_top_SoCv.png)
The header for rvmyth are automatically fetch as shown
![](mixfpga/mixfpga_vivado_headerinrvmyth.png)
## L9 FPGA Steps To Generate IPs
FPGA Implementation: RTL Simulation
Step 3: Generate PLL and ILA IPs from Xilinx IP Catalog
Here I add the PLL IP using Xilinx IP Catalog. As shown a window wizard pop up.
![](mixfpga/mixfpga_vivado_ippll.png)
Here I configured the PLL to required Settings
![](mixfpga/mixfpga_vivado_ippllbufin.png)
Next is the Integrated Logic Analyzer IP is used and configured
![](mixfpga/mixfpga_vivado_ila.png)
Step 4: Add top_SoC_tb.v as simulation source
I then upload the testbench as required for the simulation.
![](mixfpga/mixfpga_vivado_tb.png)
## L10 FPGA RTL Simulation
Step 5: Run behavioral simulatioin and verify the funtionality
Here I run the simulation as shown.
![](mixfpga/mixfpga_vivado_runsimulation.png)
I then measure the frequency by marking the rising edge of consecuted cycle for each signal. Here the clock signal and the pll clock.
![](mixfpga2/mixfpga_vivado_frequencyanalog.png)
The output DAC signal of RISC-V can be viewed in analog as shown.
Step 6: Add constraints.xdc as a constraints source
![](mixfpga2/mixfpga_vivado_constraint.png)
Constrain file use in this lab are shown above.
## L11 FPGA Synthesis
Step 7: Run Synthesis
![](mixfpga2/mixfpga_vivado_synthesiscomplete.png)
## L12 FPGA Implementation And Timing Analysis
FPGA Implementation (PNR) & STA
Step 8: Run Implementation
Here I run the simulation after synthesis is completed and get error report as shown. 
![mixfpga_vivado_‫implementationfail](https://user-images.githubusercontent.com/93269547/171042124-ef2cba44-1be1-4463-a8c9-192a43aa0210.png)
Step 9: Check for setup and hold violations if any 
I then check for the violations report in the timing tab below as shown. 
![mixfpga_vivado_‫holdviolations](https://user-images.githubusercontent.com/93269547/171042146-f7cf61ad-2112-4187-bf86-1eeff1539363.png)
Here we have a hold violations with corresponding instance of connections.
![mixfpga_vivado_‫holdviolationspath69](https://user-images.githubusercontent.com/93269547/171042225-d52fa449-370c-495b-9269-7cf22adc31d9.png)
Here I edit the constraints to ignore the signals generated in these connections.
![mixfpga_vivado_‫constraintedited](https://user-images.githubusercontent.com/93269547/171042246-e9bb9c7b-d0aa-4d51-b5bd-d43ba4a56c64.png)
After editing the constraints file I run the synthesis and implementation again. 
![](mixfpga2/mixfpga_vivado_implementationcomplete.png)
And finally here I get red of hold violations.
![](mixfpga2/mixfpga_vivado_implementationcomplete2.png)
## L13 FPGA Bit Stream Generation FPGA Programming And ILA
Step 10: Generate the bitsteam and program FPGA
After the implementation is completed I generate the bitstream by pressing the bitstream button
![](mixfpga2/mixfpga_vivado_generatebitstream.png)
The Bitstream is complete. Now we need to connect the board so we can program the fpga board. The OpenManager automatically connects to the board if you press the autoconnect button.
![](mixfpga2/mixfpga_vivado_openmanager.png)
Since I do not have a board connected as of the moment I get these response as shown.
![](mixfpga2/mixfpga_vivado_noboard.png)

## Acknowlodgement
- Shivani Shah
- Kunal Ghosh, Co-founder, VSD Corp. Pvt. Ltd.
- Steve Hoover, Founder, Redwood EDA





