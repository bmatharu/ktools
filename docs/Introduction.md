# Introduction
The purpose of this project is to provide a Specification and Reference Model of the POSIX in-memory variant of the Oasis Kernel.  It is being developed primarily at this stage for the Oasis Solutions Project (OSP) but is a generic design.
Release 1.3 (and later releases) includes an “in-memory” version of the Oasis central calculations (termed the “Kernel”) written in C++ and C to provide streamed calculation at high computational performance.  Prior releases of Oasis used SQL-based back-ends with SQL code generated by the mid-tier libraries.  The new variant was delivered as a stand-alone toolkit with R1.3 but in future releases, the compiled code will be distributed by the mid-tier.
As well as the “GUL sample” processing, the Kernel also performs numerical integration and in-memory Financial Module (FM) processing and outputs.

### Architecture

The Kernel is provided as a toolkit of components (“ktools”) which can be invoked at the command line.  Each component is a separately compiled executable with a binary data stream of inputs and outputs.  The components can also be wrapped together to provide a secure compiled executable if required.
The principle is to stream data through by event end-to-end, with multiple processes being used either sequentially or concurrently, at the control of the user using a script file appropriate to the operating system.

### Language

The components can be written in any language as long as the input and output data structures of the compiled executable are adhered to.  The current set of components have been written in POSIX-compliant C++ and C.  This means that they can be compiled for instance in Linux and Windows.

### Components

The set of components in the Reference Model provided in this release is as follows;
* **eve** is the process distributing utility. Based on the number of events in the input and the number of processes specified as a parameter, eve distributes the events to the processes. The output streams into getmodel.
* **getmodel** is a CDF plug-in to generate the CDFs for a specified list of events. It is the reference example of a plug-in using Oasis kernel format data in binary format. getmodel streams into gulcalc or can be output to a binary file.
* **gulcalc** is the core component which performs the GUL sampling calculations and numerical integration. The output is the Oasis format gul results table. This can be output to a binary file or streamed into another plug-in component, such as a Financial Module or output calculation.
* **fmcalc** is the core component which performs the insured loss calculations from the input ground up loss samples. It has the same functionality as the Oasis Financial Module.  The output is the Oasis format FM results table. This can be output to a binary file or streamed into an output calculation.
* **outputcalc** performs results analysis on the ground up loss or insured loss samples, and exports the results table to a csv file. The example given produces an event loss table.
* **cdftocsv** is a utility to output binary format CDFs to a csv.
* **gultocsv** is a utility to output binary format GULs to a csv.
* **fmtocsv** is a utility to output binary format losses to a csv. 

### Usage

Standard piping syntax can be used to invoke the components at the command line. For example the following command invokes eve, getmodel, gulcalc, fmcalc, and outputcalc, and exports the results to a csv file.
``` sh
$ eve 1 1 2 | getmodel 1 | gulcalc –S100 –C1 | fmcalc | outputcalc > output.csv
```

Example bash shell, python and vbs scripts are provided along with a binary data package in the /examples folder to demonstrate usage of the toolkit.

[Back to Contents](Contents.md)