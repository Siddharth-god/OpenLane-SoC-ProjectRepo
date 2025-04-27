![Alt text](images/img0.1.png)

# Dgital VLSI SoC Design and Planning

### Expand to see the process for shared folders in linux and windows
<details>
  <summary><h3>Expand / Creating Shared Folder<h3></summary>

  ## Setting Up Shared Folder Between Windows and Linux VirtualBox

While working on my RISC-V SoC project in the Linux VirtualBox environment, I needed to share screenshots and files from my Linux VM to my Windows system. Here’s exactly what I did, step by step:

1. __Creating a Shared Folder in Windows__

   
    First, I created a separate folder in my Windows system to keep things organized and avoid mixing with other screenshots.
    I planned to use this folder whenever I wanted to move files between Linux and Windows.

    ![Alt text](images/Sf-ss1.png)


2. __Configuring Shared Folder in VirtualBox Settings__
   

    - Then, I configured this folder as a shared folder in VirtualBox:
    I completely powered off the virtual machine (not saved state).
    Then I opened VirtualBox > Settings > Shared Folders for my VM and added the Folder.

    ![Alt text](images/Sf-s1.png)
    
    - #### Click on the small "plus file" icon on the right.

    ![Alt text](images/Sf-s2.png)
    
    - #### Then do the simple process shown in the image below to create the folder.
    - #### "MAKE SURE YOUR VM IS COMPLETELY SWITCHED OFF AND NOT IN SAVED STATE"

    ![Alt text](images/Sf-s3.png)

    - #### Just click ok and there you go.

    ![Alt text](images/SF-img.png)

    - #### As you can see there is a folder named LinuxSnaps.

#### Upto this everything should be easy : Next if the problem occurs as it did in mine.

3. __Booting into Linux & Checking the Folder__


    I added the folder but i wasn't been able to see it in my linux windows 
    SO i used this command to create the directory but it failed by saying file already exist.
    To proceed further i needed to make sure that VirtualBox Guest Additions are installed in my Linux VM. This is necessary for shared folders to work.

    Then i used this commands:
    sudo apt update
    sudo apt install virtualbox-guest-utils
    Reboot the Virtual Machine: After installation, reboot your Linux VM by typing:
    sudo reboot
    
    After starting the Linux VM again, I ran:
    ![Alt text](linux_images/img4.png)
    ![Alt text](linux_images/img3.png)


4. __Fixing the Permission Denied Issue__


    After that process i got the folder running in my linux but it was denying the permission when i tried to move the screenshot in that folder
    ![Alt text](linux_images/img2.png)
    So I had to run "sudo usermod -aG vboxsf $USER" this is the command to become the user and access the folder. I dont have the whole process as in forgot to take the SS. But all you need is COMMAND.
    ![Alt text](linux_images/img1.png)

    Then i used "sudo reboot"
    to reboot the server and make it properly work


5. __Accessing the Shared Folder and Copying Files__


    And all set now i can access my linux files in my WINDOWS as well.
    ![Alt text](linux_images/img5.png)

    SO This is how i created the shared folders for my conveninvce i just shared it, so if anyone wants to do it in future they can atleast have something to refer.

</details>



<hr>

## Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK

<details>
  <summary><h3>Theory - Click "Theory" to expand.<h3></summary>

- ##### Packiging
    In The Embedded Boards we can see the chip is implanted. As we think it's a real chip but wait that's not it - IT'S NOT A ACTUAL CHIP , BUT IT'S ONLY A PACKAGE OR WE CAN SAY A CASE TO SAVE THE ACTUAL CHIP WHICH IS INSIDE OF THAT PACKAGE. The actual chip is made of silicon and it cannot be touched by bare hands so that's why it's packed with plastic layer. The connections from package is fed to the chip by __WIRE BOUND__ method which is none other than basic wired connection.

    ![Alt text](images/img1.png)

    ![Alt text](images/img2.png)

    ![Alt text](images/img3.png)


- ##### Chip
    Now, Inside the chip, all the signals from the outside to the chip and from inside are passed through PADS. The area bound by the pads is CORE where all the digital logic of the chip is placed. Both the core and pads make up the DIE which is the basic manufacturing unit in regards to semiconductor chips.

    ![Alt text](images/img4.png)


