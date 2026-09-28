# HIFLY CAD — Overall Assembly

## 1. Assembly Overview

The HIFLY overall assembly represents the physical integration of the thermal, electrical, sensing, communication, and environmental-protection subsystems into a common mechanical architecture.

The assembly brings together:

- battery system
- Pulsating Heat Pipe (PHP)
- heating element
- thermal insulation
- temperature sensing
- controller electronics
- MOSFET switching stage
- voltage and current monitoring
- LilyGO T3-S3 LoRa hardware
- antenna
- enclosure
- flexible protection
- lightweight protective structure

```text
                         HIFLY OVERALL ASSEMBLY
                                  │
          ┌───────────────────────┼───────────────────────┐
          │                       │                       │
          ▼                       ▼                       ▼
     BATTERY SYSTEM         THERMAL SYSTEM        PROTECTION SYSTEM
          │                       │                       │
          │                  ┌────┼────┐            ┌────┼────┐
          │                  │    │    │            │    │    │
          │                  ▼    ▼    ▼            ▼    ▼    ▼
          │                 PHP Heater Insulation Enclosure Silicone
          │
          ▼
   ELECTRICAL / CONTROL
          │
     ┌────┼──────────────┐
     │    │              │
     ▼    ▼              ▼
    MCU  MOSFET       Power Monitoring
     │
     ▼
   LoRa Module
     │
     ▼
   Antenna
```
2. Assembly Purpose

The purpose of the overall CAD assembly is to provide a common physical representation of the HIFLY system.

The assembly connects the system-level architecture to the physical arrangement of the prototype.

Problem
  │
  ▼
System Requirements
  │
  ▼
HIFLY Architecture
  │
  ▼
Subsystem Designs
  │
  ▼
CAD Assembly
  │
  ▼
Physical Integration

The assembly therefore provides the mechanical context required to understand how the individual HIFLY subsystems interact.

3. Major Assembly Components

The overall assembly contains the following major functional groups:

Assembly Group	Main Elements
Battery system	Li-ion battery pack and associated interfaces
Thermal system	PHP, heater and insulation
Sensing	Temperature, voltage and current sensing
Control	Microcontroller and control electronics
Power switching	MOSFET switching stage
Communication	LilyGO T3-S3 LoRa and antenna
Protection	Enclosure, flexible silicone and protective layer
Integration	Mechanical interfaces and component placement
4. Assembly Architecture
┌─────────────────────────────────────────────────────────────┐
│                     HIFLY ASSEMBLY                         │
│                                                             │
│  ┌───────────────────────────────────────────────────────┐  │
│  │                   PROTECTION LAYER                    │  │
│  │                                                       │  │
│  │   Enclosure • Insulation • Flexible Protection       │  │
│  │   Lightweight Protective Structure                    │  │
│  │                                                       │  │
│  │   ┌────────────────────────────────────────────────┐  │  │
│  │   │              INTERNAL SYSTEM                  │  │  │
│  │   │                                                │  │  │
│  │   │  ┌──────────────┐       ┌──────────────────┐ │  │  │
│  │   │  │   BATTERY    │       │ THERMAL SYSTEM   │ │  │  │
│  │   │  │              │       │                  │ │  │  │
│  │   │  │  Li-ion Pack │◄─────►│ PHP + Heater     │ │  │  │
│  │   │  └──────┬───────┘       └────────┬─────────┘ │  │  │
│  │   │         │                        │           │  │  │
│  │   │         ▼                        ▼           │  │  │
│  │   │  Temperature Sensors      Thermal Interface │  │  │
│  │   │         │                                │  │  │
│  │   │         └────────────┬───────────────────┘  │  │  │
│  │   │                      ▼                      │  │  │
│  │   │               ┌──────────────┐              │  │  │
│  │   │               │ MCU / CONTROL│              │  │  │
│  │   │               └──────┬───────┘              │  │  │
│  │   │                      │                      │  │  │
│  │   │          ┌───────────┴───────────┐          │  │  │
│  │   │          ▼                       ▼          │  │  │
│  │   │      MOSFET Stage          LoRa / Antenna   │  │  │
│  │   │                                                │  │  │
│  │   └────────────────────────────────────────────────┘  │  │
│  │                                                       │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
5. Battery Integration

The battery is positioned as a central subsystem because it provides the electrical energy required by the active and electronic portions of HIFLY.

The assembly considers the battery in relation to:

