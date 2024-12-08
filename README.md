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

<details>
  <summary>Theory</summary>

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

</details>

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
<details>
  <summary>Practicals</summary>
  
A 'Die' which consists of core is small semiconductor material specimen on which fundamental circuit is fabricated.
In an ideal scenario we prefer utilization factor of 0.5 or 0.6

<img width="523" alt="image" src="https://github.com/user-attachments/assets/911938d1-eac0-4f0d-8dab-789b54573ec2">

** Define Location of preplaced cells (Macros)
These are IP's readily available in the market. Example Memory, Clock gating cells, Comparator etc. The arrangement of these IPS in a chip is referred as floorplanning. These IP's blocks are placed in chip before automated placement and routing and are called as pre-placed cells. Automated placement and routing tool places the remaining logical cells in the design onto chip.
Surround pre-placed cells with Decoupling capacitors. These decap cells power the block during switching activity.

Steps to run floorplan using openlane (page 8)
```bash
%run_floorplan
#Results in Screenshot 3
#Run Global Placement
%run_placement 
#Results in Screenshot 4
#Review floorplan files on command line
cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs
cd 01-12_18-18/logs/floorplan
#Grep for config variables read by the tool
cd 01-12_18-18/results/floorplan
ls -ltr
#Review floorplan files on command line
```
**Floorplan**
Floorplan generated at this can be viewed with magic or klayout tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
Screenshot 3 :

<img width="539" alt="image" src="https://github.com/user-attachments/assets/a733c80d-e0d2-42be-ae5e-97dff5bdcd9b">

? : Represents commands to be run on magic commandline
| **Shortcut/Action**        | **Description**                                                                 |
|----------------------------|--------------------------------------------------------------------------------|
| **S**                       | Select                                                                          |
| **V**                       | Fit the layout                                                                  |
| **Left and Right Click Mouse** | Select the area                                                              |
| **Left and Right Click Mouse** | Select the area                                                              |
| **Shift+Z**                       | Zoom Out                                                                     |
| **?what**                   | Magic command: Displays the selected metal layer in the terminal                 |
| **ss**                   | Shows connectivitity of the selected object                                        |
| **g**                   | Enable or Disable Grid                                        |
| **?box**                       | Dimension of Selected object                                                  |

### Things to Note at the Floorplan Stage

- **Standard Cells**:  
  Standard cells are not placed at this stage. You can observe all the cells in the lower-left corner.

- **Tap Cells**:  
  Tap cells are placed at equidistant intervals in the core area. They connect the n-well to VDD and the substrate to ground, helping to avoid latch-up.

**Placement**
- Global Placement
- Detailed Placement
HPWL : Half parameter wire length

Screeshot 4 :

<img width="533" alt="image" src="https://github.com/user-attachments/assets/9a6b9468-2e32-4b2d-9e80-60e5b4398ca1">

Results :
cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/05-12_16-52/results/placement
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

Placement DEF View :

<img width="713" alt="image" src="https://github.com/user-attachments/assets/4bb353d9-48e6-4f0f-a083-9f878ccd40fc">


**IO Placer Revision**
This example shows how to change a switch and rerun the openalane task iteratively to obtain the optimal results.

IO Placer is one of the opensource EDA tools which is used to place IO's around the core. The tool supports 4 pin placement strategy.
Inside openalane/configuration directory we have readme.md file which has brief description of all the switches.
So let us try to change the pin placement strategy in this example. FP_IO_MODE is a switch that decided the mode of IO placement. Default option of this variable can be observed in floorplan.tcl
In the openalane terminal
%set :env(FP_IO_MODE) 2
%run_floorplan

Observe the results the in the results directory of floorplan (Screenshot 5)
Screenshot 5:

