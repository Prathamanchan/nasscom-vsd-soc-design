# Digital VLSI SOC Design and Planning

# Course Overview

### Day 1: Introduction to OpenLane and Synthesis

<details>
  <summary>Day 1 Topics</summary>

- **Familiarization with OpenLane**
- **Physical Design (PnR) stages**
- **Design Preparation stages**
- **Execution and Analysis of synthesis**

</details>

### Day 2: Floorplan and Placement

<details>
  <summary>Day 2 Topics</summary>

- **Introduction to Floorplan**
- **LEF vs DEF**
- **Special Cells**
- **Execution and Analysis of Placement**

</details>

### Day 3: Standard Cell Design and Simulation

<details>
  <summary>Day 3 Topics</summary>

- **Standard cell design using sky130 PDK**
- **SPICE simulation in ngspice**
- **Standard cell characterization**

</details>

### Day 4: Custom Cell and Timing Analysis

<details>
  <summary>Day 4 Topics</summary>

- **PnR with custom cell**
- **CTS and STA**
- **Timing ECOs**

</details>

### Day 5: Routing and Analysis

<details>
  <summary>Day 5 Topics</summary>

- **Routing**
- **Algorithm behind Trinston Route**
- **SPEF Analysis**

</details>


# Day 1 : Inception of OpenSource EDA, Openlane and Sky130 PDK

<img width="359" alt="image" src="https://github.com/user-attachments/assets/cd204ce1-22b8-4874-b087-c1f28368286e">

RTL, EDA Tools and PDK are three key ingredients of ASIC Design

PDK : Is a set of libraries and associated data to design in a particular technology. It is a interface between FAB and designers. A PDK contains following information
*  Transistors
*  Standard Cells (For digital designs)
*  Design Rules
*  Timing/Power information in liberty file
*  Technology layer information.

OpenLane Flow
OpenLane is an automated RTL to GDSII flow. The flow performs full ASIC implementation from RTL all the way down to GDSII
OpenLane is based on several opensource projects such as OpenRoad, magic, klayout, fualt, yosys and Qflow etc.
Goal of openlane tool is to produce a clean GDSII with no human intervention.

## Day1 : Practicals 

<details>
  <summary>OpenLane Directory Structure</summary>
  
```bash
cd ~
cd Desktop/work/tools/openlane_working_dir/pdks
ls -l
# ------------------------------------------------------------------------
# open_pdks  : Script that makes commercial PDKs compatible with open-source tools.
# skywater-pdk : PDK compatible with commercial EDA tools.
# sky130A       : Open-source PDK designed to be compatible with open-source EDA tools.
# ------------------------------------------------------------------------
cd sky130A
# ------------------------------------------------------------------------
# libs.ref  : Contains Liberty, LEF, and other files specific to the technology.
# libs.tech : Contains files specific to EDA tools, e.g., Magic, KLayout, etc.
# ------------------------------------------------------------------------
cd libs.ref/sky130_fd_sc_hd
ls
# ------------------------------------------------------------------------
# cdl      : Circuit Description Language files (netlist format).
# doc      : Documentation related to the PDK or design.
# gds      : GDSII files containing layout data.
# lef      : Library Exchange Format files describing layout abstracts.
# lib      : Liberty files for timing, power, and area characterization.
# mag      : Magic layout files.
# maglef   : Magic layout files with LEF abstraction.
# spice    : SPICE netlist files for simulation.
# techlef  : Technology-specific LEF files.
# verilog  : Verilog HDL files for design and simulation.
# ------------------------------------------------------------------------
# ------------------------------------------------------------------------
# irsim      : Digital circuit simulator for switch-level simulation.
# klayout    : Open-source layout viewer and editor.
# magic      : Open-source VLSI layout tool.
# netgen     : LVS (Layout vs. Schematic) and netlist comparison tool.
# ngspice    : Open-source SPICE simulator for analog and mixed-signal circuits.
# openlane   : Open-source RTL-to-GDS flow for digital design.
# qflow       : Complete open-source digital synthesis flow.
# xcircuit   : Schematic capture tool and netlist generator.
# xschem     : Schematic capture tool with SPICE integration.
# ------------------------------------------------------------------------

```
</details>

