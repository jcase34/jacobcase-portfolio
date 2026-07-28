---
title: "Hard Drive Disk Controller Board Design"
description: "Electrical design and validation of the controller board for enterprise HDD products, from schematic through mass production."
featured_image: "/images/album_seagate/tatsu-hdd.png"
tags: ["Electronics", "PCB Design", "PCBA", "Mentor Graphics", "Signal Integrity"]
---

With a background in electrical engineering focused on enterprise storage, I led the design, validation, and testing of controller boards for next-generation hard disk drives — from schematic capture through multi-layer PCB layout, prototyping, and mass production with overseas contract manufacturers.

### Tools I Use

- LTSpice
- Mentor Graphics
- DSA / Oscilloscopes
- Network Analyzers
- Power / Signal Integrity Analysis

### Design, Development, Bring-up
Designs started from requirements. After full list of requirements gathered from the core business unit, schematic capture and preliminary board layout were completed. Following layout, LTSpice and other simulation tools were used to evaluate signal and power integrity. 

Following completion of pre-build board simulation, prototypes were fabricated locally in Colordo, and assembled on-site for testing. Validation of signal and power integrity were completed, along with early stage HDD motor spin, read, and write processes via SOC -> DRAM. 

Once prototyping completed, small batch prodution units were configured to supplement other functional team testing and bring-up. In our lab, bring-up included power, signal integrity validation running on HDD, memory speed testing, voltage regulator analysis at load, read-write performance, EMI, ESD, and power consumption and reliabiilty tests under temperature. 

### Cross Functional Work - China & Singapore
I met weekly with overseas CMs on both PCB and PCBA production. Included prototyping, assembly, test, and verification of new 6-layer HDI manufacturing for space optimized designs. 
{{< iframe id="seagate_map" src="/maps/seagate_map.html" >}}

### Gallery

{{< gallery dir="images/album_seagate" >}}