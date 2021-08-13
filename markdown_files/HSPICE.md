# HSPICE: How to understand

:TOC

## HSPICE - Introduction

SPICE is a general purpose analog circuit simulator that is used to verify circuit designs and to predict circuit behavior. This is of particular importance for "integrated circuits". It was for this reason that SPICE was originally developed at the Electronics Research Laboratory of the University of California, Berkeley (1975), as its name implies:

***S***imlulation ***P***rogram for ***I***ntegrated ***C***ircuit ***E***mphasis

HSPICE is a commercial and very powerful version of SPICE that has fewer bugs than the freely avaliable Berkeley SPICE. HSPICE is a relatively comprehensive program that can be used to simulate very large circuits comprising many different types of components. HSPICE is able to handle hierarchical circuit structures (i.e., you can define subcircuits) and supports both local and global variables/parameters.

Circuit analyses supported by HSPICE:
- Non-linear DC analysis: calculates the DC transfer curves from any input to any output.
- Non-linear transient analysis: calculates the voltages the currents as a function of time when a signal is applied.
- Linear AC Analysis: calculates the frequency response from the input to all specified node voltages and currents. Bode plots can be generated using a graphical post processor, such as AWAVES.
- Noise analysis
- Sensitivity analysis
- Distortion analysis
- Fourier analysis: calculates and plots the frequency spectrum
- Monte Carlo Analysis

Components supported by HSPICE:
- Independent and dependent voltage and current sources
- Resistors
- Capacitors
- Inductors
- Mutual Inductors
- Transmission Lines
- Operational Amplifiers
- Switches
- Diodes
- Bipolar Transistors
- MOSFET Transistors
- MESFET Transistors
- JFET Transistors
- etc.

## HSPICE - Files

### HSPICE Input Files
- source (nelist): filename.sp
- design configuration: filename.cfg
- initialization: hspice.ini

### HSPICE Output Files
- run status: filename.st0
- output listing: filename.lis
- graph data, transient: filename.tr# (.tr0)
- graph data, dc: filename.sw# (.sw0)
- graph data, ac: filename.ac# (.ac0)
- measure output: filename.m\*# (.mt0)

#### Source file (.sp)

The source file is your input file. This file contains your circuits description and all options and analysis you wish HSPICE to deal with. The structure looks like this:
```
title					- Implicit first line, must be there
.options				- set conditions for simulation
ANALYSIS and TEMP		- statements to sweep variables
.print/.plot/Analysis	- set print, plot, and analysis variables
.initial conditions		- input state of system
netlist					- circuit descriptions (components, input sources)
.model libraries		- .lib and .inc
.end					- terminates the simulation (always add a newline character)
```

#### Design configuration (.cfg)

Configuration files are used by HSPICE, AWAVES and HSPLOT to describe the available terminals and hardcopy devices.

#### Initialization (hspice.ini)

The initialization file deals primarily with setting up HSPICE itself and has no need to be modified by users.

#### Run status (.st0)

The run status file is a transcript of what operations HSPICE performs on a source file. The file sometimes offers help when debugging as shows steps not preformed by HSPICE.

#### Output listing - important (.lis)

This is one of the most important files in HSPICE as this file lists all results obtained from simulation. This file contains (in order of listing in the file):
1. HSPICE licensing info
2. Listing of the circuit
3. Results from the analysis of the circuit (.op, .print, .plot, .measure, .ac, and .tran in order of their appearance in the source file)

#### Graph data files (.tr#, .sw#, .ac#)

Graph date files are created by `.OPTION POST` command and contain the date to graphed by HSPLOT, AWAVES, HSPICE. Basically these files retain the data to be graphed from the .lis file in a different graphing format.

#### Measure output (.m\*#)

The measure output file hold the result from `.MEASURE` commands. These files are not very useful because the data is hard to read and already exits in a nicely formatted way in the .lis file.

## HSPICE - Source File

The HSPICE source file consists of five parts.
- *Title statement*: the first line is the circuits title; it may be anything, but not neglected.
- *Data statements*: description of the components and the interconnections.
- *Control statements*: tells SPICE what type of analysis to perform on the circuit.
- *Output statements*: specifies what outputs are to be printed or plotted.
- *End statement*: on the very last line: .END followed by a carriage return.