- ##### FOUNDRY 
    It is the place where the semiconductor chips are manufactured, This Foundries should be _CLEAN_ The word clean here dosen't mean like what we do in our house, it should not have even single particle of dust or any human part like hairs. FOUNDRY IP's are Intellectual Properties based on a specific foundry and these IP's require a specific level of intelligence to be produced whereas, repeatable digital logic blocks are called MACROS.

    ![Alt text](images/img5.png)


- ##### ISA (Intruction Set Architecture)
    A C program which has to be run on a specific hardware layout which is the interior of a chip in your laptop, there is certain flow to be followed.
    Initially, this particular C program is compiled in it's assembly language program which is nothing but RISC-V ISA (Reduced Instruction Set Compting - V Intruction Set Architecture).
    Following this, the assembly language program is then converted to machine language program which is the binary language logic 0 and 1 which is understood by the hardware of the computer.
    Directly after this, we've to implement this RISC-V specification using some RTL (a Hardware Description Language). Finally, from the RTL to Layout it is a standard PnR or RTL to GDSII flow.

    ![Alt text](images/img6.png)


    For an application software to be run on a hardware there are several processes taking place. To begin with, the apps enters into a block called system software and it converts the application program to binary language. There are various layers in system software in which the major layers or components are OS (Operating System), Compiler and Assembler.
    At first the OS outputs are small function in C, C++, VB or Java language which are taken by the respective compiler and converted into instructions and the syntax of these instructions varies with the hardware architecture on which the system is implemented.
    Then, the job of the assembler is to take these instructions and convert it into it's binary format which is basically called as a machine language program. Finally, this binary language is fed to the hardware and it understands the specific functions it has to perform based on the binary code it receives.

    ![Alt text](images/img7.png)

    For example, if we take a stopwatch app on RISC-V core, then the output of the OS could be a small C function which enters into the compiler and we get output RISC-V instructions following this, the output of the assembler will be the binary code which enters into your chip layout.

    ![Alt text](images/img8.png)

    For the above stopwatch the following are the input and output of the compiler and assembler.

    ![Alt text](images/img9.png)

    The output of the compiler are instructions and the output of the assembler is the binary pattern. Now, we need some RTL (a Hardware Description Language) which understands and implements the particular instructions. Then, this RTL is synthesised into a netlist in form of gates which is fabricated into the chip through a physical design implementation.

    ![Alt text](images/img10.png)

    There are mainly 3 different parts in this course. They are:
    RISC-V ISA
    RTL and synthesis of RISC-V based CPU core - picorv32
    Physical design implementation of picorv32

    ![Alt text](images/img11.png)

    Open-source Implementation
    For open-source ASIC design implemantation, we require the following enablers to be readily available as open-source versions. They are:-
    RTL Designs
    EDA Tools
    PDK Data
    Initially in the early ages, the design and fabrication of IC's were tightly coupled and were only practiced by very few companies like TI, Intel, etc.
    In 1979, Lynn Conway and Carver Mead came up with an idea to saperate the design from the fabrication and to do this they inroduced structured design methodologies based on the λ-based design rules and published the first VLSI book "Introduction to VLSI System" which started the VLSI education.
    This methodology resulted in the emergence of the design only companies or "Fabless Companies" and fabrication only companies that we usually refer to as "Pure Play Fabs".
    The inteface between the designers and the fab by now became a set of data files and documents, that are reffered to as the "Process Design Kits (PDKs)".
    The PDK include but not limited to Device Models, Technology Information, Design Rules, Digital Standard Cell Libraries, I/O Libraries and many more.
    Since, the PDK contained variety of informations, and so they were distributed only under NDAs (Non-Disclosure Agreements) which made it in-accessible to the public.
    Recently, Google worked out an agreement with skywater to open-source the PDK for the 130nm process by skywater Technology, as a result on 30 June 2020 Google released the first ever open-source PDK.

    ![Alt text](images/img12.png)


    ASIC design is a complex step that involves tons of steps, various methodologies and respective EDA tools which are all required for successful ASIC implementation which is achieved though an ASIC flow which is nothing but a piece of software that pulls different tools togather to carry out the design process.
    ![Alt text](images/img13.png)


    OpenLANE Open-source ASIC Design Implementation Flow
    The main objective of the ASIC Design Flow is to take the design from the RTL (Register Transfer Level) all the way to the GDSII, which is the format used for the final fabrication layout.
    ![Alt text](images/img14.png)

    Synthesis is the process of convertion or translation of design RTL into circuits made out of Standard Cell Libraries (SCL) the resultant circuit is described in HDL and is usually reffered to as the Gate-Level Netlist.
    Gate-Level Netlist is functionally equivalent to the RTL.
    ![Alt text](images/img15.png)

    The fundemental building blocks which are the standard cells have regular layouts.
    Each cell has different views/models which are utilised by different EDA tools like liberty view with electrical models of the cells, HDL behavioral models, SPICE or CDL views of the cells, Layout view which include GDSII view which is the detailed view and LEF view which is the abstract view.
    ![Alt text](images/img16.png)

    Chip Floor Planning
    ![Alt text](images/img17.png)

    Macro Floor Planning
    ![Alt text](images/img18.png)

    Power Planning typically uses upper metal layers for power distribution since thay are thicker than lower metal layers and so have lower resistance and PP is done to avoid electron migration and IR drops.
    ![Alt text](images/img19.png)

    Placement
    ![Alt text](images/img21.png)

    Global placement provide approximate locations for all cells based on connectivity but in this stage the cells may be overlapped on each other and in detailed placement the positions obtained from global placements are minimally altered to make it legal (non-overlapping and in site-rows)
    ![Alt text](images/img22.png)

    Clock Tree Synthesis
    ![Alt text](images/img23.png)

    Clock skew is the time difference in arrival of clock at different components.
    Routing
    ![Alt text](images/img24.png)

    skywater PDK has 6 routing layers in which the lowest layer is called the local interconnect layer which is a Titanium Nitride layer the following 5 layers are all Aluminium layers.
    stackup

    Global and Detailed Routing
    ![Alt text](images/img25.png)

    Once done with the routing the final layout can be generated which undergoes various Sign-Off checks.
    Design Rules Checking (DRC) which verifies that the final layout honours all design fabrication rules.
    Layout Vs Schematic (LVS) which verifies that the final layout functionality matches the gate-level netlist that we started with.
    Static Timing Analysis (STA) to verify that the design runs at the designated clock frequency.
    ![Alt text](images/img26.png)
  

