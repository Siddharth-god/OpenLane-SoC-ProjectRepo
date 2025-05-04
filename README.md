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

#### 1.Run Desing Synthesis for "picorv32a" using OpenLANE & generate necessery outputs.
##### Commands to invoke the OpenLANE flow and perform synthesis.
```bash
# As we work in Openlane we have to first change our directory to openlane flow directory. Just type -

cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker ='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# aliased basically means creating the short version of the command so we don't have to type very long command all the time.
# So using only 'docker' we can invoke the OpenLANE flow docker sub-system.

command - docker

# After running docker the "bash" directory will get opened where we're gonna work
```

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


- ### Flop count 


![Alt text](linux_images/D1-Sk3-Lec5-img1.png)

![Alt text](linux_images/D1-Sk3-Lec5-img2.png)

```
               1613
Flop Count =  ——————— = 0.108429685
               14876

Percentage of DFF's = 0.108429685 x 100 = 10.8429685 %
```

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

<hr>

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

#### Now with this process our Floorplanning is completed - Now let's see how to place the NETLIST on this floor that we've made.

<hr>

### 1. Binding NETLIST with physical cells.

All the gates and flip-flops you can see on the image have different shapes but in reality those cells are squared shape. 

These all cells [ flip-flops , gates , etc] are stored or we can say are present in one SHELF and we can call it LIBRARY.

So this Library is like a real life library where all kinds of books are present - in this library we have all the Timing information [like delay] present in it. Also it'll have shape and size information in the shell plus it'll have the required condition on which the circuit works - This all information is present in Library.

Also, In this Library we have different properties of the cells like the different size of the cells to create low-resistance if we want and we can choose this sizes of the cells based on the space the chip can afford. 

Here are the images for better understanding -

![Alt text](images/S37.png)

![Alt text](images/S38.png)

![Alt text](images/S39.png)

<hr>

### 2. Placement

Now, we have our floorplan ready with all the inputs, we now have our NETLIST and also the physical view of the logic gates.

And we are going to place this NETLIST on the floorplan. Now we can't put the NETLIST exactly as in the diagram on the FLOORPLAN but because of that reason we have our physical view ready. And now we will do the connection according to the NETLIST.

Now, while placing the physical view on the floor we make sure that the pre-placed cells area won't be touched during this process and we will put the cells in a way that they will come closer to their desired I/P and O/P pins. 
Placement of the cells :

- Section (Orange) - Here, we put the cells directly as the physical view shows.
- Section (Yellow) - Here, we put the cells very close to each other which reduce the delay between them
- Section (Blue) - Here, The Din and Dout are very far from each other and they are in the cross format which makes the placement very   hard so we have to set them properly using buffers for keeping the information signal intact.

- Sectio (Green) - Here it's the same as the section 3 

![Alt text](images/S40.png)

![Alt text](images/S41.png)

![Alt text](images/S41-2.png)

<hr>

### 3. Optimized Placement

The placement gets so messy after placing the cells so we have to solve this using __Optimized Placement__

Here we can do the Estimation [ to send the actual information from input to the flip-flop we will use Repeators/buffers to sustain the information for a long route ] - So it's like two people are standing on the sides of the river and they are trying to communicate but their voice can't cover the long distance so we use one or two or even three people on the boats and they pass the information to the other side. So this is how the buffers work.
This Process is called __System integrity__.

How the buffers work --> This repeators that we are using will create the replica of the original signal and sends it to the next repeator and that repeator will do the same. This way the signal integrity is maintained - And this process is done by doing the transition analysis to check whether we need the buffer or not. 

Drawback - This process takes too much place.


![Alt text](images/S42.png)

![Alt text](images/S43.png)

![Alt text](images/S44.png)

![Alt text](images/S45.png)

</details>

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

<hr>

#### 2. Let's Calculate the die area in microns from the values in floorplan def.
 

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
<hr>

#### 3. Load generated floorplan def in magic tool and explore the floorplan.


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

<hr>

#### 4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.



- Command to run placement
```bash
# Congestion aware placement by default
run_placement
```
Run placement screenshots

![Alt text](linux_images/run_placement.png)

![Alt text](linux_images/run_placement2.png)

<hr>

### 5. Load generated placement def in magic tool and explore the placement.

Commands to load placement def in magic in another terminal

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
Floorplan def screenshot in magic

![Alt text](linux_images/placement_op.png)

Placement of Standard cells

![Alt text](linux_images/placement_op2.png)

<details>
  <summary><h2>Theory - Cell Design flow - Click Here<h2></summary>

- Basic theory about how the cell is designed internally -
The cells you can see in the image below are called standard cells. These standard cells are placed in library - The Library also contains the pre-placed cells.
The library contains all typical cells like Flip-Flops, AND, OR, Inverters, Latches etc. and it also contains all this cells with their different sizes and their different functionalities.
![Alt text](images/S46.png)

