## Advanced Physical Design Using OpenLANE/Sky130

![Advanced-Physical-Design-using-OpenLANE_Sky130_1](https://user-images.githubusercontent.com/86367130/123914575-2b087700-d99d-11eb-819f-feb44e307b87.png)

# Table of Contents

1.[Inception of open-source EDA,Open LANE and Sky130 PDK](#first-day1)

 + How to Talk to Computers
     + [Introducion to QFN-48 Package, chips, pads, core, die and IPs](#first-content-1)
     + [Introduction to RISC-V](#first-content-2)
     + [From Sotware Applicatins to Hardware](#first-content-3)
 
 + SOC Design and Open LANE
     + [Simplified RTL2GDS Flow](#first-content-4)
     + [Introduction to Open LANE and Strive Chipsets](#first-content-5)
     + [Introduction to Open LANE Detailed ASIC Design Flow](#first-content-6)

+ Get famiier to Open Source EDA Tools
    + [Open LANE Directory Structure in Detail](#first-content-7)
    + [Design Preparation Step](#defines)
    + [Review Files After Design Prep and RUN Synthesis](#defines)
    + [Seps to characterize Synthesis Results](#defines)

2.[Good FloorPlan vs BadFloorPlan and introduction to library cells](#day2)

   + [Chip Floor Planning Considerations](#second-content-1)
   + [Library Binding and Placement](#second-content-2)
   + [Cell Design and Characterizaton Flows](#seccond-content-3)
   + [General Timing Charaterization Parameters](#scond-content-4)


    

# <a name="first-day1"></a>Inception of open-source EDA,Open LANE and Sky130 PDK

+ <a name="first-content-1"></a>Introducion to QFN-48 Package, chips, pads, core, die and IPs

QFN-48(Quad Flat No-Leads) Package:
     It consists of 48 pins with 7mm * 7mm size with the pins cnnected to the chip placed inside the package.
     Pads-Send the signals inside and outside the chip through pads.
     Chip- It is the place where all the digital Logic is fabricated with a combination of various gates.
     1.Foundary IPs(Intellectual Property)- Some amount intelligence techniques are used to build them
     2.Macros-A Pure Digita Logic
     
+ <a name="first-content-2"></a>RISC-V Architecture
      When an application or C-program is to be run on a Layout, it a first complied in its Assembly Language Program that is a RISC-V Assembly Language and then converted to Machine Language Program in terms of zeros and ones which is then run on a Layout.
      This RISC-V specification is implemented using a RTL Code for eg: picorv32 cpu core.

+ <a name="first-content-3"></a>From Software TO Hardware
![from software to hardware](https://user-images.githubusercontent.com/86367130/123951827-d546c580-d9c2-11eb-972e-4febab5c381f.PNG)

The instruction set produced by the compiler is the Instruction Set Architecture or Architecture of the computer. In this case it is RISC-V Architecture.HDL acts as an interface between RISC-V Architecture and hardware.

+ <a name="first-content-4"></a>Simplified RTL2GDS Flow
![image](https://user-images.githubusercontent.com/86367130/123952777-19869580-d9c4-11eb-8cbd-729b0e8aeeda.png)

Synthesis: Converts RTL design to a circuit out of the components from standard cell library(SCL).Standard cells have regular layout. each cell has different views/models.
For eg: Liberty View-This include power and delay models

Floor and Power Planning: Plan the silicon area and robust power distribution to power the circuits.
      Chip Floor Planning: Partition the chip die between different system building blocks and place the I\O Pads.
      Macro-Floor Planning:Dimensions, pin-locations,rows and routing tracks are defined.
      Power Planning: Power network is constucted where chip is power by multiple Vdd and Groung Pins.These pins ar connected to components through vertical and horizontal metal       straps.These use upper metal layers which are wider and have lower resistance 
      
Placement: Place the cells on the floor plan rows, aligned with the sites.Closser cells should be placed adjacent to reduce interconnect delay 
2 Steps:
Global Placement- Find optimal posiiton for all cellsand these are not legal so cells may overlap.
Detailed Placement-The detials obtained from Global Placement are minimally altered

Clock Distributon Network:
  -To deliver clock to all sequential circuis (eg;FFs)
  -With minimum skew-Can be ensured by H-Tree or X-tree
  
 Routing:Implement interconnect using metal layers.This requires to enquire a valid horizontal and vertical patterns to implement the nets. PDK defies the thickess, pitch, width of the metal layer and also the vias required to connect different metal layers.
 
 A Divide and Conquer Approach is used
    -Global Routing:Generated the routing guides
    -Detailed Routing:Used the guide to implement the actual wiring
    
 After Routing, Layout is confired through:
 Design Rule Checking(DRC)-Makes sure that the final Layout adheres to all Design Rules
 Layout Vs Schematic(LVS):Ensured that the final Layout matched the Gate-Level Netlist
 
 Timing Verification(STA): To ensure all timing constraints are met.
 
 + <a name="first-content-5"></a>Introduction to Open LANE and Strive Chipsets
 
 It is an open source tool
 
 Strive- Familyof open evrything SOCs
      Open EDA,Open PDK, Open RTL
      
 Open Lane -Creates a clean GDSII flow with no human intervention that is, there ar no DRC and LVS violations. Can be used to harden MACROS and chips.
 
 + <a name="first-content-6"></a>Introduction to Open LANE Detailed ASIC Design Flow

![image](https://user-images.githubusercontent.com/86367130/123957764-cc0d2700-d9c9-11eb-8afd-40089d6f0136.png)

Synthesis Exploration-Gives us the analysis of area and delay of the design.
Design Exploarion -Runs regression testing and comparision of different designs.
Design For Test:
    + Scan Insertion
    + Autmatic Test Pattern generation(ATPG)
    + Test Patterns Compaction
    + Fault Coverage
    + Fault Simulation
    
Physical Implementation- Is done through Open ROAD
Logic Equivalence Check -Done through yosys
Dealing with Antenna Rules Violations-Metal wires can act as an antenna so their size should be limited. This is ensured by the router.
    -Bridging- Attaches higher metal layer to intermediary.
    -Add antenna diodes to leak away the charges provided by SCL
    
Magic is used for DRC
Magic and Netgen ar used by LVS
    
+ <a name="first-content-7"> Open LANE Directory Structure in Detail
 
 Diretory Structure in 
 
 Invoke OpenLANE
 
 ![image](https://user-images.githubusercontent.com/86367130/123965735-5f4a5a80-d9d2-11eb-8c22-e8473462601d.png)

 interactive- Instead to running the completeflow at once it will run the process step by step
 
 Input all the Packges that are required to run the flow:
 
 ![image](https://user-images.githubusercontent.com/86367130/123966857-6a51ba80-d9d3-11eb-879e-097cd9978e8b.png)

# <a name="day2"> Good FloorPlan vs BadFloorPlan and introduction to library cells
 
+ <a name="second-content-1"> Chip Floor Planning Considerations
 
 1. Utilization factor:
 
        If for example,the following logic is taken:
 
 ![Capture](https://user-images.githubusercontent.com/86367130/123980231-00d7a900-d9df-11eb-9b10-48618fefc413.PNG)
 
 If the area of the single unit is 1sq. unit,then if all the gatesare clubbed together then the total minumum area of the Netist will be 4sq. units.
 If the netlist occupies 100 percent area of the core then the Utilization Factor will be 100 percent.
 
 ![image](https://user-images.githubusercontent.com/86367130/123981298-dafed400-d9df-11eb-9331-59675e44d954.png)
 If Utilization Factor is 1, then the core is completly occupied and any extra cells cannot be accomodated in the core area.Ideally ,utilization factor should 0.5 or 0.6.
 
 
 2.Aspect Ratio:
     It is the ratio of height and width of the core.
 
 If the aspect ratio is 1 , then chip is square-shaped otherwise it is of rectangle shape.
 
 3.Define Location of Pre-Placed Cells:
      Pre-Placed Cells-Are Macros or IPs with a complex logic and can be used multiple times.
 
![image](https://user-images.githubusercontent.com/86367130/123992238-12be4980-d9e9-11eb-9981-ee256779be78.png)

 if we take an example of Logic and cut them in 2 blocks
 ![image](https://user-images.githubusercontent.com/86367130/123992452-3ed9ca80-d9e9-11eb-87db-75a7ce6ceb42.png)
 We consider both the blocks as Blaxk boxes which are implemeted differently and the sections are invisible to the Netlist and Ports of the blocks will serve as I/O pins
 
 ![image](https://user-images.githubusercontent.com/86367130/123993441-0d153380-d9ea-11eb-9e42-5ca30bfd8f28.png)
 
 Advantage- It helps in replications of the blocks.
 
 Similarly blocks as memory,clock gating cells can be used multiple times and such cells are pre-placed cells.The arrangement of these IPs is known as Floor Planning.
 These IPs have user-defined locations and therefore they ae placed before automated placement and  routing and are called as pre-placed cells.
 Automated placement and routing cells place the remaining cells and pre-placed cells are not touched by the tools.
 
 4. de-Couping Capacitors- Pre-Placed cells are surrounded by the De-Coupling Capacitors. So, when there is a switch in the Logic level, these capacitors act as charging /discarging element for the same. From Logic 0-1, they get charged and from 1-0 they get discharged.
 
 When the power supply through there is a drop in the wire. This voltage drop can be modeled as combination to inductors and resistances across the wire.
 
 ![image](https://user-images.githubusercontent.com/86367130/124006427-df36eb80-d9f7-11eb-987a-f1c5bab9c5fb.png)
 
 If Logic lies between Vih and Voh, then it is treated as Logic-1 
 If Logic Lies between Vil and Vol, then it is treated as Logic-0
 Other the region is undefined.The voltage level should not be in this region.
 
 So, the Logic High or Low should be within the Noise Margin for correct detection.
 We can solve the problem of voltage drop across wire with the help of De-Couping Capcitors which are charged to the supply voltage and De-Couples the main circuit from the main supply.
 
  ![image](https://user-images.githubusercontent.com/86367130/124007675-34bfc800-d9f9-11eb-80e4-987a0244ef27.png)
 
 When there is a switching activity , the de-coupling capacitor looses its charge to the circuit and rest of the time it uses to replenish its charge.
 
5.Power Planning
   If the Macros are repeated multiple times , then there will be current demand for each Macro.We musst make sure that signal does not loose its integrity while travelling from Driver to Load. So, the Line should get necessary supply from the power.
 
 ![image](https://user-images.githubusercontent.com/86367130/124008300-e828bc80-d9f9-11eb-97e6-f3a05b0c61ec.png)
 
 Now if we assume the line to be 16-bit bus and is connected to the inverter.Then the Logics will change their state and the process of discharging they will witness a bump in Ground tap Point. If the size of the bump exceeds the Noise Margin Level, then Logic might go wrong.Alternatively, in case of charging Voltage Droop can be observed.
 
 ![image](https://user-images.githubusercontent.com/86367130/124009057-c3811480-d9fa-11eb-9f33-8d8a3c5f6631.png)
 
 SO, the solution is to have multiple power supply sources.Having multiple vertical and horizontal lines for supply voltage and ground connections across the core.
 
![image](https://user-images.githubusercontent.com/86367130/124009597-6d60a100-d9fb-11eb-8891-e1d026b897aa.png)
 
 
6. Pin Placement
 
 The connectivity information between gates is coded using VHDL/Verilog Code known as Netlist.
 Pins are placed between the area of Core and Die. The ordering of the Ports are random placed near to the Gates they are connected to.
 Front end team decides the Netlist and back end team decides Pin Placement.
 Clock drives lots of Flip-FLop So their pin size should be large for least resistance.
 
 ![image](https://user-images.githubusercontent.com/86367130/124011498-b6b1f000-d9fd-11eb-811b-688ae2d4f5d6.png)

Logical Cell Placement Blockage- Blocks area between Core and Die to ensure that Automated Placement and Routing Tool does not place any cell in the area.
 
 Labs
 
 + <a name="second-content-2"> Library Binding and Placement
 
 1. Placement and Routing
   
 Step-1 Bind the Netlist with Physical cells
 The shapes of the gates in real are square or rectangular boxes and the library has all the gates with timing information of the gates. After deciding proper shapes and sizes.
 
 Step-2 Place the Netlist on the Floor Plan
 
 The gates are placed near to their input-output port connections.
 Sometimes they are at a higher distance
 
 Estimate the Wire length and capacitances. To maintain the signal integrity we will insert ectra repeaters or bufferes which recondition the original signal with loss of area.
 
 Based on the timing analysis and slew we see if there is a need of repeater or not.
 
 
 


 
 


 


 
 

 
 
 

      

 
    




 
 
 
    
























































