</details>

<hr>

Checking what tools and files we have access to and what we are going to work with in future. 
These are some tools we have given access to. You can see in designs and there are "pdks" and "openlane" as well, let me show you. 

![Alt text](linux_images/lec1-img1.png)
![Alt text](linux_images/lec1-img2.png)

<hr>

# Starting with Openlane

## Implementation

<h4> Objectives <h4>

##### 1.Run Desing Synthesis for "picorv32a" using OpenLANE & generate necessery outputs.
##### 2.Calculate the Flop ratio

- __Formula__ 

```
                Number of D Flip Flops 
Flop Ratio =   —————————————————————————
                Total Number of Cells

Percentage of DFF's = Flop Ratio × 100
```

<hr>

##### 1.Run Desing Synthesis for "picorv32a" using OpenLANE & generate necessery outputs.
#### Commands to invoke the OpenLANE flow and perform synthesis.
```bash
# As we work in Openlane we have to first change our directory to openlane flow directory. Just type -

cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker ='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# aliased basically means creating the short version of the command so we don't have to type very long command all the time.
# So using only 'docker' we can invoke the OpenLANE flow docker sub-system.

command - docker

# After running docker the "bash" directory will get opened where we're gonna work
```
<hr>

```bash
# As we've entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now use this command to open the required package
package require openlane 0.9

- Processess to be done Before Synthesis -
# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'

# Design set-up stage - We need to setup a file system specific to the flow as we perform the steps we are going to fetch files from the perticular folder/location. 

# This step helps the openlane to fetch the information from single file instead fetching it from two different LEF files. So were going to merge the two files together which are - lep.lep and Tlep.

prep -design picorv32a

# Now that Preperations are done , we can run synthesis using following command
run_synthesis

# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```
<hr>

1. Preperation Step

![Alt text](linux_images/Day1-sec3-lec2-img1.png)

![Alt text](linux_images/Day1-sec3-lec2-img2.png)

![Alt text](linux_images/Day1-sec3-lec2-img3.png)

You can see there is a new folder is added named _"runs"_

Now the preperation step is complete.

- If we open the file that we created it'll give us the "Date file" which stores the
data such as "results","report".

![Alt text](linux_images/Day1-sec3-lec3-img1.png)

<hr>

2. Synthesis Process 

![Alt text](linux_images/synthesis-img1.png)

![Alt text](linux_images/synthesis-img2.png)

