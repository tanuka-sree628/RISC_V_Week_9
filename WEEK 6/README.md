# RISC_V_Week_6
## Day 1 – Inception of Open-Source EDA, OpenLANE, and Sky130 PDK

The open-source revolution in VLSI design began with the goal of democratizing chip design. Proprietary EDA tools and commercial PDKs were major barriers for students and startups, and this gap was filled by open-source projects like OpenLANE and SkyWater’s Sky130 PDK. OpenLANE acts as an automated RTL-to-GDSII flow that integrates several open tools such as Yosys for synthesis, OpenROAD for place and route, Magic for layout, and OpenSTA for timing analysis. Sky130 provides all the process data and rules to make real silicon chips possible. Understanding these tools helps one visualize how code eventually turns into physical transistors on silicon.

Communicating with computers essentially means translating human logic into a form the machine can execute. Software developers use languages like C or Python, while hardware designers use RTL (Verilog or VHDL). Each statement at the high level eventually becomes a sequence of logic gates and transistors that respond to clock signals.

A QFN-48 package is a physical form factor with 48 pins and no leads, used to mount a chip on a PCB. Inside the package, the die sits on a paddle connected to bond wires that link the die’s pads to the package pins. The core region contains logic circuits, while the pad ring handles I/O and power. Within the chip, reusable design blocks called IPs (like UART, SPI, or RISC-V cores) are integrated to form a complete SoC.

RISC-V is a fully open instruction set architecture that anyone can implement without licensing fees. It provides a clean, modular design supporting different word lengths and optional extensions. Because of its openness, RISC-V has become the backbone for academic research and open silicon projects such as Shakti, RocketChip, and PicoRV32.

Transitioning from software to hardware means identifying computationally intensive portions of software and moving them to dedicated hardware blocks for acceleration. This transformation is achieved by designing specialized hardware circuits for specific algorithms, which execute faster and consume less power compared to general-purpose processors.

## SoC Design and OpenLANE

In the world of digital ASICs, open-source design involves a complete ecosystem of tools and file formats that handle everything from RTL to layout verification. The core components include synthesis, placement, routing, timing analysis, and verification tools that collaborate through standardized formats such as LEF, DEF, and Liberty files.

The RTL to GDS flow represents the complete digital design journey — starting from Verilog RTL, which is synthesized into a gate-level netlist, followed by floorplanning, placement, clock tree synthesis, routing, parasitic extraction, and finally GDSII generation for fabrication.

OpenLANE automates this entire pipeline, acting as a single umbrella that ties all these stages together through configurable scripts. It offers a reproducible and open environment to experiment with chip design. The Strive chipset serves as a reference for SoC development within OpenLANE, showcasing integration of IPs and flow execution. Understanding OpenLANE’s internal structure — how each stage connects, what files are produced, and how constraints are handled — forms the foundation for efficient ASIC implementation.

Getting Familiar with Open-Source EDA Tools

The directory structure in OpenLANE is neatly organized to manage every phase of design. The designs folder contains user projects, flow has the automation scripts, pdks stores process-related files, and runs captures all intermediate and final results including GDS, DEF, and reports.

Before starting the flow, the design preparation phase ensures everything is ready. This includes verifying RTL syntax, defining clock constraints, specifying power nets, preparing configuration files, and linking standard-cell libraries. Once prepared, the synthesis step translates RTL into a gate-level netlist using Yosys, creating timing and area reports that help understand the circuit’s characteristics.

After synthesis, it’s important to review the generated reports and netlist, checking for warnings, cell usage, and timing slack. This verification ensures that the logic matches the RTL intent. The process of characterizing synthesis results involves analyzing how the design performs in terms of speed, area, and cell utilization, helping decide whether to optimize or re-synthesize with new constraints.

## Day 2 – Floorplanning and Placement Concepts

Floorplanning defines the physical arrangement of logic inside the chip. The utilization factor represents how densely the core area is filled with standard cells, and the aspect ratio determines the shape of the layout. A balanced utilization (typically around 60–70%) ensures enough space for routing and reduces congestion.

Pre-placed cells like memories, analog blocks, or I/O macros are fixed in specific locations before standard-cell placement to ensure proper routing channels and timing predictability. To stabilize voltage during switching, decoupling capacitors are added across the power grid to suppress noise and prevent IR drops.

Power planning is a crucial part of physical design, ensuring all cells receive stable supply voltage. It involves generating metal power stripes and a global grid that distributes power evenly across the die.

During pin placement, input/output pins are assigned to physical locations around the core boundary to optimize routing paths and reduce wire length. Certain areas are reserved as placement blockages to prevent cells from being placed near macros or power structures.

