---
title: Module Schematic
---

## Overview

This schematic is design to function as an onboard bluetooth low energy server for connecting to a remote control unit. 
4 high-current JST headers are provided for motor subsystems that may require more power from the battery, capable of 10A each. The battery itself is on a disconnectable JST header as well, and jumpers are provided for enabling/disabling shared power, USB power and the barrel jack supply.
A spare 8-pin header is provided for redundancy. A solder bridge is used to control whether it is RX (upstream) or TX (downstream).


![schematic](A3V4.png){style width:"350" height:"300;"}
**Figure ##:** Showing Subsystem A3's schematic

PCB design:
Front:<br>
![](topPCB.png)
<br>
Back:<br>
![](backPCB.png)


## Resouces

The schematic as a PDF download is available [*here*](A3V4.pdf), and the zip folder of the project [*here*](A3V4.zip).
A version of the schematic including a battery charging circuit for lithium-ion batteries that was shelved due to complexity and time constraints can be found as a PDF [*here*](A3V1.pdf) and a zip folder [*here*](A3V1.pdf). See [component selection](https://nsgarde.github.io/NeelGarde/03-Component-Selection/Component-Selection/) for more details.
