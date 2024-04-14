# Digital VLSI SoC Design and Planning (RTL2GDSII Flow)


## Inception of Open-Source EDA, OpenLANE and Sky130 PDK

### How to Talk to Computers?

**Introduction to Package, Chip, Pads, Core, Die, and IPs**
![Untitled](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/b1a1ce75-aa21-4b71-abdc-a32671d24c04)

Given above is an image of a microcontroller board called Arduino Leonardo which is based on ATmega32u4. Our focus will be on the microcontroller, i.e. ATmega32u4. 

**Package**: The actual chip is contained in the package (the black covering with all the pins). In this case the packge is QFN i.e. Quad Flat No-leads. If we try to open the package we will see a chip sitting in the middle of the package with several wires running from the chip to each pin on the package, which is called Wire Bonding.

![Untitled1](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/05aa3cad-2d0f-4e15-9d2d-71e2779c6c19)

![Untitled2](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/1f48fbc3-335f-47c8-8d0d-f7cbadc760b9)

**Chip**: The main functional part of the die that contains all the circuitry is called the Core and the surrounding area of the core contains pads that helps the core of the die to connect to the pins of the package in order to send and receive electrical signals.

![Untitled3](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/8089a12f-c070-4a78-9a37-4bbc7bcbcadb)

![Untitled4](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/2495f7f0-dc83-4cc6-89fc-2b48515a5211)

**Foundry**: It is highly specialised facility where these semicoductor devices are manufactured. Intellectual Property (IP) refers to a pre-defined and pre-verfied components or blocks that can be integrated in our circuit design. It requires a license with fee in order to use IPs in our design. Macros are pre-defined and pre-verified sections of the chips that are designed by the chip designer.

![Untitled5](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/2dbc50cf-0e9d-4a36-bcc7-14f7743ddd0c)

### Introduction to RISC-V

RISC-V is an open source Instruction Set Architecture (ISA) based on Reduced Instruction Set Computing (RISC) which can be used to create custom processors for various applications. It allows designers to tailor the instruction set according to their requirements.

### From Software Application to Hardware

Suppose we want to run a C program on a particular hardware or particular layout. The C program cannot be directly understood by the hardware, we need to convert the C program in a form which can be understood by the hardware easily. This conversion of C program to a form which is understood by the hardware takes few steps. The C program is first compiled into an Assembly level program which is, in our case, RISC-V instruction set. This Assembly level program is then converted into Machine level progam, i.e. 0s and 1s. Between the Architecture and the Machine level lanuage comes the Hardware Description Langauge (HDL). The Instruction set is implemented using this HDL.

![Untitled6](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/8dca4379-0150-4eac-a37f-164e3cc8d68e)

When we run some Apps or Application Softwares on our laptop, the softwares  go though a block which is called System Software. This block majorly comprises of OS (Operating System), Compiler, and Asembler. 

The OS majorly performs tasks like handling IO operations, allocating memory, performing low level function, etc. but another part of OS does the job of coverting the App into an Assembly level program., which is further converted into a Binary program. The compiler converts the High level program into instructions which depends on the hardware we are dealing with. Then these Instructions are further converted into Machine level program which are then understood by the Hardware. This Assembly level program acts as an Abstract interface between the High level program and the Machine level program.This Abstract interface is the Instruction Set Architecture (ISA) or “Architecture” of Computer. In addition to ISA or Architecture, there is another level called Hardware Description Langauge which actually implemets the Instructions and tells the hardware what to do.

![Untitled7](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/056e48f5-16bc-42fa-94b8-818480ebd0d6)


## SoC Design and OpenLANE

### Introduction to all components of open-source digital ASIC Design
To create an ASIC there are some elements which should be present from the very first day. These are mainly RTL IPs, EDA Tools, and PDK Data.

The **RTL IPs** are described using Hardware Description Langauge (HDL) and defines the behavior of the digital logic in terms of registers, combinational circuits, sequential circuits, etc. They are pre-defined and pre-verifed and can be integrated into our circuit design.

The EDA (Electronic Design Automation) tools are used by the engineers to design, analze, simulate, and verify the circuits that are used to make integrated circuits. 

PDK stands for Process Design Kit. It is a collection of technology files, models, design rules, digital standard cell libraries, I/O libraries, etc. which is provided by the Foundry or Fabrication Lab so that the designer can develop a design which is compatible with the capabilities of the foundry to manufacture that design.

![Untitled8](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/04124951-7c05-4c23-b1bd-0bd9a8533eaa)

We already had some open source EDA tools and RTL designs. What we didn’t have was the PDK. Since the companies that use such PDK sign a Non-Disclosure aggrement with Foundries it was extremely hard to get a PDK. But in recent years there are several open-source PDKs which are easily available and anybody can use them to make whatever they want. Some examples of open-source PDKs are Google’s Skywater 130nm PDK, GlobalFoundries’ 180nm PDK, NC State Universities’ FreePDKTM, etc.

**Is 130nm PDK old and not in use?**

Even though 130nm process node is considered old when comparede to advanced nodes like 5nm or 7nm, it is still used in certain applications where cost-effectiveness and reliability are prioritised over cutting-edge technology. 

**Is 130nm fast?**

In 2004 Intel released a processor named P4EE @ 3.46 GHz which was fabricated using 130nm process node. This processor can run on almost 3.5GHz frequency, so it is quite fast.

### Simplified RTL to GDSII Flow

**Register Transfer Level (RTL)**

It is a level of abstraction in digital design which sits between the high level and the low level or transistor level. It describes the behavior in terms of registers, transfer of data between them, and logical relation between them.

**Graphical Data System II (GDSII)**

