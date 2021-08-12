# HSPICE: How to understand

## HSPICE - Introduction

SPICE is a general purpose analog circuit simulator that is used to verify circuit designs and to predict circuit behavior. This is of particular importance for "integrated circuits". It was for this reason that SPICE was originally developed at the Electronics Research Laboratory of the University of California, Berkeley (1975), as its name implies:

Simlulation Program for Integrated Circuit Emphasis

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
	- It;'s best to start all node names (besides ground) with an alphabetic character.