![Alt text](images/S47.png)

![Alt text](images/S48.png)

Now, All this cells in the library has their own design flow. 
Let's take the inverter as an example -
- Now to design the normal inverter there is very big stage -
![Alt text](images/S49.png)

#### 1. Inputs
- __DRC & LVS Rules__ - This rules are given by foundry and there could be thousands of rules that a design should follow
![Alt text](images/S50.png)

- __Spice model parameters__ - This are the model files given by the foundry itself according to our design parameters. This model files have parameters like Vth, Capacitors values, VDD values and etc etc.
![Alt text](images/S51.png)

- __Library and User-Defined specializations__ - This are some specializations which are given by the user or the Top designer and the designers have to design this specialization according to the orders.
for example - 

1) Cell Height - This is defined by the seperation between the power and ground rails the above one is power rail and the next one is the ground rail and that is the height of the cell and it's fixed once designed.

2) Cell Width - This is derived by the drive strength of the cells Higher the drive strength is better because the cells with higher drive strength can drive the longer wires while short drive strength cannot do that.

This is the responsibility of the designer to define the drive strength and this is specialization asked by the user and the designer has to follow that spcification.

3) Supply voltage - A cell has to operate at a certain supply voltage that has been provided by the top level designer and according to that the library developer has to design the library cell that works on the given supply voltage.
![Alt text](images/S52.png)
![Alt text](images/S53.png)

4) Metal Layer - Which part will go on which metal layer is given by the user and the designer has to use that infomation to design accordingly.

5) Pin location - Library developer has to design the pin location based on the need of the user.
![Alt text](images/S54.png)

#### 2. Design setup 

1) Circuit design -

step 1 - Implementation of the function itself
step 2 - Modeling the PMOS and NMOS transistors to meet the library requirments
![Alt text](images/S55.png)

2) Layout design -

step 1 - First design the function and then derive the network graph for the PMOS and NMOS transistors
step 2 - Get the Euler's path
step 3 - Stick diagram based on the circuit diagram
step 4 - Convert that stick diagram into the physical layout according to the DRC rules which are given by foundry and by top level designer.
step 5 - Then we use EDA tool to create the Neat Layout design.

![Alt text](images/S56.png)
![Alt text](images/S57.png)

This is the final library layout design.
![Alt text](images/S58.png)

#### Characterization 

Characterization gives us Timing, noise, power and the funcitionality of the NETLIST.

Steps to perform the characterization - 

1) Read into the model files PMOS and NMOS.
2) Read the extracted spice netlist
3) Recognize the behaviour of the buffer
4) Read the subcircuit of the inverter 
5) Attach the power supply 
6) Apply the stimulus
7) Provide the necessary load capacitance
8) Provide the necessary simulation command ex - .tran
9) Feed all inputs in the form of config file to the software GUNA and then we can generate the timing, noise and power.

Here are all the related images - Step by Step
![Alt text](images/S59.png)
![Alt text](images/S60.png)
![Alt text](images/S61.png)
![Alt text](images/S62.png)
![Alt text](images/S63.png)
![Alt text](images/S64.png)
![Alt text](images/S65.png)
![Alt text](images/S66.png)

This is how the characterization is done. This is not explained in details but it's only a roadmap fo the Cell design flow.

This whole process is for only one small inverter with one input and output. Crazy ha...

## Timing Characterization

- Formula for finding the propogation delay and Transition time.

```
Propogation Delay :

Propogation Rise Delay = Time(out_rise_thr) - Time(in_rise_thr)

Propogation Fall Delay = Time(out_fall_thr) - Time(in_fall_thr)

--------------------------------------------------------------------------
Transition Time :

Transition Rise Time = Time(slew_high_rise_thr) - Time(slew_low_rise_thr)

Transition Fall Time  = Time(slew_high_fall_thr) - Time(slew_low_fall_thr)
```

- Typical values of __Threshold points__ on input and output waveforms for DELAY and TRANSITION TIME Measurment.
![Alt text](images/S67.png)

- 1) Propogation Delay :
![Alt text](images/S69.png)

![Alt text](images/S68.png)

- We have to select the threshold points to get the perfect delay value if we take the wrong delay points we will get the (-ve) delay value which is not correct.

![Alt text](images/S70.png)

- Sometimes we get (-ve) delay values even with the correct threshold points. Although this is very rare but it happens and it is because higher slew in the waveform and this happens due to long wires in between the transistors so they give the longer slew and due to that our output waveform comes before input, And even when we use the correct threshold points we get the (-ve) value. So we might have to alter the points accordingly.

![Alt text](images/S71.png)

![Alt text](images/S72.png)

- 2) Transition Time :