It is a binary level file that represents the layers needed to make an ASIC. It reprsents the layout structure of IC in hierarchical structure, which helps to arrange compelx layout into hierarchical levels.

![Untitled9](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/25e4ef21-a908-444b-81e6-48d9c18485ad)

Shown above is a simplified version of RTL to GDSII flow. It majorly contains following steps:

1. **Synthesis**
    - It is a process of converting RTL code, which is written in HDL like Verilog or VHDL, into gate level netlist.
    - It involves the matching of RTL code with the standard cells that are given in library provided by the technology node.
    - It helps in the optimization of the design in terms of area, power consumption, and timing, thus improving the overall performance of the design.
2. **Floor and Power Planning**
    - Floor and Power planning means different things whether you are planning for a single component (or macro) or you are planning for the entire chip. The main objective here is to plan the silicon area to create a robust network for power distribution.
    - In Chip-Floor planning, we partition the chuip die between system building blocks and place the I/O pads.
    - In Macro-Floor planning, we define the dimension, pin location, and rows and routing tracks.
    - In Power planning, the power network is constructed. The chip is actually power by the vertical and the horizontal lines that run throughout the chip. The parallel structures are meant to reduce the resistance.
3. **Placement**
    - This stage involves the placement of standard cells, present in the netlist, in the core of the die.
    - It optimizes the overall design.
    - It involves the placement of the cells on the floorplan rows, aligned close to each other to reduce the interconnect delay also to enable successful routing afterwards.
    - It is usually done in 2 steps: Global placement, followed by Detailed placement.
    - Global placement tries to find the optimal placement for all cells. Such positions may not be legal, so cells may overlap or off-rows.
    - In Detailed placement the placement obtained by the Global placement are altered to make it legal.
4. **Clock Tree Synthesis**
    - It involves the generation of a clock distribution network in order to deliver the clock signal required by the sequential elements with minimum skew (arrival of clocks to different components on different times) and minimum latency.
    - It can have different structures such as Fishbone, H-tree, X-tree, Multi-level tree.
5. **Routing**
    - It is the process of creating interconnection wiring between different components using the available metal layers in an integrated circuit.
    - Skywater 130nm PDK defines 6 layers in which the lowest, Titanium Nitride, layer is called as the Local Interconnect Layer and the remaining 5 layers are Aluminium layers.
    - A routing grid is form with the metal tracks. As the routing is huge, Divide and Conquer approach is used for routing.
    - Global Routing: It generates routing guides.
    - Detailed Routing: It uses the routing guides to implement the actual wiring.
6. **Sign Off**
    - After finishing the routing the final layout can be generated which undergoes multiple Sign-Off checks.
    - Design Rule Check (DRC): It is used to check whether the design follows the rules and manufacturing constraints defined by the foundry.
    - Layout Versus Schematic (LVS): It is a process which is used to compare the layout of the integrated circuit with its corresponding schematic to make sure the design is correct.
    - Static Timing Analysis (STA): It is a process used to analyse timing behavior of the circuits and to make sure they meet all the timing constriants and the circuits are being run at the designated frequency.

### Introduction to OpenLANE and striVe Chipsets

**OpenLANE**

OpenLANE is designed for implementing Application Specific Integrated Circuits (ASICs). It is based on several other open-source projects such as OpenROAD, Magic, Yosys, etc. It was started as an open-source flow for a True Open Source Tape-out experiment.

**striVe SoC**

striVe is a family of open everything SoCs (System on Chips) with open PDK, open EDA, and open RTL.

![Untitled10](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/0c1fdf6b-03fa-4415-9638-239851be4c53)

![Untitled11](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/2bfdfab2-294e-489c-87c2-5c391a3c8893)

![Untitled12](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/bfa63a85-c073-4e78-80de-25a3aa9721c8)

The main goal of OpenLANE is to produce a clean GDSII with no human intervention (no-human-in-the-loop). An clean means that there should be no LVS, DRC, or Timing violations. OpenLANE is tuned for Skywater 130nm Open PDK but it also supports XFAB180 and GF130G. It can be used to harden (to make GDSII) Macro and Chips. It has two modes of operation- Autonomous or Interactive. Autonomous mode is a push button flow, i.e. we configure the flow, we run the flow and then we wait for the results. On the other hand, in Interactive mode we can run commands one at a time so that we can understand the flow and make changes accordingly. It also a feature called Design Space Exploration which helps to find the best set of flow configurations.


## Getting Familiar With Open-source EDA Tools
### Design Preparation
Our first task is to run OpenLANE and in order to do that we need to enter into some directories and then run some commands.

````bash
# First we change the current directory to openlane directory
# using 'docker' to invoke openlane flow docker
docker
````
````bash
# Invoking the openlane flow in interactive mode
./flow.tecl -interactive

# Invoking necessary packages
package require openlane 0.9

# Running a design
prep -design picorv32a

# Running synthesis
run_synthesis
````
![capture](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/8b72bcef-4614-415c-a58b-8ad794b946cc)

![Capture1](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/44137553-7f6b-47ac-b28c-b6c26c151667)

![Capture2](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/34c6b573-5990-4193-8c0e-133c2d8cbe3b)

![Capture3](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/3ce64e95-5b33-4c0d-b50b-b82a99e8e251)

![Capture4](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/235e8876-7347-4c12-aba7-cc402329dba9)

![Capture5](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/5b922d81-76a6-4706-8908-35efde4e4969)

![Capture6](https://github.com/Manan2902/Digital-VLSI-SoC-Design-and-Planning-RTL2GDSII-Flow-/assets/97438921/6d3e298b-27ff-4e60-8533-659340c8f51d)

**Calculation of Flop Ratio**

Flop Ratio = 1613/14876 = 0.108429685

Percentage = 10.8429685%