<img width="355" alt="image" src="https://github.com/user-attachments/assets/6e75229b-5e30-431f-8c44-927bde34ce6a">

 Clone custom inverter standard cell design from github repository
 ```bash
cd Desktop/work/tools/openlane_working_dir/openlane
# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign
cd vsdstdcelldesign
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .
magic -T sky130A.tech sky130_inv.mag &
#Screenshot 6
```
Screenshot 6 :
<img width="377" alt="image" src="https://github.com/user-attachments/assets/272fcdb7-7358-4d46-b6cf-b192eb3cbf94">

</details>

# Day 3 : Introduction to Magic and ngspice
<details>
  <summary>Practicals</summary>
## Lab introduction to Sky130 basic layers layout and LEF using inverter

 Reference : https://github.com/nickson-jose/vsdstdcelldesign
 Extract Spice netlist on ngspice terminal
 %pwd
 %extract all
 #Generates Extracted netlist *.ext
 %ext2spice cthresh 0 rthresh 0
#Enable parasitic extraction
%ext2spice
#Extract netlist

Simulating the Generated netlist :
<img width="377" alt="image" src="https://github.com/user-attachments/assets/09fbee8d-f788-43d9-9ea8-48982eb25b1e">

#plot output vs input transient Screenhot 7
ngspice 1 -> plot y vs time a 

<img width="604" alt="image" src="https://github.com/user-attachments/assets/48eed693-d0ae-40a8-80bf-4d787f65e7ca">

Library Characterization : 
Measuring Rise/Fall Transition :

<img width="612" alt="image" src="https://github.com/user-attachments/assets/b1f8f317-4481-4f02-9c8b-518804447da9">


Measuring Rise and Fall Delay :
Rise Delay : Delay from 50% of input to 50% of output when the output is rising
Fall Delay : Delay from 50% of input to 50% of output when the output is falling

<img width="498" alt="image" src="https://github.com/user-attachments/assets/5855e2d9-1a93-46b5-9e9a-f84c01c2c9fb">


Magic Tutorials, Fixing DRC etc

DRM of Sky130 : https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html

```bash
# Change to home directory
cd
# Command to download the lab files
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
# Since lab file is compressed command to extract it
tar xfz drc_tests.tgz
# Change directory into the lab folder
cd drc_tests
# List all files and directories present in the current directory
ls -al
# Command to view .magicrc file, loads automatically when magic is opened
gvim .magicrc
# Command to open magic tool in better graphics
magic -d XR &
#Open met3.mag
```
The white patches are the DRC Errors highlighted by the tool

<img width="353" alt="image" src="https://github.com/user-attachments/assets/80515054-7ceb-44e8-9dd1-1120b31b93e4">

Enter ":" on magic layout view to switch to commandline mode in Magic and select the area that has drc error and enter "drc why". This will dispaly a brief description of the error.

<img width="353" alt="image" src="https://github.com/user-attachments/assets/9f9ee31b-57af-40a6-80ae-4951f807ec43">

:sif see via4

The above command will display the VIA in the VIA layer which is generally not visible in the tool. Helpful in resolving DRC-Metaloverlap errors. To undo the same type
:feed clear

<img width="210" alt="image" src="https://github.com/user-attachments/assets/d7068fd5-9a18-4f3d-945e-2bc9e3a1a025">

:box 
Can be used to measure bbox of selected region. It can also be used as a scale, as shown in below diagarm

<img width="360" alt="image" src="https://github.com/user-attachments/assets/f054f77d-7afa-47dd-84c2-7161315d9c58">

:load poly        #Will load poly.mag

<img width="552" alt="image" src="https://github.com/user-attachments/assets/2543c4d5-32c5-4760-a862-a4db09f9ed8b">

**Fixing DRC between Poly and Polyresistor**
<img width="308" alt="image" src="https://github.com/user-attachments/assets/20b46b89-5346-4dec-92ed-14edd11f0e1a">

Update Poly.9 DRC rule in sy130A.tech, load the tech file in magic and rerun DRC (Commands are enclosed in below snippet)
Now we could observe the spacing DRC.