- This is how we find the transition time. The formula is given at the start of the topic.
![Alt text](images/S73.png)

![Alt text](images/S74.png)

![Alt text](images/S75.png)

</details>

<hr>

## Day 3 - Design library cell using Magic Layout and ngspice characterization.

<details>
  <summary><h2>Theory - Click "Theory" to expand.<h2></summary>

### - VTC Spice simulation

1) Creating the spice deck
- Component connectivity 
- Component Values 
- Identify nodes :

    How do we decide nodes ?
    --> To decide nodes there should be a component in between of these nodes if there is no component we do not consider it as a node.
- Name the nodes
![Alt text](images/S76.png)
![Alt text](images/S77.png)

2) Writing the spice deck or we can say the netlist for INVERTER.

- This is the syntax for setting the connection information of CMOS

Transistor - drain - gate - source - substrate - transistor name - width - length (ex- M1 out in vdd vdd pmos w=0.375u L=0.25u)
.op , .dc , .tran are all the simulation commands there are other commands as well and this start from (.--command--)
![Alt text](images/S78.png)
![Alt text](images/S79.png)

3) This is how model file look if you are not familiar with it -

![Alt text](images/S80.png)
![Alt text](images/S81.png)

4) Waveform when PMOS and NMOS W/L ratio was same.
![Alt text](images/S82.png)

5) Waveform when PMOS ratio is greater than the NMOS ratio. Usually we have to set the PMOS bigger as NMOS is already powerful than PMOS so that is the reason why we make PMOS stronger than NMOS to make them equal.

![Alt text](images/S83.png)
![Alt text](images/S84.png)

- Static behaviour : CMOS inverter Robustness

1) Switching threshold , Vm (Vm is the point where Vin = Vout)

You can see the intersection/diagonal line on the graph which shows us the Vm. The Vm lies in the saturation region where so much current can get leaked due to Vin = Vout

You can see in the image below that our Idsp = -Idsn it means that our Vgs = Vds (The Drain current from PMOS is going towards capacitor and the Drain current of NMOS is going opposite and it is almost equal) The current from Vdd to Vout which is going through the PMOS is equal to the current to the current from capacitor to the NMOS so thats why we say Vds = Vgs.
![Alt text](images/S85.png)
![Alt text](images/S86.png)

2) Dynamic simulation

- In the dynamic simulation our whole NETLIST remains the same only the pulse and the .tran command gets added in the NETlist

- Transit analysis :
 
Using this analysis we can calculate the rise and fall delay.

- Process to calculate the delay :

To calculate the delay we just need to select the desired area on the waveform and then we have to click on the point and we will get that point's fall or rise time whatever we are calculating will come on the screen of the ngspice and then we have to the calculator to find the delay by substraction it accprding to the formulas which you can see on the image below.
![Alt text](images/S87.png)
![Alt text](images/S89.png)
![Alt text](images/S90.png)
![Alt text](images/S91.png)

---

## 16 MASK CMOS Process

## 🔹 Step 1 - Selectiong the Substrate
- What we have to do is select a substrate which we normally do p-type or n-type silicon substrate. We keep the doping level less than the well doping. 

![Alt text](images/S92.png)


## 🔹 Step 2 - Creating active region for transistors.

1) Creating isolation between p-substrate and active packets using 40nm silicon dioxide(SiO₂) layer which acts as insulator. 
2) We deposit 80nm layer of Si₃N₄ onto 40nm silicon dioxide (SiO₂).

## 🔹 Step 3 - Creating the Pockets

1) Identify the region where to grow the pockets. So, first we have to grow the photoresist film of 1 micron onto the 80nm silicon nitrate(Si₃N₄)

2) We have to use the mask as a protection layer from UV light. So there will be no reaction between the UV light and the photoresist layer. This mask protects the photoresist layer directly below the mask and the area which is not covered by the mask will react with UV light. 

![Alt text](images/S93.png)

- Now, After that we have protected the area by mask which we wanted for our n-well and p-well creation. These wells are exactly below the area of the mask layer. This process of protecting the photoresist from UV light is called __Photolithography__. 

- After this process we wash out the unwanted area with some solution and what remains is the area protected by the mask. 
- Then we remove the mask. 
- The photoresist layer acts as a protective layer and when we do further process like etching the area under this photoresist layer does not get affected while the area which is not protected by this layer will be affected.

![Alt text](images/S94.png)

![Alt text](images/S95.png)

- Now we do the etching process and the silicon nitrate(Si₃N₄) will get etched off from the not protected area. 

![Alt text](images/S96.png)

- We now remove the photoresist layer chemically. 

![Alt text](images/S97.png)

- Now once this process is completed we put the chip in oxidation furnace and the temperature of the furnace is around 900 to 1000 degree celsius which helps us grow the oxidation layer on our chip. Like we grown the SiO₂ layer first we have to again grow the SiO₂ layer but this time the area below the silicon nitrate is protected and it won't grow silicon dioxide there. 