Running a floorplan in OpenLANE involves setting parameters like core area, die dimensions, utilization factor, and macro placements. Once the tool completes the floorplan stage, the resulting DEF file can be viewed in Magic to visualize the layout and confirm correct placement of pins, macros, and power networks.

## Day 3 – Designing Library Cells using Magic and ngspice

The third day transitions from understanding system-level design to diving into the device-level world — where logic gates are formed using transistors. The process begins with simulating a CMOS inverter using ngspice, which models the electrical behavior of PMOS and NMOS transistors. By creating a SPICE deck, parameters like supply voltage, transistor width, and load capacitance are defined to simulate switching behavior. From these simulations, one observes transfer characteristics, determines the switching threshold (Vm), and studies both static and dynamic responses to understand how fast and efficiently the circuit transitions between logic states.

Once simulations are complete, the focus moves to layout creation using Magic. This tool visualizes the physical layers of a CMOS device — such as diffusion, polysilicon, and metal — that correspond to real fabrication steps. The journey starts with active region formation, followed by N-well and P-well creation, gate patterning with polysilicon, and source-drain implantation. After local interconnects and multiple metal layers are added, the layout represents a complete standard cell ready for extraction.

Magic allows the user to generate a SPICE netlist directly from the layout, bridging the gap between the physical and electrical domains. The layout must comply with design rules (DRC) and connectivity checks (LVS). Labs on the Sky130 PDK help explore these layers in detail — understanding how errors like poly spacing violations or missing contacts occur and how to fix them manually. These exercises strengthen the grasp of semiconductor geometry and ensure a clean, manufacturable layout.

By the end of this stage, one gains complete visibility into the fabrication process, from dopant diffusion to metal interconnect formation, effectively linking transistor physics to real silicon design.

## Day 4 – Pre-Layout Timing and Clock Tree Design

This stage shifts back from physical layout to the timing perspective of a chip. Each gate introduces propagation delays that depend on load, input transition, and drive strength — captured in delay tables. These tables are used to generate timing libraries (.lib), which form the foundation of timing analysis and synthesis optimization. By incorporating newly designed cells into synthesis, designers can expand the cell library and enhance performance.

Timing analysis starts with ideal clocks, where skew and jitter are not yet considered. Using OpenSTA, setup and hold timing checks are performed to ensure signals arrive within defined constraints. Parameters like setup time, clock uncertainty, and jitter influence how robust the timing is under variation. OpenLANE allows re-running synthesis or applying timing ECOs (Engineering Change Orders) to fix slack issues by resizing cells or buffering critical paths.

Next comes Clock Tree Synthesis (CTS) using TritonCTS, which builds the real clock network across the design. A clock signal cannot be fanned out directly to thousands of flip-flops — it needs structured distribution through buffers following patterns like H-trees to minimize skew. Power and signal integrity are enhanced using shielding techniques that protect clock lines from crosstalk.

Once CTS is complete, post-CTS timing analysis evaluates the actual clock propagation. Designers check how buffer sizing affects setup and hold times, ensuring data paths and clock paths are balanced. The process emphasizes how clock design is the heartbeat of digital circuits — its quality determines the overall timing health of the chip.

## Day 5 – Routing, DRC, and Final Sign-Off

The final phase of RTL-to-GDS focuses on routing, where the logical connectivity is transformed into physical metal paths on silicon. The TritonRoute tool handles both global and detailed routing, ensuring each net is properly connected with minimal congestion and no rule violations.

Routing relies on algorithms like Lee’s Maze Routing, which systematically explores paths between pins using a grid-based search, ensuring the shortest possible route while avoiding blockages. Once routing is done, Design Rule Check (DRC) validates that all layout geometries follow process constraints — such as spacing, width, and enclosure rules — as defined in the Sky130 PDK.

Parallelly, Power Distribution Network (PDN) creation ensures a robust and uniform delivery of power to all cells. Power straps, rails, and vias interconnect all layers to minimize voltage drop and maintain IR integrity. After PDN and routing, signal nets are finalized and verified for electrical correctness.

TritonRoute’s advanced features enable inter-guide connectivity, multi-layer routing, and pre-processed guide honoring, ensuring optimized wire lengths and reduced congestion. Once routing is complete, the design undergoes post-route timing analysis using OpenSTA to verify final slack and ensure all constraints are met.

The culmination of all these steps produces the final GDSII file — a ready-to-fabricate blueprint of the chip. It marks the completion of the ASIC design cycle, where logic coded in Verilog has successfully transformed into a manufacturable 
