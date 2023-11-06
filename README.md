# Pes_8_bit_bcd
## 1. Introduction <br />
The 8 bit Binary Coded Decimal (BCD) Counter is a counter that counts 100 digits starting from 0 to 99.BCD is an encoding where each digit in a decimal number is represented in the form of bits(usually 4 bits). For example the number 89 can be represented as 10001001 in BCD as 1000 is the BCD representation of 8 and 1001 is the BCD representation of 9.BCD code is also known as 8421 BCD code. This also makes it a weighted code which implies that each bit in the four bit groups representing each decimal digit has a specific weight. As compared to prevalent binary positioning system it’s easy to convert it into human readable representation with the drawback of slight increase in complexity of the circuits.<br />
<br />
## 2. Application of BCD Counter<br />
The BCD Counter finds application in clock production, clock division, used in minimal power cmos circuit, implemented in frequency counting circuits.<br />
<br />
## 3. Verilog Implementation of BCD Counter <br />
The 8 bit BCD counter counts from 00000000(0) to 10011001(99). After that it resets to initial value 0 and the process is repeated again. The Verilog code contains 8 bit output and clock, reset & enable as input. The 8 bit BCD counter block diagram is shown in Fig1 and the port list are given in Table1.In Fig2 output waveform for few clock cycles is displayed.<br />
<br />
The block diagram of BCD counter is shown below.
<p align="center">
  <img width="350" height="200" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/blockdiagram.png">
</p><br>
<br />

The port description is given below.
<p align="center">
  <img width="350" height="200" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/Table1.png">
</p><br>
<br />

## 4. Functional simulation <br />

### 4.1 Softwares  <br />
### - **Iverilog**
Icarus Verilog is a Verilog simulation and synthesis tool. It operates as a compiler, compiling source code written in Verilog (IEEE-1364) into some target format.

### - **GTKwave**
GTKWave is a fully featured GTK+ v1. 2 based wave viewer for Unix and Win32 which reads Ver Structural Verilog Compiler generated AET files as well as standard Verilog VCD/EVCD files and allows their viewing.


### Installing necessary softwares:
  ```
  sudo apt-get install git 
  sudo apt-get install iverilog 
  sudo apt-get install gtkwave 
  ```
### 4.2 Steps & Results:
steps for functional-simulation:-<br />
1.To clone the respository and download the netlist files for simulation, enter the following commands in your terminal<br />
``` 
 $ sudo apt install -y git
 $ git clone https://github.com/sanjanaharish18/Pes_8_bit_bcd.git
```
 
2.To run the simulation use the following commands:- <br />
``` 
iverilog pes_bcdc.v pes_bcdc_tb.v
./a.out
gtkwave pes_bcdc.vcd
 ```
 The output waveform is given below.
 <p align="center">
  <img width="800" height="200" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/waveform.jpeg">
</p><br>
<br />

## 5. Synthesis

### 5.1 Softwares used

#### yosys – Yosys Open Synthesis Suite


This is a framework for RTL synthesis tools. It currently has
extensive Verilog-2005 support and provides a basic set of
synthesis algorithms for various application domains.

Yosys can be adapted to perform any synthesis job by combining
the existing passes (algorithms) using synthesis scripts and
adding additional passes as needed by extending the yosys C++
code base.

Yosys is free software licensed under the ISC license (a GPL
compatible license that is similar in terms to the MIT license
or the 2-clause BSD license).
#### **Installing Prerequsites for Yosys**
 ```
 sudo apt-get install build-essential clang bison flex \
	libreadline-dev gawk tcl-dev libffi-dev git \
	graphviz xdot pkg-config python3 libboost-system-dev \
	libboost-python-dev libboost-filesystem-dev zlib1g-dev
```

#### **Installing Latest Version of Yosys**

```
git clone https://github.com/YosysHQ/yosys.git
make
sudo make install 
make test
```
### 5.2. Run Synthesis

The commands to run synthesis in yosys are given below. First create an yosys script `yosys_run.sh` and paste the below commands.

```
read_liberty -lib lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog pes_bcdc.v
synth -top pes_bcdc	
dfflibmap -liberty lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty lib/sky130_fd_sc_hd__tt_025C_1v80.lib
clean
flatten
write_verilog -noattr pes_bcdc_net.v
stat
show
```