<details>
  <summary>Working with OpenLane</summary>
  
```bash
cd ~/Desktop/work/tools/openlane_working_dir/openlane
# ------------------------------------------------------------------------
# This is the directory where we will be working.
# The Openlane tool will be invoked from here.
# ------------------------------------------------------------------------
alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
docker
./flow.tcl -interactive
# The above command will invoke openLane in interactive mode. Without -interactive mode the tool attempts to complete the flow in one run
# %:Represents openlane terminal
% package require openlane 0.9

cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a
ls -ltr 
# ------------------------------------------------------------------------
# config.tcl                     : Openlane configuration file used for design configuration. Redefines default settings of the tool.
# src                            : Directory where Verilog and SDC files will be included.
# sky130A_sky130_fd_sc_hd_config.tcl : Custom TCL file, sourced inside config.tcl.
# ------------------------------------------------------------------------

% prep -design picorv32a
# Results in Screenshot 1
# Prepares designs and reads config.tcl. Creates Runs directory
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/01-12_18-18
ls -ltr
# ------------------------------------------------------------------------
# cmds.log                       : Log file that records all commands run.
# config.tcl                     : Default parameters taken by the run.
# logs                           : Contains logs from each stage of the process.
# reports                        : Timing reports generated from each stage.
# results                        : Output results from each Openlane stage.
# tmp                            : Temporary files are stored during the run.
# ------------------------------------------------------------------------
%run_synthesis
# Results in Screenshot 2
# ------------------------------------------------------------------------
# Runs Yosys and ABC (Logic Mapping and Optimization)
#
# Yosys: Open-source synthesis tool that takes high-level RTL code (e.g., Verilog)
#        and converts it into a gate-level netlist. It performs various optimization
#        tasks, including synthesis, technology mapping, and formal verification.
#
# ABC: A tool for logic synthesis, optimization, and technology mapping. It is often
#      used after Yosys to further optimize the netlist generated and map it to a 
#      specific technology library
#
# In this step, Yosys synthesizes the RTL code, and ABC performs additional mapping
# to the target library, ensuring that the design meets the desired area, timing,
# and power requirements. This process is crucial for preparing the design for
# further stages in the ASIC flow, such as place and route.
# ------------------------------------------------------------------------

# ------------------------------------------------------------------------
# Results directory will have the synthesized netlist file: picorv32a.synthesis.v
# Path to the results directory:
# cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/01-12_18-18/results/synthesis
#
# Timing reports from the synthesis stage can be found in the reports directory.
# Path to the reports directory:
# cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/01-12_18-18/reports/synthesis
# ------------------------------------------------------------------------

```

Screenshot 1 :

<img width="461" alt="image" src="https://github.com/user-attachments/assets/cf377477-7efd-45ea-acbc-95805e451455">

Screenshot 2 :

<img width="476" alt="image" src="https://github.com/user-attachments/assets/cb2443d8-8757-47cf-8130-b921eaa4d59e">

Exercise :

<img width="437" alt="image" src="https://github.com/user-attachments/assets/cc2ab4fa-d2f9-4f5d-8aa9-2d629bafb4c1">


Smaller Designs we can run without interactive mode but for larger designs we need to explore each and every design to check if everything is meeting the requirement.
</details>

# Day 2 : Good floorplan vs bad floorplan and intro to libcells
A 'Die' which consists of core is small semiconductor material specimen on which fundamental circuit is fabricated.
In an ideal scenario we prefer utilization factor of 0.5 or 0.6
<img width="523" alt="image" src="https://github.com/user-attachments/assets/911938d1-eac0-4f0d-8dab-789b54573ec2">