thermal insulation
heating element
PHP
temperature sensing
electrical connections
enclosure
control electronics
                   BATTERY
                      │
       ┌──────────────┼──────────────┐
       │              │              │
       ▼              ▼              ▼
   Thermal          Electrical     Mechanical
   Protection       Connection     Packaging
       │              │              │
       ▼              ▼              ▼
   Insulation       Controller     Enclosure
   PHP              Power Path
   Heater

The CAD assembly provides the physical relationship between these elements.

6. Battery and Thermal System

The battery is integrated with the thermal-management system rather than treated as an isolated electrical component.

              ┌─────────────────┐
              │     BATTERY     │
              └────────┬────────┘
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
       Heater         PHP       Insulation
          │            │            │
          └────────────┼────────────┘
                       │
                       ▼
                 Thermal System
                       │
                       ▼
                Temperature Sensor

This arrangement allows the CAD assembly to represent both energy storage and thermal-management relationships.

7. PHP Integration

The Pulsating Heat Pipe is integrated into the thermal structure around the relevant heat-transfer region.

                   PHP
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
    Thermal Source       Thermal Region
          │                   │
          └─────────┬─────────┘
                    ▼
              Battery/System

The PHP geometry is represented in the assembly as a physical thermal-management element.

The CAD model describes geometry and placement; it does not by itself establish the PHP's experimental thermal performance.

8. Heater Integration

The heating element is mechanically integrated with the thermal-management structure.

The functional relationship is:

Controller
    │
    ▼
MOSFET Switching Stage
    │
    ▼
Heating Element
    │
    ▼
Thermal Interface
    │
    ▼
Battery / Thermal Structure

The heater is therefore part of both the electrical and thermal assembly.

9. Insulation Integration

Thermal insulation forms part of the physical thermal-protection structure.

External Environment
        │
        ▼
┌──────────────────────┐
│      Insulation      │
├──────────────────────┤
│                      │
│   Protected System   │
│                      │
└──────────────────────┘

The insulation is integrated with the enclosure and internal thermal-management elements.

Its role is to reduce unwanted thermal exchange between the protected system and its environment.

10. Temperature-Sensor Integration

Temperature sensors are placed in relation to the thermal regions that require monitoring.

Thermal Region
      │
      ▼
Temperature Sensor
      │
      ▼
Electrical Connection
      │
      ▼
MCU / Controller
      │
      ▼
Thermal-Control Logic

The assembly therefore connects physical sensor location with the software thermal-control loop.

11. Power-Monitoring Integration

Voltage and current monitoring are part of the electrical subsystem represented in the assembly.

Battery / Electrical Source
           │
           ▼
     Electrical Path
           │
     ┌─────┴─────┐
     ▼           ▼
 Voltage       Current
 Sensor        Sensor
     │           │
     └─────┬─────┘
           ▼
          MCU
           │
           ▼
        Telemetry

This allows electrical behavior to be monitored alongside thermal behavior.

12. Controller Integration

The onboard controller forms the central connection between the physical sensors, thermal-control system, safety logic, and communication system.

                 ┌──────────────────┐
                 │       MCU        │
                 └────────┬─────────┘
                          │
       ┌──────────────────┼──────────────────┐
       │                  │                  │
       ▼                  ▼                  ▼
   Temperature        Voltage/Current       LoRa
     Sensors            Monitoring        Communication
       │                  │                  │
       ▼                  ▼                  ▼
   Thermal Control     Power Data          GCS
       │
       ▼
    MOSFET
       │
       ▼
    Heater

The CAD assembly provides the physical packaging context for this controller.

13. MOSFET Integration

The MOSFET switching stage connects the low-power control electronics to the heating element.

MCU Control Signal
        │
        ▼
┌──────────────────┐
│ MOSFET Switching │
│      Stage       │
└────────┬─────────┘
         │
         ▼
     Heater Load

The physical assembly maintains the MOSFET within the electrical and mechanical architecture of the system.

14. LoRa Module Integration

The LilyGO T3-S3 LoRa module is integrated into the electronics section.

MCU
 │
 ▼
LilyGO T3-S3
 │
 ▼
Antenna
 │
 ▼
Protective Structure
 │
 ▼
Communication Environment

The CAD assembly represents the module and antenna as part of the complete system rather than as an independent communication subsystem.

15. Antenna Integration

The antenna requires a suitable physical interface with the surrounding protection structure.

              Electronics
                   │
                   ▼
              LoRa Module
                   │
                   ▼
                Antenna
                   │
                   ▼
             Protection
                   │
                   ▼
              Environment

