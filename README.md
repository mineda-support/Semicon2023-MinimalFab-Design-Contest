# Semicon2023-MinimalFab-Design-Contest
Minimal Fab Design contest for SemiCon Japan 2023. Because Minimal Fab is missing 2nd metal due to VIA forming difficulty, only 1st metal is available. On prefabricated wafers, participants boast their design skill with 1st metal routing only. 


# Participate in the Minimal Fab Design Contest
The contest reception period is from November 1, 2023 to November 31, 2023, but even after the reception period, you can use this document as a tutorial for using the open source minimal fab PDK with open source EDA.
I used Xschem as the schematic editor, but you can also do the same thing with LTspice. Please use this document as a reference even just for the layout GDS creation part.

**Used data**

https://www.dropbox.com/scl/fi/r3q1jlgax7d4c2oxkr63b/xschem.zip?rlkey=494hohv5aqh8k0pfhhc06gdyp&dl=0

## PDK installation

For Windows, run the following under ~/KLayout/salt

    git clone  https://github.com/mineda-support/AnagixLoader.git
    git clone https://github.com/mineda-support/ICPS2023_5

Note: Create ~/KLayout/salt if it does not exist.
For details, please refer to  [+ICPS2023_5 PDKユーザマニュアルv1.01](https://paper.dropbox.com/doc/ICPS2023_5-PDKv1.01-MzNF89mYjQ8nonWSGXb1j) (in Japanese).

For PDK update information and Q&A, please use Discord's dedicated server (ICPS PDK).
Invitation code: https://discord.gg/bYUeMkVz5N (in Japanese).

## EDA tools installation

Use Xschem (or LTspice) for circuit design and KLayout for layout design. The latest version of KLayout is fine, but version 0.28.3 is recommended.

## Create a schematic

This is the NAND circuit I created (nand_min.sch):

![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699317792675_image.png)

## Check operation with simulation

Added external elements and configured simulation settings (nand_min_simulation.sch)

![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699318068081_image.png)


Following the netlist button, I clicked simulate and ran ‘plot net1, net2, y’.

![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699318401454_image.png)


Note:

- Change the model path to your own.
- If the simulation does not run, check the settings in Menu → Simulation → Configure simulator and tools.
## Create a layout

If you don't want to do the preparation work, try [+Participate in the Minimal Fab Design Contest: Completed-Example](https://paper.dropbox.com/doc/Participate-in-the-Minimal-Fab-Design-Contest-Completed-Example-Nx6HLSLlAomupxZKQSsIH#:h2=Completed-Example) 

**Preparation Work**
Please use the following base design for prefabrication.

     ~/KLayout/salt/ICPS2023_5/Samples/Semicon2023/base_contest2023.GDS

First, open this GDS and save it to an appropriate location (use Menu → File → Save As). Here, I saved it to c:/tmp/NAND/nand.GDS.
Create a cell for just your own wiring.

![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699319350232_image.png)


As for the structure of the cell, you can see that base_Rev0.93 of the base and nand_min are at the same level (that is, it is not yet in a hierarchical structure). So, make nand_min the top level and import base_Rev0.93 there.

![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699319511241_image.png)


Then, drag and drop base_Rev0.9 onto the nand_min layout screen. To display all layers, select Menu → Display → Full Hierary. Next, press the F2 button to display the full screen, and it will look like the image below, indicating that the design is ready.

![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699320053627_image.png)


**Design Work**
Enlarge the GDS and start drawing the PATH. First, select the ML1 layer.。

![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699320305861_image.png)


Then, click the Path icon to start PATH wiring. Width was set to 10um.

![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699320472243_image.png)


**Completed Example**
Here is a completed example. If you want to pass the preparation work, please use this.

    ~/KLayout/salt/ICPS2023_5/Samples/Semicon2023/NAND/nand_sample.GDS
![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699320614952_image.png)

## Execute DRC

Run Menu → Tools → DRC → DRC for ICPS2023_5.

![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699321278735_image.png)


Please correct the layout until the error disappears.

## Create a schematic for LVS

**Explanation for Pre-processing**
As shown in the figure, LVS compares the reference netlist extracted from the schematic with the netlist extracted from the layout.

![](https://paper-attachments.dropboxusercontent.com/s_D77ECE07A617D0EC4BA135230251D2F6E0BD93906A026F633371E2068C2C230D_1699363436615_image.png)


The script get_reference preprocesses the netlist output from the schematic editor.
**Creating a circuit netlist**
The created circuit diagram is nand_min.sch shown in [+Participate in the Minimal Fab Design Contest: Create-a-schematic](https://paper.dropbox.com/doc/Participate-in-the-Minimal-Fab-Design-Contest-Create-a-schematic-Nx6HLSLlAomupxZKQSsIH#:uid=669305565414356345640357&amp;h2=Create-a-schematic) . The goal of LVS is to match nand_sample.GDS in the [+Minimal Fab Design Contest: Completed Example](https://www.dropbox.com/scl/fi/brm8pu9swck2n0yt36smz/Participate-in-the-Minimal-Fab-Design-Contest.paper?rlkey=j39e30d1chc5rps2c2uq6vp8p&dl=0#:h2=Completed-Example), but the layout contains many floating MOS elements that were not used.

So, first, create a circuit diagram of just the floating MOS elements:

![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699321898732_image.png)


To import this into nand_min.sch, create a symbol by running Menu → Symbol → Make symbol from schematic.

I imported the created symbol and named it nand_min_with_floating_mosfets.sch.

![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699322127653_image.png)


**Create a netlist for LVS**
You can change the location of the netlist that Xschem generates. By default, it is ~/xschem/userConf/simulations, but if you select Menu → Simulation → Use 'simulation' dir under schematic current dir, simulation folder will be created and the netlist will be placed under the schematic folder.  You can check this netlist by going to Menu → Simulation → Edit netlist:

![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699324722847_image.png)

## Run LVS

**Execute get_reference**
Return to KLayout, run Menu → Macros → Get reference for ICPS2023_5 and select nand_min_with_floating_mosfets.spice in the simulation folder.
**Running LVS**

![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699325147295_image.png)


It is OK if it becomes solid green as shown in the image below.

![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699325707862_image.png)

## Submit design data for the contest

We have prepared a channel called Contest 2023 on the Discord dedicated server (ICPS PDK), so
please submit a complete set of circuit design data and layout GDS there.

![](https://paper-attachments.dropboxusercontent.com/s_B9AB128CD32F29A56B153513EAD5967CACC1D76117513D7A1DC8EF76295E1344_1699326120980_image.png)


　　　　　　　　　　　　　　　　　　　　　　　　　　　（2023/11/7　written by S. Moriyama）