```
TITLE STATEMENT
ELEMENT STATEMENTS
.
.
COMMAND (CONTROL) STATEMENTS
OUTPUT STATEMENTS
.END
```

*Comment statements*: can be anywhere in the source file:
- '\*' - indicates a full line comment
- '$' - indicates and end-of-line comment

Continuation: '+' - indicates a continuation of the previous line.

### NOTATION

HSPICE notation is relatively simple. Both upper and lower case letters are allowed, but no distinction is made (HSPICE converts everything to upper-case characters before any interpretation is attempted).

The statements have a free format and consist of fields separated by a blank. Each line is interpreted as a separate statement.
1. All statements begining with a '.' are commands (.OPTION, .PRINT, .AC, etc.)
2. Statements that begin with an alphabetic character are treated as elements, the first character determines the type of element:
	- R for resistors
	- C for capacitors
	- L for inductors:
	- D for diodes
	- Q for bipolar junction transistors
	- M for MOS transistors
	- V for voltage sources
	- I for current sources
	- G for voltage-controlled current sources (tranconductance amplifiers)
	- F for current-controlled current sources (current amplifiers)
	- E for voltage controlled voltage sources (voltage amplifiers)
	- H for current-controlled voltage sources (tranresistance amplifiers)
	- X for subcircuits (hierarchical circuit structure)

### Topology Rules
- Every node must have a DC path to ground.
- No dangling nodes (all nodes must have at least two connections).
- No loops of only ideal voltage sources and capacitors.
- No nodes connected to only ideal current sources and inductors.

### Naming conventions
- Node Identification:
	- 0 is ALWAYS ground
	- It's best to start all node names (besides ground) with an alphabetic character.

### HSICE units
- f=1e-15
- p=1e-12
- n=1e-9
- u=1e-6
- m=1e-3
- k=1e3
- meg=x=1e6
- g=1e9
- t=1e12

### Global Variables
- Syntax
	- `.global node1 node2 node3` : globally defined nodes
	- `.global vbias vcc`		  : globally defined sources
- Usage
	- when subcircuits are included in the data file
	- assign common node name to subsircuit nodes
	- power supply connection of all subcircuits is often done this way:
		- `.global vcc`: connects all nodes named vcc (all circuits have a common node)

Essential globals are used most often for subcircuits, though they are great for sources. Notice that ground (node 0) always is a global node.

## HSPICE - Devices

### General Element Statements

The element statement specifies the type of element or independent source used. 
It has a field for the element name, the connecting nodes, a value for the
component, and optional parameters.

> General Form:   
```
NAME node1 node2 ... nodeN <model reference> value <optional parameters>
```

> **NAME**   
Specifies the type and name of element. The first letter in the name field
identifies the element as a specific type, the remaining letters give the element
a unique name.

> **node**   
Specifies how the element is connected in the netlist.

> **value**   
Gives the values of the element

> **<model reference>**   
Refers to the model when the basic element value does not provide an adequate
description.

> **<optional parameters>**   
Used to modify existing values within a `.model` for this paricular element.
In order to words a `.model` might have a temperature of 27 degrees when you
what 40 degrees Centigrade.

-----------------------
### `.model` Statements

`.model` statements allow extra device descriptions are required for accuracy.
For instance, a `.model` statement can be used to give resistors operating
temperatures, L and W values to MOSFET's, or even insulator thickness on a
capacitor. `.model` statements can be included any within the `.lis`, though
they are usually placed before the netlist for convenience.

> General Form:
```
.model mname modeltype<keyword=value>
```

> **mname**   
The model reference name specified in the associated element statement.

> **modeltype**   
Specifies the model type to be used, i.e., R for a resistro, C for a capacitor,
and L for inductor.

-----------------------
### `.param` Statements

`.param` statements allow the user to define local variables.
The variables can be defined as values, equations, or even functions.

> General Form:   
```
.param variable = 'expression'
	or
.param variable = value
```

> **variable**    
Represents the name of the variable or function to be defined.
> **expression**    
Represents the value, equation, or function to be assigned to the variable.

----------------------
### Devices
#### Passive
##### Resistors

The resistor elements use the following formats:
```
Rxxx n1 n2 Rval
Rxxx n1 n2 <mname> Rval <TC1=val> <TC2=val> <SCALE=val> <m=val>
+<AC=val> <DTEMP=val> <L=val> <W=val> <C=val>
Rxxx n1 n2 R='equation'
```