```
yosys -s yosys_run.sh
```
After running the above commands we get the following results. 
 <p align="center">
  <img width="350" height="400" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/stats.jpg">
</p><br>
<br />
 <p align="center">
  <img width="1200" height="500" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/show.jpg">
</p><br>
<br />
 
 ## 6. Gate Level Simulation GLS <br />
 GLS stands for gate level simulation. When we write the RTL code, we test it by giving it some stimulus through the testbench and check it for the desired specifications. Similarly, we run the netlist as the design under test (dut) with the same testbench. Gate level simulation is done to verify the logical correctness of the design after synthesis. Also, it ensures the timing of the design. <br>
Commands to run the GLS are given below.
 ```
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 verilog_model/primitives.v verilog_model/sky130_fd_sc_hd.v bcdc_net.v bcdc_tb.v
./a.out --> For Generating the vcd file.
gtkwave pes_bcdc.vcd
```
The post GlS waveform is given below.
 <p align="center">
  <img width="800" height="200" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/waveform2.jpg">
</p><br>
<br />

## 7. Physical Design - RTL to GDSII

### 7.1 ASIC design flow

The ASIC flow objective is to convert RTL design to GDSII format used for final layout. The flow is essentially a software also known as automated PnR (Place & route).

The Simplified RTL2GDS Flow is given below.
<p align="center">
  <img width="600" height="200" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/flow.png">
</p><br>
<br />

### 7.2 Creating Custom Cell
First, clone the github repo containing the inverter and prepare for the next steps.
```
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
cd vsdstdcelldesign
cp ./libs/sky130A.tech sky130A.tech
magic -T sky130A.tech sky130_inv.mag &
```
On typing the following commands, the following netlist will open.

<p align="center">
  <img width="800" height="600" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/layout1.jpg">
</p><br>

Now, to extract the spice netlist, type the following commands in the tcl console. Here, parasitic capacitances and resistances of the inverter is extracted by  `cthresh 0 rthresh 0`.
```
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```
<p align="center">
  <img width="800" height="600" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/tkcon2.3.jpg">
</p><br>

The extracted spice model is shown below (which is edited to simulate the inverter).
```
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=0.01u
.include ./libs/pshort.lib
.include ./libs/nshort.lib


M1001 Y A VGND VGND nshort_model.0 ad=1435 pd=152 as=1365 ps=148 w=35 l=23
M1000 Y A VPWR VPWR pshort_model.0 ad=1443 pd=152 as=1517 ps=156 w=37 l=23
VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)
C0 Y VPWR 0.08fF
C1 A Y 0.02fF
C2 A VPWR 0.08fF
C3 Y VGND 0.18fF
C4 VPWR VGND 0.74fF


.tran 1n 20n
.control
run
.endc
.end
```


<p align="center">
  <img width="800" height="600" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/trans.jpg">
</p><br>

To get a grid and to ensure the ports are placed correctly we type the following command in the tcl console
```
grid 0.46um 0.34um 0.23um 0.17um
```
In Magic Layout window, first source the .mag file for the design (here inverter). Then Edit >> Text which opens up a dialogue box. Then do the steps shown in the below figure.

<p align="center">
  <img width="800" height="600" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/textA.jpg">
</p><br>

<p align="center">
  <img width="800" height="600" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/texY.jpg">
</p><br>

<p align="center">
  <img width="800" height="600" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/textvpwr.png">
</p><br>

<p align="center">
  <img width="800" height="600" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/textvgnd.png">
</p><br>

Now, to extract the lef file and save it, type the following command.
```
lef write
```
<p align="center">
  <img width="800" height="600" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/tkcon2.3_2.png">
</p><br>

