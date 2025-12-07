# RISC-V_SOC_Week7
## VSDBabySoC Physical Design Flow – Sky130HD (OpenROAD)

This document summarizes the complete RTL-to-GDS flow carried out for the VSDBabySoC design using OpenROAD-flow-scripts on the Sky130HD PDK. It includes the commands used, stage-wise observations, and notes on routing and parasitic extraction.

# 1. Deliverables
## 1.1 Layout Screenshots Captured

## Floorplan view
![Yosys_simulation](assets/floorplan.jpeg)

## Placement view
![Yosys_simulation](assets/placement.jpeg)

## CTS view
![Yosys_simulation](assets/cts.jpeg)

## Routing view (noting congestion issues)
![Yosys_simulation](assets/routing_failed.jpeg)

Terminal log showing SPEF generation

# 2. Commands Used (Step-by-Step)
## 2.1 Running the Full Flow
''' bash
cd OpenROAD-flow-scripts/flow
make DESIGN_CONFIG=designs/sky130hd/VSDBabySoC/config.mk
'''

## 2.2 Viewing Results in OpenROAD GUI

Start GUI:
''' bash
openroad -gui
'''
Load LEF files:
''' bash
read_lef platforms/sky130hd/lef/sky130_fd_sc_hd.tlef
read_lef platforms/sky130hd/lef/sky130_fd_sc_hd_merged.lef
read_lef platforms/sky130hd/lef/sky130io_fill.lef
'''
Load specific design stage databases:

Floorplan
''' bash
read_db results/sky130hd/VSDBabySoC/base/2_floorplan.odb
'''
Placement
''' bash
read_db results/sky130hd/VSDBabySoC/base/3_5_place_dp.odb
'''
CTS
''' bash
read_db results/sky130hd/VSDBabySoC/base/4_1_cts.odb
'''
Global Routing (congestion reported)
''' bash
read_db results/sky130hd/VSDBabySoC/base/5_1_grt-failed.odb
'''
# 3. Observations and Challenges
Floorplanning

-Core utilization was applied correctly.

-Power distribution network (PDN) generation completed successfully.

Placement

-Both global and detailed placement completed without any major violations.

-No significant density issues observed.

Clock Tree Synthesis (CTS)

-CTS completed successfully.

-Clock buffers were inserted as expected.

-Skew and insertion delay values remained within acceptable limits.

Routing

-Routing did not complete successfully.

-Global routing reported congestion in multiple areas.

Error:
'''
[ERROR GRT-0116] Global routing finished with congestion.
'''
-A failed routing database was generated:
  5_1_grt-failed.odb
-Due to congestion, detailed routing could not proceed.

-Routing issues have been documented for debugging and future improvement.

# 4. SPEF Generation

After routing (if successful), OpenROAD generates a SPEF file automatically.

Typical parasitic extraction output:
'''
Writing parasitics SPEF...
'''
Commands used internally:
'''
estimate_parasitics -global_routing
write_spef results/sky130hd/VSDBabySoC/base/VSDBabySoC.spef
'''
# 5. SPEF Overview and Importance

A SPEF (Standard Parasitic Exchange Format) file contains detailed parasitic information, including:

-Wire resistances

-Net capacitances

-Coupling capacitances

-RC data contributing to signal delay

# Importance for Static Timing Analysis (STA):

-Timing without SPEF assumes ideal (zero-delay) interconnects.

-Timing with SPEF incorporates real wire RC delays.

-Leads to more realistic slack, setup/hold, and clock skew analysis.

-Essential for sign-off quality timing verification.

6. Verification Summary:

| Stage                  | Status                   |
| ---------------------- | ------------------------ |
| RTL Synthesis          | Completed                |
| Floorplan              | Successful               |
| Placement              | Successful               |
| CTS                    | Completed                |
| Routing                | Failed due to congestion |
| Congestion Reports     | Generated                |
| Intermediate ODB Files | Saved                    |

# Final Note

The flow progressed smoothly until routing. Congestion in the design prevented global and detailed routing from completing successfully. All log