> **AC**   
AC resistance used in the AC analysis.    
default=Reff

> **C**    
Capacitance connected from node n2 to bulk.    
default=0.0

> **DTEMP**    
Element and circuit temperature difference.    
default=0.0

> **equation**    
The resistor can be described as a function of any node voltage, branch currents
and any independent variables such as TIME, frequency (HERTZ), or temperature
(TEMPER)

> **L**    
Resistor length.    
default=0.0

> **M**    
Multiplier used to simulate parallel resistors.    
default=1

> **mname**    
Model name.

> **n1**     
Positive terminal node name.

> **n2**     
Negative terminal node name.

> **R**     
Resistance.

> **Rxxx**      
resistor element name. Must begin with 'R'.

> **SCALE**    
Element scale factor for resistance and capacitance.    
default=1

> **TC1**     
1st order Temp. Coeff. for resistor.

> **TC2**     
2nd order Temp. Coeff. for resistor.

> **W**    
Resistor width.    
default=0.0

-------------------------
##### Capacitors

The capacitor elements use the following formats:
```
Cxxx n1 n2 capval
Cxxx n1 n2 <mname> capval <TC1> <TC2> <SCALE=val> <IV=val> <M=val>
+<DETMP=val> <L=val> <W=val>
Cxxx n1 n2 <mname> C=val <TC1=val> <TC2=val> <SCALE=val> <M=val>
+<DTEMP=val> <L=val> <W=val>
Cxxx n1 n2 C='equation' CTYPE=0 or 1
```

> **Cxxx**    
Capacitor element name. Must begin with an 'C'.

> **C**    
Capacitance in farads at room temperature.

> **CTYPE**    
For capacitance, if C is a function of v(n1,n2),then CTYPE=0 else =1.
The capacitance charge is calculated differently depending on the value of
CTYPE.

> **IV**     
Initial voltage across the capacitor in volts.

The rest can refer to resistor which the definitions are the same.

*Note: Inductor is skipped since I don't use it in my design.*

-------------------------
#### Active
##### Diodes

Diodes in HSPICE can be anywhere from very complex realistic diodes to simple
ideal ones depending on the circuit designer. In general all diodes will have a
model name and be defined by a `.model` statement. Since most models are
provided, `.model` descriptions will not be given in any detail.
The diode element uses the following formats:
```
Dxxx nplus nminus mname <options>
Dxxx nplus nminus mname <AREA=val> <PJ=val> <WP=val> <LP=val> <WM=val>
+<LM=val> <OFF> <IC=vd> <M=val> <DTEMP=val>
Dxxx nplus nminus mname <AREA=val> <L=val> <WP=val> <LP=val> <WM=val>
+<LM=val> <OFF> <IC=vd> <M=val> <DTEMP=val>
```

> **AREA**     
Represents the area of the diode.    
default=1

> **Dxxx**    
Diode element name, must begin with an 'D'.

> **nplus**     
Positive terminal name (arrow end) of the diode.

> **nminus**      
Negative terminal name (line end) of the diode.

> **PJ**     
Periphery of junction.

> **WP**     
Polysilicon capacitor width (in meters).

> **LP**     
Polysilicon capacitor length (in meters).

> **WM**        
Metal capacitor width (in meters).

> **LM**    
Metal capacitor length (in meters).

> **OFF**     
Sets the diode initially to the off operation region (DC analysis).    
default=ON

> **IC**       
Initial diode voltage.

The rest options can be refer to resistor.

*BJT is skipped.*

-------------------------
##### MOSFETS

MOSFETs are another device within HSPICE that requires a `.model` statement.
The `.model` statement is required to define whether or not the circuit designer
want to deal with a p- or n-channel device. The syntax for the `.model` statement
is shown below:
```
.model mname NMOS <or PMOS> <parameters>
```
Only the model name and the NMOS (or PMOS) are required. The MOSFET transistor element uses the following formats:
```
Mxxx nd ng ns mname
Mxxx nd ng ns <nb> mname <L=val> <W=val> <AD=val> <AS=val> <PD=val>
+<PS=val> <NRD=val> <NRS=val> <RDC=val> <RSC=val> <OFF>
+<IC=vds,vgs,vbs> <M=val> <DTEMP=val> <GEO=val> <DELVTO=val>
Mxxx nd ng ns <nb> mname lval wval ...
```

