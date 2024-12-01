Digital VLSI SOC Design and Planning

Course Overview

Day 1
* Familiarization with OpenLane
* Physical Design (PnR) stages
* Design Prepeation stages
* Execution and Analysis of synthesis

Day 2
* Introduction to Floorplan
* LEF vs DEF
* Special Cells
* Execution and Analysis of Placement

Day 3
* Standard cell design using sky130 pdk
* SPICE simulation in ngspice
* Standard cell characterization

Day 4
* PnR with custom cell
* CTS and STA
* Timing ECO's

Day 5
* Routing
* Algorithm behind Trinston Route
* SPEF Analysis

Day 1 : Inception of OpenSource EDA, Openlane and Sky130 PDK

<img width="359" alt="image" src="https://github.com/user-attachments/assets/cd204ce1-22b8-4874-b087-c1f28368286e">
RTL, EDA and PDK are three key ingredients of ASIC Design

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

Day1 : Practicals 

<details>
  <summary>Click to expand the Bash code</summary>
  
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