<hr>

- ### Flop count 


![Alt text](linux_images/D1-Sk3-Lec5-img1.png)

![Alt text](linux_images/D1-Sk3-Lec5-img2.png)

```
               1613
Flop Count =  ——————— = 0.108429685
               14876

Percentage of DFF's = 0.108429685 x 100 = 10.8429685 %
```

<hr>

### Here you can see the RESULT and the REPORT by running these commands and the files 
### inside are the whole data of it. 

![Alt text](linux_images/D1-Sk3-Lec5-img3.png)

![Alt text](linux_images/D1-Sk3-Lec5-img4.png)
.
.
.
.
.
### Day 1 comes to the end with this and we are moving on to the next Day from here.

<hr>

## Day 2 - Good floorplan vs bad floorplan and introduction to library cells

<details>

<hr>
  <summary><h3>Theory - Click here to understand the process</h3></summary>

### 1. Define Height and width of core and Die.

- Basic netlist
![Alt text](images/S1.jpg)

FF - flip flop | A1 & O1 - Standard cells like AND, OR, INVERTER.

- We're taking this one netlist you can see in the image above as an example
Netlist define the connectivity between all the components 

![Alt text](images/S2.jpg)

- Each cell has Area of 1sq unit and when we connect them together,
The Total Area is - 4 squnit. 

![Alt text](images/S3.jpg)

- As you can see in the image the circular area is a silicon wafer and it has multiple cells on it and these cells are called _Die_ and the Die is made up of _Core_

__core__ : A core is a the section of the chip where the fundamental logic of the design is placed.

__Die__  : A die consist of core is a small semiconductor material speimen on which the fundamental circuit is fabricated.The DIE encapsulates the core.

- How to arrive on dimensions ? 

![Alt text](images/S4.jpg)
```
                          Area occupied by Netlist
Utilization Factor =    —————————————————————————————
                          Total Area of the core
```

When the Utilization Factor is equal to 1.

It means that our core is fully occupied and we canno't add any extra cell,
so normally we go for 50% or 60% Utilization and not 100%

```
                 Height
Aspect Ratio =  ────────
                 Width
```

- When the aspet ratio is one, it signifies that the core is __SQUARE__ Shaped and whenever the Aspect ratio is not 1 it signifies that the chip is __Rectangular__.

- When the Utilization factor is anything other than the 1 then it suggests that the core is not fully utilized and we can add extra cells to it.

- Here you can see the diagram explanation for better understanding.
![Alt text](images/S5.jpg)
![Alt text](images/S6.jpg)
![Alt text](images/S7.jpg)

- Now for example if we put our 2x2 core (netlist) into the 4x4 chip then the Utilization Factor = 0.25.

```
                      Area occupied by Netlist    2x2      4
Utilization Factor = ————————————————————————— = ————— = ————— = 0.25
                      Total Area of the core      4x4      16
```

- This suggest that the area occupied by the initial Netlist is 0.25 and 0.75 Area is free for Optimization.
- The Netlist is completely connected by ideal wires which don't have any shape and size.

![Alt text](images/S8.jpg)

<hr>

### 2. Define Locations of the _Pre-Placed_ or _Macros_ cells.

<hr>

- As you can see in the image below - it's a chip and we have to decide some specific location for our logic on this chip. Which once decided we cannot change the location  later.
![Alt text](images/S9.jpg)

