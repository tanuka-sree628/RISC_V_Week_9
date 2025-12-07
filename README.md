# India RISC-V Chip Tapeout Journey
A Complete Chronicle of My End-to-End Experience in the Open-Source Silicon Program

This repository captures my detailed learning path and hands-on development work as part of the India RISC-V Chip Tapeout Program—a national initiative that aims to democratize semiconductor design through open-source tools and accessible silicon fabrication.

My objective throughout this journey was to understand, design, verify, and tapeout a RISC-V–based SoC using a fully open-source toolchain—including OpenLane, OpenROAD, Magic, KLayout, Yosys, and the Skywater 130nm PDK.
Along the way, I encountered and resolved several real, industry-grade challenges that strengthened my understanding of chip design.

## 📚 Table of Contents

- Program Overview

- Tools & Technologies

- Design Flow Summary

- RTL Design Insights

- Key Debugging Challenges & Solutions

- Learnings & Reflections

- Acknowledgements

- Special Note of Gratitude

# ⭐ Program Overview

The India RISC-V Tapeout Program is a first-of-its-kind open-source VLSI initiative that enables students and engineers to design their own silicon chips—without needing direct access to expensive fabrication labs.

The program spans foundational to advanced topics including:

- RISC-V ISA fundamentals

- Semiconductor fabrication & PDK concepts

- SoC design and integration

- Custom RTL development

- Physical design using OpenLane

- Static Timing Analysis and signoff

- Final GDS submission for tapeout

# 🛠 Tools & Technologies Used

- Open-Source EDA Stack

- Yosys — RTL Synthesis

- OpenROAD — Full PnR toolchain

- OpenLane — RTL-to-GDS automation

- Magic — DRC and layout debugging

- KLayout — GDS visualization

- Netgen — LVS

- OpenSTA — Timing analysis

- SkyWater Sky130 PDK

# 🧩 RTL → GDSII Design Flow

The SoC followed the standard OpenLane flow:

- RTL Design → Synthesis → Floorplanning → Placement → CTS → Routing → SPEF Extraction → STA → DRC/LVS → GDSII Finalization

- This end-to-end pipeline provided an industry-relevant understanding of the ASIC design cycle.

# 🔧 RTL Design and Understanding

The primary design implemented was VSDBabySoC, containing modules such as:

- avsddac — DAC

- avsdpll — PLL

# Key tasks included:

- Debugging module hierarchy

- Fixing missing RTL source paths

- Understanding integration of macros

- Ensuring correct clock/reset propagation

# 🚧 Key Debugging Issues & How I Solved Them
- 1. Liberty files missing

Fix: Added correct .lib paths inside config.tcl

2. RTL file not found

Fix: Corrected the VERILOG_FILES list

3. Off-grid and misplaced pins

Fix: Edited LEF and aligned pins to the correct manufacturing grid

4. STA failures due to missing corners

Fix: Included ff, ss, and tt timing library sets

5. Routing Conjestion : explanation of how I solved them has ebeen given in week 8
   
Each issue helped solidify my understanding of EDA workflows and silicon-level constraints.

# 🎓 Major Learnings & Reflections

This journey equipped me with:

✔ A complete RTL-to-GDS skillset
✔ Strong grasp of STA and timing closure
✔ Experience modifying LEF/GDS files
✔ Insights into SoC-level integration
✔ Debugging confidence with real silicon constraints
✔ Hands-on mastery of open-source ASIC tools

Most importantly, I learned how real chips move from concept to fabrication, and how much meticulous engineering lies behind a single piece of silicon.

# 🤝 Acknowledgements

My heartfelt thanks to:

- VLSI System Design (VSD)

- The global open-source hardware community

- Contributors of OpenROAD, OpenLane, Yosys, Magic, KLayout

- SkyWater for releasing the Sky130 PDK

# ❤️ A Special Note of Appreciation for Mr. Kunal Ghosh

I would like to express my sincere appreciation to Mr. Kunal Ghosh, Co-Founder of VSD.

His visionary efforts have opened doors for students like me to engage directly with real silicon design—an opportunity that was once limited to major semiconductor industries.

Being part of this program has significantly shaped my perspective and helped me develop:

A disciplined, methodical approach to engineering

Strong confidence in tackling and resolving complex design challenges

The willingness to explore, make mistakes, grow, and innovate

Thank you, Sir, for placing your trust in students, advancing India’s semiconductor landscape, and creating pathways that were previously unimaginable.
It has been a privilege to participate in such a truly transformative journey.