The physical design balances antenna placement with the enclosure and environmental-protection structure.

The CAD geometry alone does not establish a measured communication range or link-performance improvement.

16. Enclosure Integration

The enclosure forms the outer mechanical boundary of the integrated HIFLY system.

┌───────────────────────────────────────────┐
│                  ENCLOSURE                 │
│                                           │
│   ┌───────────────────────────────────┐   │
│   │           INTERNAL SYSTEM         │   │
│   │                                   │   │
│   │ Battery                          │   │
│   │ PHP                              │   │
│   │ Heater                           │   │
│   │ Sensors                          │   │
│   │ Controller                       │   │
│   │ MOSFET                           │   │
│   │ LoRa                             │   │
│   │                                   │   │
│   └───────────────────────────────────┘   │
│                                           │
└───────────────────────────────────────────┘

The enclosure geometry provides the physical boundary within which the thermal and electrical subsystems are integrated.

17. Flexible Protection Integration

Flexible silicone protection is incorporated at appropriate mechanical or thermal interfaces.

Component A
    │
    ▼
┌──────────────┐
│    Silicone  │
│   Protection │
└──────┬───────┘
       │
       ▼
Component B

The flexible element provides a physical interface between components where flexibility and thermal-cycle tolerance are part of the protection concept.

18. Lightweight Protective Layer Integration

The lightweight protective layer is represented as part of the outer protection architecture.

External Environment
          │
          ▼
┌──────────────────────┐
│ Lightweight Layer    │
├──────────────────────┤
│      Enclosure       │
├──────────────────────┤
│      Insulation      │
├──────────────────────┤
│ Protected Components │
└──────────────────────┘

The physical layer contributes to the environmental-protection architecture.

The CAD representation does not by itself quantify its radiation-protection performance.

19. Mechanical Interfaces

The assembly contains interfaces between:

battery and thermal structure
PHP and thermal structure
heater and thermal structure
sensors and monitored components
electronics and enclosure
LoRa module and antenna
antenna and protective structure

A conceptual interface map is:

Battery
  │
  ├────► Thermal Interface ◄──── PHP
  │
  ├────► Heater Interface
  │
  ├────► Sensor Interface
  │
  └────► Electrical Interface
               │
               ▼
             MCU
               │
       ┌───────┴────────┐
       ▼                ▼
    MOSFET             LoRa
       │                │
       ▼                ▼
    Heater           Antenna

The CAD assembly provides the geometry needed to represent these interfaces.

20. Assembly Packaging

The HIFLY system is packaged so that the thermal, electrical, and communication subsystems coexist within the same physical structure.

┌────────────────────────────────────────────────┐
│                  HIFLY PACKAGE                 │
│                                                │
│   ┌────────────────┐   ┌───────────────────┐  │
│   │    BATTERY     │   │ THERMAL SYSTEM    │  │
│   │                │   │                   │  │
│   │   Li-ion Pack  │   │ PHP + Heater      │  │
│   └────────────────┘   └───────────────────┘  │
│                                                │
│   ┌────────────────┐   ┌───────────────────┐  │
│   │    CONTROL     │   │   COMMUNICATION   │  │
│   │                │   │                   │  │
│   │ MCU + MOSFET   │   │ LilyGO + Antenna  │  │
│   └────────────────┘   └───────────────────┘  │
│                                                │
│            Protection / Insulation             │
│                                                │
└────────────────────────────────────────────────┘

The assembly layout provides the physical relationship between these functional groups.

21. Assembly and Thermal Path

The CAD assembly represents the physical thermal path from active heating and internal thermal regions to the surrounding structure.

                    Heater
                      │
                      ▼
              Thermal Interface
                      │
             ┌────────┴────────┐
             │                 │
             ▼                 ▼
          Battery             PHP
             │                 │
             └────────┬────────┘
                      ▼
               Thermal Structure
                      │
                      ▼
                  Insulation
                      │
                      ▼
                  Environment

The thermal path shown in the CAD architecture is a design representation rather than a measured heat-flow result.

22. Assembly and Electrical Path

The electrical path is represented separately from the thermal path.

Battery
  │
  ▼
Electrical Distribution
  │
  ├──────────────► Controller
  │
  ├──────────────► Sensors
  │
  ├──────────────► MOSFET
  │                    │
  │                    ▼
  │                  Heater
  │
  └──────────────► LoRa Module
                       │
                       ▼
                    Antenna