<img width="697" alt="image" src="https://github.com/user-attachments/assets/3a46ebb3-0899-4bc8-9330-529f71f963ba">

**Incorrectly implemented difftap.2 simple rule correction**

<img width="692" alt="image" src="https://github.com/user-attachments/assets/fcca84a7-aadb-4f5f-8822-2abdfc0b0cf9">

**Incorrectly implemented nwell.4 complex rule correction**

<img width="719" alt="image" src="https://github.com/user-attachments/assets/35b71732-cb45-4c03-9803-4740bda8b6df">

```bash
drc style drc(full)
#Changes DRC style to Full
```
</details>


# Day 4 : Pre-Layout timing analysis and importance of good clock tree
<details>
  <summary>Practicals</summary>
  
## Timing modeling using delay table

Guidelines for PNR tool
* inputs and output port must lie on the intersection of vertical and horizontal tracks
* Width of the cell should be odd multiple of track pitch and Height should be odd multiple of track vertical pitch

The tracks.info file in OpenLane is used to provide information about the track definitions for routing in a physical design. This file plays a critical role in ensuring that the tools involved in the place-and-route (PnR) process understand the grid and track alignment of the standard cells and routing layers. Information used during routing stage.

tracks.info path : /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/tracks.info

<img width="344" alt="image" src="https://github.com/user-attachments/assets/267d88e4-0900-4e2c-afe5-6c59f8961b7e">

<img width="551" alt="image" src="https://github.com/user-attachments/assets/24476f8f-856b-4511-8314-75e11c804567">

Refer : https://github.com/nickson-jose/vsdstdcelldesign
Define Label pins and Ports for the layout and generate LEF

<img width="709" alt="image" src="https://github.com/user-attachments/assets/a499e17a-5cfd-4ba9-8d7d-6e3a0e6952b1">

## Adding our custom cell in openlane flow

Copy the generated LEF and it's dotlibs to the src directory. 
```bash
cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src
cp ../../../vsdstdcelldesign/inv_anchan.lef .
cp ../../../vsdstdcelldesign/libs/sky130_fd_sc_hd__* .
mv inv_anchan.lef sky130_vsdinv.lef
#Replace inv_anchan with sky130_vsdinv inside LEF
```
Commands to be added to config.tcl to include our custom cell in the openlane flow
```bash
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```
<img width="436" alt="image" src="https://github.com/user-attachments/assets/02353e45-a318-4119-926d-7a6d1dce9505">

```bash
cd /Desktop/work/tools/openlane_working_dir/openlane
docker
>./flow.tcl -interactive
>package require openlane 0.9
>prep -design picorv32a 
# Additional commands to include newly added lef to openlane flow
#included our lef in merged lef. that means pnr selects our cell
#/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/08-12_14-37/tmp/merged.lef
>set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
>add_lefs -src $lefs
>run_synthesis
```
<img width="434" alt="image" src="https://github.com/user-attachments/assets/26195f71-90af-424c-97a9-68cfff6870cc">

Run ended with huge slack and violation has to be fixed

```bash
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 08-12_14-37 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to display current value of variable SYNTH_STRATEGY
echo $::env(SYNTH_STRATEGY)

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# adds buffer to high fanout nets ensuring less delay and compensating area
echo $::env(SYNTH_BUFFERING)

# Controls sizing of gates to meet delay
echo $::env(SYNTH_SIZING)

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```
Area before : Chip area for module '\picorv32a': 147712.918400
Area after : Chip area for module '\picorv32a': 181730.544000
Area increase because of change in strategy.
Results of the run :

<img width="459" alt="image" src="https://github.com/user-attachments/assets/2206df1b-365e-4fb8-a820-756283082181">

%run_floorplan
Failed with below error message:

<img width="605" alt="image" src="https://github.com/user-attachments/assets/c0b43363-cb47-47fd-9c58-3ed87bef363b">

To resolve the error followed the steps mentioned in https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd?tab=readme-ov-file