> **nd**     
Node name for drain terminal. 

> **nd**     
Node name for gate terminal.

> **ns** 
Node name for source terminal.

> **nb**     
Node name for the bulk terminal.

> **AD**     
Diffusion area of the drain.

> **AS**      
Diffusion area of the source.

> **PD**     
Drain junction perimeter (including channel edge).    
default=0

> **PS**     
Source junction perimeter (including channel edge).     
default=0

> **NRD**
Number of resistance squares of drain diffusion.     
default=0

> **NRS**    
Number of resistance squares of source diffusion.    
default=0

> **RDC**     
Contact resistance on the drain.    
default=0

> **RSC**    
Contact resistance on the source.    
default=0

> **OFF**    
Sets the transistor initially to the off operation region (DC analysis).    
default=ON

> **vbs,vds,vgs**     
- vbs: initial bulk-source voltage
- vds: initial drain-source voltage
- vgs: initial gate-source voltage

> **DELVTO**    
Zero-bias threshold voltage shift.    
default=0

> **GEO**    
Source-drain sharing selector (only use for ACM=3 in `.model`).    
default=0

------------------------
### Independent sources
#### General Form 

In general, voltage and current sources in HSPICE can be very almost anything
from simple dc sources to incredibly complex piecewise linear ones. The general
form for both current and voltage sources are shown below:
```
* Voltage
Vxxx n+ n- <DC=>dcval
Vxxx n+ n- <AC=>acmag, acphase
Vxxx n+ n- tranfun
* Current
Ixxx n+ n- <DC=>dcval
Ixxx n+ n- <AC=>acmag, acphase
Ixxx n+ n- tranfun
```

> **tranfun**    
Transient source function (i.e. PULSE, PU, SIN, EXP, PWL, PL).

--------------------------
#### Pulse Function

The pulse function can create either a linear rectangular or linear trapazoidal
pulse with an initial time delay. The function also has user defined rise, fall,
and high times. The functions are placed in the tranfun position listed above.
The pulse function has the form:
```
PULSE (v1 v2 <td> <tr> <tf> <pw> <per>)
PU (v1 v2 <td> <tr> <tf> <pw> <per>)
```

> **v1**    
Initial value of voltage or current before the pulse.

> **v2**     
Pulse current or voltage value.

> **td**     
Time delay for the start of the transient interval to the up ramp.    
default=0

> **tr**     
Rise time.     
default=TSTEP

> **tf**     
Fall time.        
default=TSTEP

> **pw**      
Pulse width.    
default=TSTEP

> **per**     
Pulse repetition period in seconds.       
default=TSTEP (transient timestep)

-----------------------------
#### Sin Function

The HSPICE sin function can produce a sin wave input with user defined delay,
magnitude and phase. Also, HSPICE can also produce exponentially damped sin
sources.
```
sin (vo va freq td damp<pd>)
```

> **vo**    
Wave offset.

> **va**    
wave amplitude.

> **freq**     
Wave frequency in Hz.    
default=1/TSTOP

> **td**    
Wave time delay in seconds.    
default=0

> **damp**    
Wave damping factor in 1/seconds.    
default=0

> **pd**    
Phase delay in degrees (*not radians*).    
default=0

-------------------------------
#### Exponential Function

The exp function produces an exponential with user definable rise time and fall
time constants and delay values.
```
exp (v1 v2 <td1> <t1> <td2> <t2>)
```

> **v1**    
Starting voltage or current.

> **v2**     
Pulse value of voltage or current.

> **td1**    
Rise delay time in seconds.    
default=0

> **t1**    
Rise time constant in seconds.    
default=TSTEP

> **td2**    
Fall delay time in seconds.     
default=td1+TSTEP

> **t2**     
Fall time constant in seconds.    
default=TSTEP

-------------------------------
#### Piecewise Linear Function

The HSPICE piecewise linear function is a collection of user defined pulses and
steps. All plateaus and rise and fall times are also user definable. In addition,
when moving between differing voltages or currents, the function will always
proceed linearly.
```
PWL (v1 t1, <v2 t2, v3 t3, ...> <R=<repeat>> <TD=delay>)
```

> **R**   
Forces the function to repeat.    

> **repeat**     
The start time for the function to repeat in seconds.

> **t1...**    
The time value in seconds.