This allows the CAD assembly to represent the relationship between electrical power distribution and physical component placement.

23. Assembly and Data Path

The physical assembly supports the software data path.

Physical System
      │
      ▼
Sensors
      │
      ▼
MCU
      │
      ├──────────► Thermal Control
      │
      ├──────────► Power Monitoring
      │
      └──────────► LoRa
                        │
                        ▼
                       GCS

The CAD model therefore forms the physical foundation of the complete hardware/software system.

24. Assembly and Environmental Protection

The overall assembly integrates multiple environmental-protection concepts.

             HIGH-ALTITUDE ENVIRONMENT
                        │
        ┌───────────────┼────────────────┐
        │               │                │
        ▼               ▼                ▼
    Low Temperature   Low Pressure    Radiation
        │               │                │
        ▼               ▼                ▼
    Insulation       Protection      Protective
                      Enclosure        Layer
        │               │                │
        └───────────────┼────────────────┘
                        │
                        ▼
                  HIFLY Assembly

The assembly is therefore designed as an integrated environmental-protection package rather than a collection of unrelated components.

25. Assembly and Thermal Cycling

Thermal cycling affects interfaces between different materials and components.

The assembly includes flexible protection as part of the mechanical response to these interfaces.

Thermal Cycling
      │
      ▼
Repeated Expansion / Contraction
      │
      ▼
Component Interface
      │
      ▼
Flexible Silicone
      │
      ▼
Protected Mechanical Interface

The CAD assembly provides the spatial representation of this interface.

26. Assembly and PHP

The PHP occupies a dedicated position within the thermal-management region.

┌─────────────────────────────────┐
│         THERMAL REGION          │
│                                 │
│       ┌───────────────┐         │
│       │      PHP      │         │
│       └───────┬───────┘         │
│               │                 │
│               ▼                 │
│          Thermal Path            │
│               │                 │
│               ▼                 │
│            Battery              │
│                                 │
└─────────────────────────────────┘

The CAD assembly provides the physical relationship required for later thermal analysis and prototype construction.

27. Assembly and GCS Relationship

The physical assembly contains the hardware required to generate and transmit the data displayed by the GCS.

HIFLY Physical Assembly
        │
        ├── Temperature Sensors
        ├── Voltage Monitoring
        ├── Current Monitoring
        ├── Heater State
        ├── Safety State
        │
        ▼
       MCU
        │
        ▼
   LilyGO T3-S3
        │
        ▼
       LoRa
        │
        ▼
       GCS

The GCS therefore represents the system state produced by the physical assembly and onboard software.

28. CAD-to-Prototype Relationship

The assembly serves as the geometric reference for the physical prototype.

CAD Assembly
     │
     ▼
Mechanical Integration
     │
     ▼
Fabrication / Assembly
     │
     ▼
Physical Prototype
     │
     ├────────► Photographs
     ├────────► Video
     ├────────► Testing
     └────────► Experimental Data

This relationship provides traceability between the designed geometry and physical evidence.

29. CAD-to-Simulation Relationship

The assembly can also provide the geometry required for simulation.

CAD Assembly
     │
     ▼
Geometry Selection
     │
     ▼
Thermal Model
     │
     ▼
Simulation
     │
     ├── Temperature Distribution
     ├── Heat Flux
     └── Comparative Thermal Evaluation

The simulation results remain separate from the CAD geometry so that design inputs and analytical outputs are not conflated.

30. PHP CAD and Simulation Interpretation

The overall assembly may contain a PHP geometry for thermal analysis.

A conventional steady-state thermal simulation should be interpreted as an evaluation of the thermal architecture containing the PHP rather than as a direct simulation of the internal pulsating two-phase flow.

PHP CAD
  │
  ▼
Thermal Geometry
  │
  ▼
Steady-State Thermal Evaluation
  │
  ├── Temperature Distribution
  ├── Heat Flux
  └── Thermal Comparison

A dedicated multiphase transient model would be required to directly represent PHP internal flow behavior.

31. Assembly Evidence

The overall CAD assembly can provide evidence for:

physical integration
component placement
thermal-system arrangement
battery packaging
enclosure architecture
communication hardware placement
sensor placement
mechanical interfaces

The CAD model does not by itself establish:

measured thermal performance
measured communication range
validated battery endurance
measured system weight
measured power consumption
radiation attenuation
pressure performance

