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