- __LOCOS__ and __Bird's beak__ : Now the silicon dioxide layer works as isolator or insulator and the two sections of the silicon nitrate layer are fully separated from each other and this process is called __LOCOS__ or __local oxidation of silicon__. And the dense area of silicon dioxide are called __Bird's beak__.

![Alt text](images/S98.png)

![Alt text](images/S99.png)

- After that we strip out the silicon nitrate layer Si₃N₄ in the hot phosphoric acid and then we get our SiO₂ isolation layer between two transistors so the transistor won't be able to communicate with each other. The narrow area or the area which is not the bird's beak is the active region where we use our transistors and the wide area acts as a isolation area between them.

![Alt text](images/S100.png)


## 🔹 Step 4 - N-Well and P-Well

Now our oxidation process has been completed. So we have to create n-well and p-well. So the procedure for n-well and p-well is like this.  

- __P-Well__ :
To creatr P-well, We need to protect one area while we fabricate other.
We again use photoresist layer on the whole chip. And we will use Mask 2 to protect the one side of area. After we done with mask, we again expose the photoresist area to the UV light. And the mask protects the one side while UV rays reacts with the exposed area. 

- Mask 2 -
![Alt text](images/S101.png)

![Alt text](images/S102.png)

Then we wash away the exposed or reacted area by some chemical. And we can now do the further process. 
We then remove the mask which we placed on the one side of the whole chip. And we try to create p-well on the right side or the exposed area using the __boron material__. 

  __Ion Implantation__ :
  Boron is p-type material and it is diffused into the material using process called ion implantation. And it creates the p-well. Boron passes through the thick surface of the SiO₂ to create p-well. It has the energy of 200 keV energy. And in this process boron does damage to the oxidation layer. So we have to again repair that layer as well. We will see that in the later process.

![Alt text](images/S103.png)

- __N-Well__ :
Now for the n-well we use the similar process. And we use mask 3 on the other side which we did not covered previously. And in this process for n-well creation we use the __phosphorus material__ which is heavier than the boron material. And it has the energy of 400 keV. And it penetrates through the SiO₂ and creates n-well. 

- Deapth of Wells -
Now the depth of the wells is not finalized yet. So we have to diffuse this wells to make them bigger for our PMOS and NMOS fabrication. Now, we put these chips into the drive-in furnace, which is a very high temperature furnace. And we put the chip for a long time there. It's about 1100 degrees Celsius for 4-6 hours. And it will create the welds big enough to fabricate our PMOS and NMOS onto them. And this process is called __Twin-TUBS__ process.

![Alt text](images/S104.png)

![Alt text](images/S105.png)

---

### 🔹 Threshold Voltage Adjustment

To ensure that NMOS and PMOS devices switch at the desired voltage, we must adjust the threshold __voltage (Vth)__.

This is done through threshold voltage implantation, also called Vth adjustment:

  - For NMOS, we implant a small amount of Boron (p-type dopant) into the active region.

  - For PMOS, we implant a small amount of Phosphorus or Arsenic (n-type dopant).

The implant dose and energy are carefully controlled, usually using low-energy implantation so that the dopants stay close to the surface.


![Alt text](images/S106.png)

## 🔹 Step 5 – **Formation of the Gate**

In this step, we perform the **photolithography process** once again, as it is required at every fabrication stage involving pattern transfer.

### Gate Formation in P-Well (for NMOS):

* We use **Mask 4** for the N-well region.
* A **boron implant (P-type impurity)** is used with **low energy (\~60 keV)** so that doping occurs **close to the surface**.
* The **dose of boron** is carefully controlled to achieve the required **threshold voltage (V<sub>th</sub>)**.
* This surface-level doping is known as **channel engineering** and ensures proper transistor behavior.

![Alt text](images/S107.png)

![Alt text](images/S108.png)


### Gate Formation in N-Well (for PMOS):

* We use **Mask 5** for the P-well region.
* **Phosphorus or arsenic** (N-type impurities) are implanted using **low energy** to keep doping shallow.
* This also helps in controlling the **V<sub>th</sub>** for NMOS.

![Alt text](images/S109.png)

![Alt text](images/S110.png)

---

### Fixing the Damaged Oxide:

* Due to prior processing, the gate oxide might be damaged.
* So, we **strip the damaged oxide** using **hydrofluoric acid (HF)**.
* A **new gate oxide layer of same thickness** is regrown over both N-well and P-well.

![Alt text](images/S111.png)

![Alt text](images/S112.png)

![Alt text](images/S113.png)

---

### Polysilicon Deposition and Gate Etching:

* A **400 nm thick polysilicon layer** is deposited over the wafer.
* To reduce gate resistance, this polysilicon layer is **heavily doped with N-type impurities**.
* Photolithography is repeated using **Mask 6** to define the gate shape.
* After developing the photoresist, the **unwanted polysilicon is etched away**.
* Finally, the remaining photoresist is removed, and **gate structures are formed** for both NMOS and PMOS.

![Alt text](images/S114.png)

![Alt text](images/S115.png)

![Alt text](images/S116.png)


## 🔹 Step 6 – **Lightly Doped Drain (LDD) Formation**

We now form the **Lightly Doped Drain (LDD)** regions to reduce short channel and hot electron effects.

(P+)  (P-)  (N) - This is the doping profile we are trying to achieve.

(P+) is our source and drain of PMOS, N is our N-well and (P-) is the LDD which is lightly doped drain formation we are trying to achieve.

(N+) (N-) and (P) where (N+) is the source and drain (N-) is what we are trying to achieve which is lightly doped drain and then there is P which is P-well which we already have.

![Alt text](images/S117.png)

#### Why LDD is used instead of P⁺/N⁺ directly:

1. **Hot Electron Effect**:

   * When device size shrinks but supply voltage remains constant, the **electric field increases** (E = V/d).
   * This can generate **high-energy carriers** that break Si–Si bonds or even damage the thin gate oxide (\~3.2 eV barrier).
   * Using **lightly doped N⁻ or P⁻** near the drain reduces the field intensity, thus minimizing this effect.

2. **Short Channel Effect**:

   * Smaller channel lengths make it difficult for the gate to control the current due to **drain field penetration**.
   * LDD regions help control this by spreading the electric field and maintaining threshold voltage integrity.


### LDD Formation Process:

* The **dotted regions in the image** represent where the **LDD implants (N⁻ and P⁻)** will be made.

#### For NMOS (in P-well):

* Apply **Mask 7** on N-well to protect PMOS.
* Perform **photolithography**, then implant **phosphorus (N-type, light dose)** to form N⁻ regions.

![Alt text](images/S118.png)

![Alt text](images/S119.png)

#### For PMOS (in N-well):

* Apply **Mask 8** to protect NMOS side.
* Implant **boron (P-type, light dose)** into dotted areas to form P⁻ regions.

![Alt text](images/S120.png)

These **N⁻ and P⁻ regions** are now the **Lightly Doped Drain regions**, located just beside the gate on both sides.

---

### Spacer Formation (to protect LDD):

* After LDD implantation, a **thick insulating layer** (usually **SiO₂ or Si₃N₄**) is deposited.
* **Plasma anisotropic etching** (directional etching) is performed.

  * It **removes material only in vertical direction**, leaving spacer material intact **near the gate sidewalls**.
* These **spacers protect the shallow LDD regions** from being destroyed during the next heavy implantation step.

![Alt text](images/S121.png)

---

### Final Source and Drain Implantation:

* After spacer formation:

  * **Heavily doped N⁺** (for NMOS) and **P⁺** (for PMOS) source and drain regions are implanted.
* These sit just **outside the spacers**, completing the **self-aligned LDD structure**.

---

### Summary of LDD Structure:

* Final doping profile:

  * **PMOS**: P⁺ | P⁻ | N-well | P⁻ | P⁺
  * **NMOS**: N⁺ | N⁻ | P-well | N⁻ | N⁺
* The **central gate** controls the channel, and the **LDD regions (P⁻/N⁻)** serve as buffers before the heavily doped source/drain.

Great! Here's your explanation of **Step 6: Source and Drain Formation**, formatted with `__` for bold emphasis, and clearly structured for clarity and understanding:

---

## 🔹Step 7: Source and Drain Formation

### Purpose:

To form the __heavily doped regions__ (N+ and P+) that act as the **source and drain** of NMOS and PMOS transistors.

### **Screen Oxide Deposition**

* Before implantation, we grow a very thin layer of **screen oxide** (a few nm thick).
* This is done to:

  * **Prevent channeling** of implanted ions deep into the substrate.
  * Help **control dopant distribution** near the surface.
* This oxide layer is grown **on top of the silicon dioxide layer** and also **on top of the polysilicon gate**.

![Alt text](images/S122.png)

### **NMOS (N+ Implantation)**

1. We begin by defining which side of the chip we want to do the N+ implantation on.

2. Apply a **photoresist layer** and expose it using **Mask 9**, which **covers the left side** of the wafer.

3. We perform standard photolithography:

   * **Expose** the right side.
   * **Develop** the photoresist.
   * **Etch away** the oxide and remove the resist where needed.

4. Now the **right side is open** for implantation, and the **left side is protected** and we remove the MASK 9.

![Alt text](images/S123.png)

5. We implant **Arsenic (As)** at **75 keV** into the exposed region.