Those claims require corresponding experimental or analytical evidence.

32. Assembly Status

The overall assembly status is represented through the engineering evidence associated with the CAD and physical build.

Area	Evidence Type
Overall geometry	CAD
Battery arrangement	CAD
PHP geometry	CAD
Thermal-management integration	CAD
Enclosure	CAD
Component packaging	CAD
Physical implementation	Prototype evidence
Thermal behavior	Simulation / testing
Electrical behavior	Measurements
Communication behavior	Communication testing
System validation	Requirement-specific evidence

The status of each individual subsystem is maintained according to the actual evidence available for that subsystem.

33. Assembly Traceability

The overall assembly connects the major HIFLY engineering layers.

                 PROBLEM
                    │
                    ▼
              REQUIREMENTS
                    │
                    ▼
              ARCHITECTURE
                    │
                    ▼
             HARDWARE DESIGN
                    │
                    ▼
                CAD MODEL
                    │
                    ▼
             OVERALL ASSEMBLY
                    │
          ┌─────────┼─────────┐
          │         │         │
          ▼         ▼         ▼
      Simulation Prototype Testing
          │         │         │
          └─────────┼─────────┘
                    ▼
              SYSTEM EVIDENCE

This traceability is important for connecting the physical CAD design with the wider HIFLY project.

34. Complete Assembly Functional Relationship
                         HIFLY ASSEMBLY
                              │
                              ▼
                 ┌──────────────────────┐
                 │      ENCLOSURE       │
                 └──────────┬───────────┘
                            │
          ┌─────────────────┼─────────────────┐
          │                 │                 │
          ▼                 ▼                 ▼
       BATTERY          THERMAL            CONTROL
          │             SYSTEM                │
          │                 │                 │
          │            ┌────┼────┐            │
          │            │    │    │            │
          │            ▼    ▼    ▼            ▼
          │           PHP Heater Insulation  MCU
          │                                    │
          │                             ┌──────┼──────┐
          │                             │             │
          │                             ▼             ▼
          │                          MOSFET          LoRa
          │                             │             │
          │                             ▼             ▼
          │                          Heater        Antenna
          │
          └──────────────┬───────────────────────────┐
                         │                           │
                         ▼                           ▼
                  Temperature                  Voltage / Current
                     Sensors                      Monitoring
                         │                           │
                         └──────────────┬────────────┘
                                        ▼
                                  HIFLY Controller
                                        │
                                        ▼
                                      LoRa
                                        │
                                        ▼
                                       GCS
35. Final Assembly Representation

The HIFLY overall CAD assembly represents the complete physical relationship between the thermal, electrical, communication, sensing, and protection systems.

┌────────────────────────────────────────────────────────────────┐
│                        HIFLY SYSTEM                            │
│                                                                │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                    PROTECTION                             │  │
│  │                                                          │  │
│  │  Enclosure • Insulation • Flexible Protection            │  │
│  │  Lightweight Protective Layer                            │  │
│  │                                                          │  │
│  │   ┌────────────────────────────────────────────────────┐ │  │
│  │   │                  THERMAL SYSTEM                   │ │  │
│  │   │                                                    │ │  │
│  │   │       PHP ──────── Heater ──────── Insulation     │ │  │
│  │   │          │            │                           │ │  │
│  │   │          └────────────┼──────────────┐            │ │  │
│  │   │                       │              │            │ │  │
│  │   │                       ▼              ▼            │ │  │
│  │   │                    BATTERY      Temperature       │ │  │
│  │   │                                  Sensor           │ │  │
│  │   └───────────────────────┬────────────────────────────┘ │  │
│  │                           │                              │  │
│  │                           ▼                              │  │
│  │                    ┌──────────────┐                      │  │
│  │                    │ MCU / CONTROL│                      │  │
│  │                    └──────┬───────┘                      │  │
│  │                           │                              │  │
│  │                ┌──────────┼──────────┐                   │  │
│  │                ▼                     ▼                   │  │
│  │             MOSFET                  LoRa                 │  │
│  │                │                     │                   │  │
│  │                ▼                     ▼                   │  │
│  │             Heater                Antenna                │  │
│  │                                                          │  │
│  │         Voltage / Current Monitoring                     │  │
│  │                                                          │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                │
└────────────────────────────────────────────────────────────────┘

The overall HIFLY CAD assembly is the physical integration layer connecting the battery, thermal-management system, electronics, communication hardware, and environmental-protection structures into one coherent engineering design.