The extracted lef file is shown below.
```
VERSION 5.7 ;
  NOWIREEXTENSIONATPIN ON ;
  DIVIDERCHAR "/" ;
  BUSBITCHARS "[]" ;
MACRO sky130_vsdinv
  CLASS CORE ;
  FOREIGN sky130_vsdinv ;
  ORIGIN 0.000 0.000 ;
  SIZE 1.380 BY 2.720 ;
  SITE unithd ;
  PIN A
    DIRECTION INPUT ;
    USE SIGNAL ;
    ANTENNAGATEAREA 0.165600 ;
    PORT
      LAYER li1 ;
        RECT 0.060 1.180 0.510 1.690 ;
    END
  END A
  PIN Y
    DIRECTION OUTPUT ;
    USE SIGNAL ;
    ANTENNADIFFAREA 0.287800 ;
    PORT
      LAYER li1 ;
        RECT 0.760 1.960 1.100 2.330 ;
        RECT 0.880 1.690 1.050 1.960 ;
        RECT 0.880 1.180 1.330 1.690 ;
        RECT 0.880 0.760 1.050 1.180 ;
        RECT 0.780 0.410 1.130 0.760 ;
    END
  END Y
  PIN VPWR
    DIRECTION INOUT ;
    USE POWER ;
    PORT
      LAYER nwell ;
        RECT -0.200 1.140 1.570 3.040 ;
      LAYER li1 ;
        RECT -0.200 2.580 1.430 2.900 ;
        RECT 0.180 2.330 0.350 2.580 ;
        RECT 0.100 1.970 0.440 2.330 ;
      LAYER mcon ;
        RECT 0.230 2.640 0.400 2.810 ;
        RECT 1.000 2.650 1.170 2.820 ;
      LAYER met1 ;
        RECT -0.200 2.480 1.570 2.960 ;
    END
  END VPWR
  PIN VGND
    DIRECTION INOUT ;
    USE GROUND ;
    PORT
      LAYER li1 ;
        RECT 0.100 0.410 0.450 0.760 ;
        RECT 0.150 0.210 0.380 0.410 ;
        RECT 0.000 -0.150 1.460 0.210 ;
      LAYER mcon ;
        RECT 0.210 -0.090 0.380 0.080 ;
        RECT 1.050 -0.090 1.220 0.080 ;
      LAYER met1 ;
        RECT -0.110 -0.240 1.570 0.240 ;
    END
  END VGND
END sky130_vsdinv
END LIBRARY

```

### 7.3 Synthesis:
Now, to run synthesis, type the following command
```
run_synthesis
```
<p align="center">
  <img src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/step1%262.png">
</p><br>

Here, we notice that our custom cell `sky130_vsdinv` is displayed in the netlist generated.
<p align="center">
  <img width="500" height="400" src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/nowire.jpg">
</p><br>



### 7.4 Floorplan & Placement

Also, sta report post synthesis can be viewed by going to the location `logs\synthesis\2-sta.log`

The next step is to run `floorplan` and `placement`. Type the following commands.
```
run_floorplan
run_placement
```

<p align="center">
  <img src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/floorplansteps.jpg">
</p><br>

The floorplan can be viewed by typing the following command.

<p align="center">
  <img src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/magic.jpg">
</p><br>

<p align="center">
  <img src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/floorplandia.png">
</p><br>
<p align="center">
  <img src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/core.jpg">
</p><br>
<p align="center">
  <img src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/die.jpg">
</p><br>


The placement can be viewed by typing the following command.
```
magic -T /home/sritam/Desktop/vsdflow/work/tools/openlane_working_dir/OpenLane/pdks/volare/sky130/versions/e8294524e5f67c533c5d0c3afa0bcc5b2a5fa066/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef fed read iiitb_bcdc.def &
```
<p align="center">
  <img src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/focused.jpg">
</p><br>

<p align="center">
  <img src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/skyvsd.jpg">
</p><br>
It can also be viewed in the pes_bcdc.def
<p align="center">
  <img src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/compact.png">
</p><br>

### 7.5 Clock Tree Synthesis

The next step is to run run clock tree synthesis. The CTS run adds clock buffers in therefore buffer delays come into picture and our analysis from here on deals with real clocks. To run clock tree synthesis, type the following commands

```
run_cts
```
<p align="center">
  <img src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/cts.png">
</p><br>
<p align="center">
  <img src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/clocktree1.jpg">
</p><br>
<p align="center">
  <img src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/clocktree2.jpg">
</p><br>



### 7.6 Routing
The command to run routing is 
```
run_routing
```
<p align="center">
  <img src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/runrouting.png">
</p><br>

<p align="center">
  <img src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/decap.jpg">
</p><br>
<p align="center">
  <img src="https://github.com/sanjanaharish18/Pes_8_bit_bcd/blob/main/compact.png">
</p><br>