![Alt text](images/S124.png)

6. The **sidewall spacers** already present beside the gate protect the **LDD regions**, so the **N+ implant** occurs only in the **exposed source/drain areas**.

7. This creates the **N+ source and drain** regions for the **NMOS** transistor.

In image look the region of P-Well you can see the extra built region that is N+ Implant

![Alt text](images/S125.png)

---

### **PMOS (P+ Implantation)**

1. We repeat the same process, this time for the **PMOS** device on the **other side** of the chip.

2. Apply photoresist again and use **Mask 10** to **cover the right side**.

3. Now the **left side is exposed** and we implant **Boron (B)** at **50 keV**.

4. Again, the **sidewall spacers** protect the LDD areas.

5. This results in **P+ source and drain** regions for the **PMOS** transistor.

![Alt text](images/S128.png)

---

### **Annealing / Drive-In Diffusion**

* After both N+ and P+ implants, the wafer is placed in a **high-temperature furnace** at about **1000°C**.

* This step is called **annealing** or **drive-in diffusion**, and it serves two purposes:

  * **Repair damage** caused by ion implantation.
  * **Push the dopants deeper** into the substrate to form proper junctions.

![Alt text](images/S126.png)

* The result:

  * **N+ regions** form deep and well-defined **source and drain** for **NMOS**.
  * **P+ regions** form deep and reliable **source and drain** for **PMOS**.

![Alt text](images/S127.png)

---

At the end of this step, we now have:

* **N+ source and drain** for NMOS (in the P-substrate).
* **P+ source and drain** for PMOS (in the N-well).

These are essential for completing the active regions of both transistors.

---

## Step 8 : Formation of Contacts and Interconnect Locals

### Purpose:

To create electrical **contacts** to the **source (S), drain (D), and gate (GND)** terminals of both NMOS and PMOS. These contacts are **the only accessible points** for external circuits to communicate with the transistors.

#### 1) Removing the Screen Oxide :

* First, we **remove the thin screen oxide** layer (used in the previous step to prevent channeling).
* This is done using a **hydrofluoric acid (HF) solution**, which etches away the oxide.
* We remove this screen oxide **completely from all areas** where it was deposited — including above the **source, drain, and gate**.
* Now the **source, drain, and gate contacts are exposed** and ready for further processing.

![Alt text](images/S129.png)

#### 2) Titanium Deposition (Local Interconnect Formation) :

* We now deposit **titanium (Ti)** over the wafer surface.
* This is done using the **sputtering process**, which works as follows:

  * Titanium metal is bombarded with **argon gas (Ar)** ions.
  * The titanium atoms get **ejected and deposited** uniformly over the wafer surface.
* Titanium is chosen because it has **low resistivity**, which helps reduce contact resistance.

![Alt text](images/S130.png)

![Alt text](images/S131.png)


#### 3) Silicidation Process (Forming Contact Material):

* Next, we **heat the wafer** to around **650–700°C** in a **nitrogen (N₂) ambient** for about **60 seconds**.
* Two chemical reactions occur:

  1. **Titanium + Silicon** (at source/drain/gate) → **Titanium Silicide (TiSi₂)**

     * This forms a **low-resistance contact** at S, D, and G.
  2. Titanium also reacts with surrounding nitrogen to form **Titanium Nitride (TiN)**.

     * TiN is used only for **local interconnections** within the chip.

![Alt text](images/S132.png)

![Alt text](images/S133.png)


#### 4) Contact Patterning with Mask 11 :

* We now define the actual contact areas using **photolithography**:

  * Apply photoresist and expose it using **Mask 11**, which defines where contacts should remain.
  * Develop and etch the wafer.
* We remove the unwanted **Titanium Nitride (TiN)** from exposed areas using an **RCA cleaning process**.

![Alt text](images/S134.png)

![Alt text](images/S135.png)

![Alt text](images/S136.png)

![Alt text](images/S137.png)

---

#### Final Result:

* Only the required **TiSi₂ contacts** remain at the source, drain, and gate terminals.
* These contacts:

  * Provide **electrical connectivity** to external circuits.
  * Enable **communication** between internal devices and the chip's outer world.
  * Serve as the foundation for **metal interconnects** in the next step.

![Alt text](images/S138.png)


Absolutely! Here's a **more detailed, clear, and beginner-friendly version** of **Step 9: Higher Level Metal Formation**, written in a way that even someone new to VLSI or chip fabrication can follow:

---

## Step 9: Higher Level Metal Formation (with Complete Explanation)

####  Why This Step is Important:

After we’ve created the initial metal connections (local interconnects) and contacts to source, drain, and gate, we need to build **higher-level metal connections** that link different parts of the chip together. These layers carry signals across the chip and allow different transistors and logic blocks to communicate. But before we do this, we face a problem...