- This logic can be any complex circuit ex - Complex MUX or Any complex logic which will perform very huge tasks. This logic/circuit is the main function of our core and we implement this circuit only once and use it multiple times [i am talking about it's functionality not the actual logic]
- As you can see in the image below we have taken one logic as an example.

- We can split/cut this main circuit into two blocks and implement them seperately.

![Alt text](images/S11.jpg)

- Now, we can extend the pins of the input side and output side of the circuit after setteling it inside two blocks. As you can see in the image below and what is going on inside is not visible from outside you can see only pins and not the content inside the blocks.

![Alt text](images/S13.jpg)

- Now, we seperate this two blocks and we can have two seperate Blocks/circuits which have output and input pins, You can see it in the image below.

![Alt text](images/S14.jpg)
- These two circuits behave as different circuits.
- And instead of implementing the whole logic together which is very complex, we implement them in parts which makes it very easier and faster and after that we can connect this blocks together using their input and output ports multiple times.
- And we use this blocks on the top NETLIST by implementing them _Only Once_.

![Alt text](images/S16.jpg)

- This blocks have predefined locations on the main chip and that's why this are called _Pre Placed Cells_, As these cells get placed before any automated placing or routing.
- Once the location of this cells is fixed they cannot be moved or touched by any automated routing tools. 
- This blocks are also called MACAROS or IC'S.

![Alt text](images/S17.jpg)

<hr>

### 3. Sorrounding Pre-Placed cells with _Decoupler Capacitor_.

<hr>


- #### Conditions needs to be fulfilled.

![Alt text](images/S18.jpg)

- When the circuit is switching from logic 0 to logic 1 the capacitance between(Inside the circuit on the right side) the circuit needs to be charged by the vdd.
- And, When the circuit is switching from logic 1 to logic 0 the capacitance between the circuit needs to be discharged by the vss(ground).
- But, The problem is that the power supply is connected to the circuit using the wires and wires have some resistance and inductance to them so multiple times voltage drop happens which is not ideal for our condition.
- When the voltage lies between Vil and Vol the logic becomes 0.
- When  the voltage lies between Voh and Vih the logic becomes 1.
- But, When the logic is inside the undefined region which Vih and Vil - it can go either way [logic 1 or logic 0] and we don't certainly know what it'll be and that's not good for our circuit. As you can see in the image 
[ The image might be blurry so apologies for that.]

![Alt text](images/S19.jpg)

- So here comes the Decoupling capacitor.

- What are _Decoupling Capacitors_ ?
---> The __Decoupling Capacitors__ are local electrical energy reservoirs which provides a bypass path for transient currents. They are placed between power line and ground to the circuit that current is to be provided. When used as decoupling capacitors, they oppose quick changes of voltage. If the input voltage suddenly drops, the capacitor provides the energy to keep the voltage stable.

- It basically means that the power supply is connected to the circuit using the wires and wires have some resistance and inductance to them so the current sometimes drop which is not ideal for our condition so to remove this problem we use these capacitors which are charged and when there is need for the power to the circuit they join the game. They get discharged after using the power and then they get pre charged from power supply.

![Alt text](images/S20.jpg)

In the image above you can see there is a capacitor in parallel to the circuit.

![Alt text](images/S21.jpg)

And This we we make sure our circuit is well designed for working efficiently.

<hr>

### 4. Power Planning

In the power planning we will see that how the power planning is done for more efficient chip design.

As we can see this is our circuit which we are designing and we call it "MACRO"

![Alt text](images/S23.png)

Now this circuit, which we also call MACRO we consider it as a black box.

![Alt text](images/S24.png)

Now this macro is being repeated many times in the chip we take it as a example let's say it's repeating 4 times in our circuit.

![Alt text](images/S25.png)

If you can see in the above image there is driver and load in the circuit and there is a 16-bit BUS which connects the driver OUTPUT to the LOAD and we have to make sure that the signal from driver reaches to the load with same capacity.

Now the problem is that - 

We are using the same power supply to every black box or we can say MACRO as a VDD. Here the blue line shows the VDD and the grey line shows the VSS/Ground. 

Now let's suppose the 16-bit BUS is sending the signal as logic "0" or logic "1" so this BUS has to retain this value until this value reaches to the LOAD. 
In the circuit we can see that every block or MACRO has a decoupling capacitor but the BUS does not its because it is not physically possible to use decoupling capacitor in the circuit everywhere.

![Alt text](images/S26.png)

Now this 16-bit BUS is made up of 16 capacitors and each capacitor is connected to the VDD or Ground based on the value which DRIVER is sending to the LOAD. And this value is then connected to the INVERTER in the end which inverts all the values which the BUS is carrying.

![Alt text](images/S27.png)

- WHAT PROBLEM IT CREATES ?

- As the O/P reaches to the input of the LOAD, All the capacitors charged by VDD starts to discharged to the ground and Vice Versa. 
- When all the capacitors discharge their charges and connects to the ground at the same time to the one and only ground line it creates the scenario called __GROUND BOUNCE__ .
If this bump gets into the undefined _NOISE MARGINE_ it can create a very big problem.

![Alt text](images/S28.png)

This same thing happens to the VDD line and it creates the __VOLTAGE DROOP__. 
So here the voltage levels changes from the ideal 1 to 0 and 0 to 1 and this problem happens because of only one voltage supply.

![Alt text](images/S29.png)

- __Problem Solving__ :

![Alt text](images/S30.png)
This problem can be solved by using multiple voltage and ground lines or supplies. This way whichever component needs the supply it can take it from the nearest supply line. As you can see there are vertical and horizontal lines on the chip area.s
and this scenario on the chip created the MESH.

- POWER MESH :
![Alt text](images/S31.png)

### 5. Pin Placement 

Lets take an design that we are going to implement on our chip it has pre-placed cells in it and we're going to make this circuit accroding to the placment of this pre-placed cells.
This connectivity of the circuit is defined in VHDL and this specific connectivity of the gates is called "NETLIST"
NETLIST = Connectivity Information between defferent gates.

Now as you can see in the image below we have two different designs with different inputs and outputs and clocks i/p and o/ps.
We then connect this two designs together using the wires to make our main circuit and then we are going to place this circuit on our chip so that the inputs will come closer to their desired locations. 

![Alt text](images/S32.png)

![Alt text](images/S33.png)

![Alt text](images/S34.png)

If you see in the image below you can see that the Din4 is placed in the start because it goes to the __BLOCK A__ which is already placed and it's location cannot be changed so we have to keep that in mind when we create the new circuit over this "BLOCKS". Now for __BLOCK B__ we can use the buffers to connect it to clkout.

![Alt text](images/S35.png)

So now we have our inputs on the left  side and outputs on the right side of the chip - we can definitely choose the location according to design or designer's choice. so now the input and output pins are placed on empty place. 

If you see all the ports are outside of the "BLOCK" area because nothing can be placed in that area.

The clock ports are bigger in area because clk ports input continuously sends the signal to the flip-flops in the circuit, so these are the ones who drives the whole circuit therefore we have to make their path bigger so they can have lower resistence.
It's also is the same for the clkout as it takes everythig out of the circuit alone therefore it needs lower resistance.

- __BLOCKING__ :

The blocking means covering which is used to block the i/p and o/p port area so that automated placement and routing tool dosen't place any cells in that area so we use "_logical cell placement blockage_" to seal the area ensuring that nothing gets placed near the are of pins. You can see it in the image below.
![Alt text](images/S36.png)
 
</details>

<hr>

## Implementation


#### Objectives :

1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.
2. Calculate the die area in microns from the values in floorplan def.
3. Load generated floorplan def in magic tool and explore the floorplan.
4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.
5. Load generated placement def in magic tool and explore the placement.

```
Area of die in Microns = Die width in Microns x Die height in Microns
```
<hr>

#### 1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.

<hr>

- Commands to invoke the OpenLANE flow and perform floorplan.

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker

```
```bash
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Now we can run floorplan
run_floorplan
```
- Screenshots of the floorplan run 

![Alt text](linux_images/run_floorplan1.png)
![Alt text](linux_images/run_floorplan2.png)

#### 2. Let's Calculate the die area in microns from the values in floorplan def.

<hr> 

- Screenshot of contents of floorplan def

![Alt text](linux_images/run_floorplan3.png)

```
According to the floorplan def :

1000 units distance = 1 micron
Die witdth in unit distance = 660685 - 0 = 660685
Die height in unit distance = 671405 - 0 = 671405

                         Value in unit distance 
Distance in microns =  —————————————————————————
                         1000

                        660685
Die width in microns = ———————— = 660.685 Microns
                         1000

                        671405
Die width in microns = ———————— = 671.405 Microns
                         1000
                        
Area of die in microns = 660.685 ∗ 671.405 = 443587.212425
```

#### 3. Load generated floorplan def in magic tool and explore the floorplan.

<hr>

Commands to load floorplan def in magic in another terminal

```bash
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
- Screenshots of floorplan def in magic :

![Alt text](linux_images/magic_op1.png)

- Equidistant placement of ports

![Alt text](linux_images/magic_op4.png)

- Port layer as set through config.tcl

![Alt text](linux_images/magic_op3.png)
![Alt text](linux_images/magic_op6.png)

- Decap Cells and Tap Cells

![Alt text](linux_images/magic_op2.png)

- Diogonally equidistant Tap cells

![Alt text](linux_images/magic_op7.png)

- Unplaced standard cells at the origin

![Alt text](linux_images/magic_op8.png)
![Alt text](linux_images/magic_op9.png)

#### 4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.

<hr>

- Command to run placement
```bash
# Congestion aware placement by default
run_placement
```