```bash
Since we are facing unexpected un-explainable error while using run_floorplan command, we can instead use the following set of commands available based on information from Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands/floorplan.tcl and also based on Floorplan Commands section in Desktop/work/tools/openlane_working_dir/openlane/docs/source/OpenLANE_commands.md

# Follwing commands are all together sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or
```

<img width="466" alt="image" src="https://github.com/user-attachments/assets/be4d305c-713a-49e1-a6d4-e7ca55588c2e">

%run_placement
#Screenshot of Placement run
<img width="536" alt="image" src="https://github.com/user-attachments/assets/cae7ea08-32b4-4833-9183-52f75e09ffea">

*To view the results go to*
cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/08-12_14-37/results/placement
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def

#Custom Cell was found in a DEF view

<img width="538" alt="image" src="https://github.com/user-attachments/assets/e37728a8-433e-447b-8e37-4be603b9842d">

%expand
#To view the Abstract view in magic

<img width="536" alt="image" src="https://github.com/user-attachments/assets/701097aa-1b34-4b3a-bb77-dba68bbf30a6">


# LAB steps to configure OpenSTA for post-synth timing analysis

Place pre_sta.conf in openlane directory
```bash
set_cmd_units -time ns - capacitance pF -current mA -voltage V -resistance kOhm -distance um
read_liberty -max /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib
read_liberty -min /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib
read_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/08-12_14-37/results/synthesis/picorv32a.synthesis.v
link_design picorv32a
read_sdc /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/my_sdc.sdc
report_checks -path_delay min_max -fields {slew trans net cap input_pin} 
report_tns
report_wns
```
max .lib required for setup analysis and min .lib is required for hold analysis
picorv32a.synthesis.v : is the output of synthesis stage.

Modified my_sdc.sdc
```bash
set ::env(CLOCK_PORT) clk
set ::env(CLOCK_PERIOD) 24.73
#set ::env(SYNTH_DRIVING_CELL) sky130_vsdinv
set ::env(SYNTH_DRIVING_CELL) sky130_fd_sc_hd__inv_8
set ::env(SYNTH_DRIVING_CELL_PIN) Y
set ::env(SYNTH_CAP_LOAD) 17.653
set ::env(IO_PCT) 0.2
set ::env(SYNTH_MAX_FANOUT) 6

create_clock [get_ports $::env(CLOCK_PORT)]  -name $::env(CLOCK_PORT)  -period $::env(CLOCK_PERIOD)
set input_delay_value [expr $::env(CLOCK_PERIOD) * $::env(IO_PCT)]
set output_delay_value [expr $::env(CLOCK_PERIOD) * $::env(IO_PCT)]
puts "\[INFO\]: Setting output delay to: $output_delay_value"
puts "\[INFO\]: Setting input delay to: $input_delay_value"

set_max_fanout $::env(SYNTH_MAX_FANOUT) [current_design]

set clk_indx [lsearch [all_inputs] [get_port $::env(CLOCK_PORT)]]
#set rst_indx [lsearch [all_inputs] [get_port resetn]]
set all_inputs_wo_clk [lreplace [all_inputs] $clk_indx $clk_indx]
#set all_inputs_wo_clk_rst [lreplace $all_inputs_wo_clk $rst_indx $rst_indx]
set all_inputs_wo_clk_rst $all_inputs_wo_clk

# correct resetn
set_input_delay $input_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] $all_inputs_wo_clk_rst
#set_input_delay 0.0 -clock [get_clocks $::env(CLOCK_PORT)] {resetn}
set_output_delay $output_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] [all_outputs]

# TODO set this as parameter
set_driving_cell -lib_cell $::env(SYNTH_DRIVING_CELL) -pin $::env(SYNTH_DRIVING_CELL_PIN) [all_inputs]
set cap_load [expr $::env(SYNTH_CAP_LOAD) / 1000.0]
puts "\[INFO\]: Setting load to: $cap_load"
set_load  $cap_load [all_outputs]
```


</details>