#### Problem: Uneven Surface

* The surface of the wafer is **bumpy and non-planar** due to all the previous fabrication steps (implantation, gate formation, contact formation, etc.).
* If we deposit metal on this uneven surface, it won’t work well:

  * The metal may **break or disconnect**.
  * It may not **etch correctly**.
  * It may cause **short circuits**.
* So, we first need to **make the surface flat**. This is called **planarization**.

![Alt text](images/S138.png)

---

#### 1) Planarizing the Surface 

* We deposit a **1 micron thick layer of Silicon Dioxide (SiO₂)** over the wafer.

  * But not pure SiO₂ — we **dope** it with:

    * **Phosphorus** → helps **block sodium contamination**, which can damage the chip.
    * **Boron** → helps **reduce high-temperature effects**.
* This doped SiO₂ is called:

  * **Phosphosilicate Glass (PSG)** or
  * **Borophosphosilicate Glass (BPSG)**.
* After deposition, we use **CMP (Chemical Mechanical Polishing)**:

  * A technique that **polishes the surface** flat using both **chemical etching** and **mechanical abrasion**.
* Result: A **smooth, flat surface**, ready for more metal layers.

![Alt text](images/S139.png)

![Alt text](images/S140.png)

#### 2) Making Contact Holes (Using Mask 12) 

* Now we need to create **holes (vias)** in this new oxide layer so we can connect the **next metal layer** to the existing lower metal or contacts.
* Process:

  1. **Photolithography using Mask 12** to define where the holes should be.
  2. Expose and develop the photoresist.
  3. **Etch the SiO₂** where photoresist was exposed to form the **contact holes**.
  4. Remove the photoresist layer.

![Alt text](images/S141.png)

![Alt text](images/S142.png)

#### 3) Adding Barrier and Contact Metal 

* First, we deposit a **thin layer (\~10 nm) of Titanium Nitride (TiN)**:

  * TiN acts as a **barrier** to prevent diffusion of metal into the oxide.
  * It also **helps aluminum stick better** to the surface.
* Then we **fill the contact holes with Tungsten (W)**:

  * Tungsten is chosen because it **completely fills the small holes** without leaving gaps.
  * This is done by **blanket deposition**.
* After that, we use **CMP again** to **remove excess tungsten** and leave a flat surface with contact holes perfectly filled with tungsten.

![Alt text](images/S143.png)

![Alt text](images/S144.png)

![Alt text](images/S145.png)

#### 4) First Higher-Level Metal (Metal 2) 

* Now we’re ready to build the **second metal layer (Metal 2)** on top.
* We deposit a layer of **Aluminum (Al)** over the whole surface.
* Then:

  1. Use **Mask 13** to define the metal wire pattern using photolithography.
  2. Remove photoresist and use **plasma etching** to remove unwanted aluminum.
  3. Strip the remaining photoresist.
* Now we have **metal lines** connecting different areas of the chip.

![Alt text](images/S146.png)

![Alt text](images/S147.png)

![Alt text](images/S148.png)

#### 5) Repeating for More Metal Layers (Metal 3, Metal 4, etc.) 

We follow the **same steps again** to build more layers:

1. **Deposit SiO₂** for insulation.
2. **Planarize with CMP**.
3. **Photolithography** to define contact holes (Mask 14, Mask 15, etc.).
4. **Etch holes**, **add TiN barrier**, **fill with tungsten**, and **CMP**.
5. **Deposit Aluminum**, **pattern using new mask**, **etch, and clean**.

⚠️ Note: As we go to higher layers:

* The **aluminum wires are made thicker** to carry more current for power delivery.

![Alt text](images/S149.png)

![Alt text](images/S150.png)

![Alt text](images/S151.png)

#### Final Protection Layer 

* Once all the metal layers are done, we **protect the chip**:

  * We deposit a **Silicon Nitride (Si₃N₄)** layer.
  * This layer acts as a **final protective coating** to:

    * Block moisture,
    * Stop dust or particles,
    * Prevent scratches and corrosion.

![Alt text](images/S152.png)

#### Final Step: External Contact Holes (Using Mask 16) 

* We need to connect the chip to the **outside world** (like the pins on a chip package).
* So, we use **Mask 16** to:

  1. Drill holes through the nitride protection layer.
  2. These holes expose the **final metal contacts**.
* After this, the chip is ready to be **packaged and used**.

#### Final Result 

* A **multi-layer interconnect system** with strong, low-resistance metal connections.
* A **planar and protected chip** ready for electrical testing and packaging.

![Alt text](images/S153.png)

---




</details>

## Implementation

#### Objectives :

1. Clone custom inverter standard cell design from github repository: Standard cell design and characterization using OpenLANE flow.
2. Load the custom inverter layout in magic and explore.
3. Spice extraction of inverter in magic.
4. Editing the spice model file for analysis through simulation.
5. Post-layout ngspice simulations.
6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