> **TD**    
Keyword for the function time delay.

> **delay**    
Actual time delay in seconds.

> **v1...**    
Current or voltage values.

**Note**: each voltage and time come in pairs.

## HSPICE - Commands

**Commands or Control Statements to Specify the Type of Analysis**

### `.op` Statement

This statement instructs SPICE to compute the DC operating points:
- voltage at the nodes
- current in each voltage source
- operating point for each element

In HSPICE it is usually not necessary to specify `.op` as it gives you
automatically the DC node voltages. However, HSPICE does not give the DC voltages
unless you have specified a certain analysis type, such as for instance `.tran`
or `.ac` analysis (SPICE automatically does a DC analysis before doing a
transient or AC analysis). Thus, if you are only interested in the DC voltages in
HSPICE, you should specify the `.op` option, or the `.dc` option.

### `.dc` Statement

This statement allows you to increment (sweep) an independent source over a
certain range with a specified step. The format is as follows:
```
.dc SRCname START STOP STEP
```
in which `SRCname` is the name of the source you want to vary; `START` and `STOP`
are the starting and ending value, respectively; and `STEP` is the size of the
increment.

### `.tf` Statement

The `.tf` statement instructs HSPICE to calculate the following small signal
characteristics:
1. the ratio of output variable to input variable (gain or transfer gain)
2. the resistance with respect to the input source
3. the resistance with respect to the output terminals

`.tf outvar insrc` in which `outvar` is the name of the output variable and
`insrc` is the input source.

### `.sens` Statement

This instructs HSPICE to calculate the DC small-signal sensitivities of each
specified output variable with respect to *every* circuit parameter.
```
.sens variable
```	 
### `.tran` Statement

This statement specifies the time interval over which the transient analysis
takes place, and the time increments. The format is as follows:
```
.tran TSTEP TSTOP <TSTART <TMAX>> <UIC>
```

- `TSTEP` is the printing increment.
- `STEOP` is the final time.
- `TSTART` is the starting time (if omitted. TSTART is assumed to be zero)
- `TMAX` is the maximum step size.
- `UIC` stands for Use Initial Condition and instructs HSPICE not to do the
quiescent operating point before begining the transient analysis. If UIC is specified, HSPICE will use the initial conditions specified in the element statements.

### `.ic` Statement

This statement provides an alternative way to specify initial conditions of nodes
(and thus over capacitors).
```
.ic Vnode1 = value Vnode2 = value
```

### `.ac` Statement

This statement is used to specify the frequency (AC) analysis. The format is as
follows:
```
.ac LIN NP FSTART FSTOP
.ac DEC ND FSTART FSTOP
.ac OCT NO FSTART FSTOP
```
in which `LIN` stands for a linear frequency variation, `DEC` and `OCT` for a
decade and octave variation respectively. `NP` stands for the number of points
and `ND` and `NO` for the number of frequeny points per decade and octave.
`FSTART` and `FSTOP` are the start and stopping frequencies in Herz.

### Output Statements

These statements will instruct HSPICE what output to generate. If you do not
specify an output statement, HSPICE will always calculate the DC operating
points. The two type of outputs are the prints and plots. A print is a table of
data points and a plot is a graphical representation. The format is as follows:
```
.print TYPE OV1 OV2 OV3
.plot TYPE OV1 OV2 OV3
```
in which `TYPE` specifies the type of analysis to be printed or ploted and can
be:
- DC
- TRAN
- AC

The output variables are OV1, OV2 and can be voltage or currents. Node voltages
and device currents can be specified as magnitude (M), phase (P), real (R) or
imaginary (I) parts by adding the suffix to V or I as follows:
- M: magnitude
- DB: magnitude in dB (deciBells)
- P: phase
- R: real part
- I: imaginary part

## NOTES

1. Know what to expect from the simulation before running it.
2. Simulation is NO substitute for THINKING.

### Other helpful hints
1. You can measure the voltage at any node, or the voltage between any two nodes.
However, you can only measure the current passing through a voltage source. To
measure a current through a branch, insert a dummy voltage source of value 0 in
the branch (like an ammeter) and measure the current through this dummy voltage
source.
2. To extract the data from the `.lis` file, have `.print` statements. Use
`.options ingold` so that the data is in the exponent notation. Just edit the
`.lis` file that you get to have the data suitable for MatLab.