#### 1. Clone custom inverter standard cell design from github repository

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design 
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory from openlane directory using
cd vsdstdcelldesign

# Open new Tab and Change the directory to magic 
cd Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic 

# Copy magic tech file to the repo directory for easy access
cp sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/

# Check contents whether everything is present
ls -ltr

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

![Alt text](linux_images/clone.png)
![Alt text](linux_images/path.png)
![Alt text](linux_images/copy_techfile.png)
![Alt text](linux_images/copied.png)

#### 2. Load the custom inverter layout in magic and explore.

Screenshot of custom inverter layout in magic

![Alt text](linux_images/inverter.png)

NMOS and PMOS identified

![Alt text](linux_images/nmos_part.png)

![Alt text](linux_images/pmos_part.png)

Output Y connectivity to PMOS and NMOS drain verified

![Alt text](linux_images/drain_region.png)

PMOS source connectivity to VDD (here VPWR) verified

![Alt text](linux_images/source_vdd.png)

NMOS source connectivity to VSS (here VGND) verified

![Alt text](linux_images/source_gnd.png)

// If we delete the specific part from the layout then we will get the DRC error 
- We want DRC = 0 for clean layout without errors 
Therefore, if we delete any part then we have to add that part again to tackle the error.
The error will be written in the tkcon window and if we click on the drc tab then it'll zoom on the error.

![Alt text](linux_images/error_drc.png)

#### 3. Spice extraction of inverter in magic.

Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic
```bash
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```
Extracting the file into our vsd folder

![Alt text](linux_images/extract_file.png)

File has been extracted into our folder

![Alt text](linux_images/extracted_file.png)

- We will create spice file using ext file which will be used in our ngspice tool

![Alt text](linux_images/ext2spice.png)
![Alt text](linux_images/spicefile.png)
![Alt text](linux_images/content_of_spice.png)

#### 4. Editing the spice model file for analysis through simulation.

Measuring unit distance in layout grid

![Alt text](linux_images/grid_units.png)

Final edited NETLIST file ready for ngspice simulation

![Alt text](linux_images/netlist.png)

#### 5. Post-layout ngspice simulations.

Commands for ngspice simulation

```bash
#Install ngspice
sudo apt install ngspice

# Command to directly load spice file for simulation to ngspice
ngspice sky130_inv.spice

# Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
plot y vs time a
```
Ngspice install 
![Alt text](linux_images/ngspice.png)

Screenshots of ngspice run
![Alt text](linux_images/ngspicerun.png)

Screenshot of generated plot
![Alt text](linux_images/waveform.png)
![Alt text](linux_images/waveform2.png)

```
Rise time calculation :

Rise time calculation = Time taken for o/p to rise 80% - Time taken for o/p to rise 20%
20% of output value = 0.660 V
80% of output value = 2.64 V
```
20% RISE -
![Alt text](linux_images/rise20w.png)
![Alt text](linux_images/rise20.png)

80% RISE -
![Alt text](linux_images/rise80w.png)
![Alt text](linux_images/rise80.png)

```
Rise transition time = 2.24638 - 2.18242 = 0.06396ns = 63.96ps
```
```
Fall time calculation :

Fall transition time = Time taken for o/p to fall 20% - Time taken for o/p to fall 80%
20% of output value = 0.660 V
80% of output value = 2.64 V
```
20% FALL -
![Alt text](linux_images/fall20w.png)
![Alt text](linux_images/fall20.png)

80% FALL -
![Alt text](linux_images/fall80w.png)
![Alt text](linux_images/fall80.png)

```
Fall transition time = 4.0955 - 4.0536 = 0.0419 = 41.9
```
```
Rise Delay calculation :

Rise cell delay = Time taken for o/p to rise 50% - Time taken for i/p to fall to 50%

50% of 3.3 V = 1.65 V
```
50% RISE -
![Alt text](linux_images/risedelayw.png)
![Alt text](linux_images/risedelay.png)

```
Rise cell delay = 2.21144 - 2.15008 = 0.06136 = 61.36
```
```
Fall Delay calculation :

Fall cell dealy = Time taken for o/p to fall 50% - Time taken for i/p to rise to 50%

50% of 3.3 V = 1.65 V
```
50% FALL -
![Alt text](linux_images/falldelayw.png)
![Alt text](linux_images/falldelay.png)

```
Fall cell delay = 4.07 - 4.05 = 0.02 = 20ps
```

#### 6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.
Link to Sky130 Periphery rules: https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html

Commands to download and view the corrupted skywater process magic tech file and associated files to perform drc corrections

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

# Command to view .magicrc file
gvim .magicrc

# Command to open magic tool in better graphics
magic -d XR &
```