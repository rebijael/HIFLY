# 05 — CAD

## 1. CAD Overview

The HIFLY CAD section represents the mechanical and thermal integration of the major physical subsystems.

The CAD architecture connects:

- overall system assembly
- battery enclosure and battery placement
- Pulsating Heat Pipe (PHP)
- thermal-management structure
- protection enclosure
- flexible thermal-protection elements
- antenna protection
- internal component arrangement

The CAD models provide the physical representation required to understand how the HIFLY thermal, electrical, and environmental-protection concepts are integrated into a single system.

```text
                         HIFLY CAD SYSTEM
                              │
             ┌────────────────┼────────────────┐
             │                │                │
             ▼                ▼                ▼
        Main Assembly      Battery          Thermal
                           Structure        Management
             │                │                │
             └────────────────┼────────────────┘
                              │
                              ▼
                       Protection System
                              │
                 ┌────────────┼────────────┐
                 │            │            │
                 ▼            ▼            ▼
              Enclosure    Silicone      Antenna
                           Protection     Protection
                              │
                              ▼
                        Complete CAD
                           Assembly
```
2. CAD Purpose

The CAD models are used to represent the physical implementation of the HIFLY design.

The CAD work provides a connection between the system architecture and the prototype hardware by showing:

component placement
enclosure relationships
thermal-management integration
battery arrangement
PHP placement
protection structures
mechanical interfaces
overall packaging

The CAD representation is therefore part of the engineering evidence for the proposed system.

3. CAD Architecture

The HIFLY CAD structure is divided into major mechanical and thermal subsystems.

05_CAD/
│
├── README.md
│
├── assembly/
│   └── README.md
│
├── battery/
│   └── README.md
│
├── php/
│   └── README.md
│
├── enclosure/
│   └── README.md
│
└── thermal_management/
    └── README.md

Each directory corresponds to a specific physical aspect of the HIFLY design.

4. Overall Assembly

The overall HIFLY assembly represents the relationship between the major physical subsystems.

                  ┌──────────────────────────┐
                  │      HIFLY ASSEMBLY      │
                  └─────────────┬────────────┘
                                │
         ┌──────────────────────┼──────────────────────┐
         │                      │                      │
         ▼                      ▼                      ▼
   Battery System         Thermal System        Protection System
         │                      │                      │
         │                      ├── PHP                ├── Enclosure
         │                      ├── Heater             ├── Insulation
         │                      └── Thermal Path       ├── Silicone
         │                                             └── Protective Layer
         │
         └──────────────────────┬──────────────────────┘
                                │
                                ▼
                       Electrical / Control
                                │
                                ├── Sensors
                                ├── Controller
                                ├── MOSFET
                                └── LoRa / Antenna

The assembly is intended to provide a common physical representation of the complete HIFLY architecture.

5. Mechanical Integration

Mechanical integration is important because the thermal, electrical, and protection systems occupy the same physical package.

The CAD design considers the relationship between:

Battery
   │
   ├── Thermal Protection
   │
   ├── Heating Element
   │
   ├── PHP
   │
   ├── Temperature Sensing
   │
   └── Electrical Connections
            │
            ▼
        Main Assembly
            │
            ├── Controller
            ├── MOSFET
            ├── Power Monitoring
            └── LoRa / Antenna

The CAD model therefore represents the physical arrangement required for the integrated HIFLY system rather than treating every subsystem as an isolated component.

6. Thermal Integration

The CAD architecture places the thermal-management elements around the components that require thermal protection or thermal control.

The major thermal elements are:

thermal insulation
Pulsating Heat Pipe
heating element
battery
temperature-sensing locations
thermal-contact structures
enclosure/protection structure
                  THERMAL INTEGRATION

                     Environment
                          │
                          ▼
                  ┌──────────────┐
                  │  Insulation  │
                  └──────┬───────┘
                         │
              ┌──────────┴──────────┐
              │                     │
              ▼                     ▼
             PHP                  Heater
              │                     │
              └──────────┬──────────┘
                         │
                         ▼
                      Battery
                         │
                         ▼
                 Temperature Sensor

The CAD model provides the physical geometry needed to understand this thermal arrangement.

7. Battery CAD

The battery subsystem is represented as a dedicated CAD component because the battery is both:

the primary stored-energy source
a major thermal-management target

The battery CAD representation connects the battery geometry with the surrounding thermal-management and protection structure.

                ┌─────────────────────┐
                │   BATTERY SYSTEM    │
                └──────────┬──────────┘
                           │
             ┌─────────────┼─────────────┐
             │             │             │
             ▼             ▼             ▼
        Insulation       Heater         PHP
             │             │             │
             └─────────────┼─────────────┘
                           │
                           ▼
                   Thermal Structure

The battery CAD section represents the physical packaging relationship rather than establishing battery performance values that are not available from the CAD model.

8. PHP CAD

The Pulsating Heat Pipe is represented as a dedicated thermal-management component.

The PHP CAD model provides the physical geometry used to represent its placement and integration with the thermal structure.

                 ┌──────────────────────┐
                 │   PHP GEOMETRY       │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Thermal Interface    │
                 └──────────┬───────────┘
                            │
                            ▼
                         Battery

The CAD representation establishes the physical form and placement of the PHP.

It does not by itself prove thermal performance or reproduce the internal pulsating two-phase flow of the PHP.

9. PHP Thermal Integration

The PHP is integrated with the HIFLY thermal architecture as a passive-assisted heat-transfer element.

                 Heat Source / Thermal Region
                              │
                              ▼
                       ┌─────────────┐
                       │     PHP     │
                       └──────┬──────┘
                              │
                              ▼
                    Thermal Distribution
                              │
                              ▼
                         Thermal Body

The physical CAD model is used to represent:

PHP placement
PHP geometry
interface with the surrounding structure
relationship with the protected component

The actual thermal performance of the PHP is evaluated separately through simulation and experimental evidence.

10. Enclosure CAD

The enclosure provides the mechanical boundary for the protected HIFLY hardware.

The enclosure is associated with the environmental-protection concept and supports the physical arrangement of the internal components.

             ┌────────────────────────────┐
             │          ENCLOSURE         │
             │                            │
             │   ┌────────────────────┐   │
             │   │ Internal Hardware  │   │
             │   │                    │   │
             │   │ Battery            │   │
             │   │ Thermal System     │   │
             │   │ Controller         │   │
             │   │ Sensors             │   │
             │   └────────────────────┘   │
             │                            │
             └────────────────────────────┘

The enclosure geometry provides the physical packaging context for the internal subsystems.

11. Protection Structure

The physical protection architecture combines several mechanisms.

             HIGH-ALTITUDE ENVIRONMENT
                       │
                       ▼
                ┌─────────────┐
                │  ENCLOSURE  │
                └──────┬──────┘
                       │
                       ▼
                ┌─────────────┐
                │ INSULATION  │
                └──────┬──────┘
                       │
                       ▼
              Internal Components
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
       Battery        PHP        Electronics

The physical design is intended to support the broader HIFLY protection architecture covering low-temperature conditions, low-pressure operation, thermal cycling, electrical protection, and environmental exposure.

12. Flexible Silicone Protection

Flexible silicone is included as part of the physical protection concept for regions exposed to thermal cycling and mechanical interfaces.

Its role within the CAD architecture is represented as:

Mechanical / Thermal Interface
             │
             ▼
      Flexible Silicone
             │
      ┌──────┴──────┐
      │             │
      ▼             ▼
 Thermal Isolation  Mechanical Flexibility

The CAD model provides the physical placement and geometry of the flexible protection element.

13. Lightweight Protective Layer

The HIFLY design includes a lightweight protective layer as part of the environmental-protection concept.

The CAD architecture represents this layer as a physical element surrounding or supporting the protected subsystem where applicable.

          External Environment
                  │
                  ▼
       Lightweight Protective Layer
                  │
                  ▼
             Enclosure
                  │
                  ▼
             Insulation
                  │
                  ▼
          Protected Electronics

The CAD model establishes the physical implementation of the layer.

The CAD geometry alone does not establish a quantified radiation attenuation value.

14. Antenna Protection

The antenna and communication hardware require physical protection while maintaining the intended communication path.

The CAD relationship is:

              HIFLY Electronics
                     │
                     ▼
               LoRa Module
                     │
                     ▼
                  Antenna
                     │
                     ▼
             Protective Structure
                     │
                     ▼
                Environment

The protection structure is represented physically in the CAD architecture without treating the protective layer as proof of a specific communication-performance value.

15. Internal Component Arrangement

The physical layout of HIFLY brings together thermal, electrical, and communication components.

A conceptual arrangement is:

┌────────────────────────────────────────────────┐
│                 HIFLY ENCLOSURE                │
│                                                │
│  ┌──────────────────┐                          │
│  │     BATTERY      │                          │
│  │                  │                          │
│  │  Thermal System  │                          │
│  └────────┬─────────┘                          │
│           │                                    │
│      ┌────▼─────┐                              │
│      │   PHP    │                              │
│      └──────────┘                              │
│                                                │
│  ┌──────────────────┐  ┌────────────────────┐  │
│  │   Controller     │  │   Power / MOSFET   │  │
│  │      MCU         │  │      Section       │  │
│  └──────────────────┘  └────────────────────┘  │
│                                                │
│  ┌──────────────────────────────────────────┐  │
│  │          LoRa / Antenna Section          │  │
│  └──────────────────────────────────────────┘  │
│                                                │
└────────────────────────────────────────────────┘

The exact final physical placement is represented by the corresponding CAD assembly.

16. Sensor Placement

Temperature sensors are positioned as part of the thermal-monitoring architecture.

The physical relationship is:

Thermal Region
      │
      ▼
Temperature Sensor
      │
      ▼
Electrical Connection
      │
      ▼
HIFLY Controller

The CAD model provides the spatial context for the sensor relative to the battery and thermal-management components.

17. Heater Placement

The heating element is positioned so that its thermal input can contribute to the intended thermal-management region.

                  Heater
                    │
                    ▼
             Thermal Interface
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
       Battery               PHP
          │                   │
          └─────────┬─────────┘
                    ▼
             Thermal Structure

The heater placement is therefore part of both the mechanical and thermal CAD design.

18. CAD and Thermal Management

The CAD model provides the geometry used by the thermal-management design.

                  CAD Geometry
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
       Battery        PHP         Heater
          │            │            │
          └────────────┼────────────┘
                       │
                       ▼
                 Thermal Model
                       │
                       ▼
                  Simulation

The geometry can therefore support later thermal analysis without treating the CAD model itself as a simulation result.

19. CAD and Simulation

The CAD geometry forms the physical basis for the thermal simulation workflow.

The relationship is:

CAD Model
   │
   ▼
Geometry Preparation
   │
   ▼
Thermal Simulation Model
   │
   ▼
Thermal Evaluation
   │
   ├── Temperature Distribution
   ├── Heat Flux
   └── Thermal Comparison

The simulation results are maintained separately from the CAD files so that geometry and analysis evidence remain distinguishable.

20. PHP Simulation Relationship

A CAD model containing a PHP-assisted thermal architecture can be used for thermal evaluation.

However, an ordinary steady-state thermal FEA model does not directly simulate the internal pulsating two-phase flow of a real PHP.

The appropriate interpretation is:

PHP CAD Geometry
       │
       ▼
Thermal Evaluation
       │
       ▼
PHP-Assisted Thermal Architecture
       │
       ├── Temperature Distribution
       ├── Heat Flux
       └── Comparative Thermal Behavior

The simulation should therefore be described as a thermal evaluation of the PHP-assisted architecture rather than as a direct CFD reproduction of PHP pulsating flow unless a dedicated multiphase model is actually used.

21. CAD Evidence

The HIFLY CAD section provides physical evidence for the proposed mechanical architecture.

The evidence chain is:

System Concept
     │
     ▼
Mechanical Architecture
     │
     ▼
CAD Geometry
     │
     ▼
Assembly
     │
     ▼
Prototype / Fabrication
     │
     ▼
Physical Evidence

The CAD files therefore connect the conceptual solution with its physical implementation.

22. CAD and Prototype Relationship

The CAD model represents the intended physical structure, while photographs and physical prototype evidence represent the fabricated or assembled system.

                  DESIGN
                    │
                    ▼
                   CAD
                    │
                    ▼
              Physical Build
                    │
                    ▼
                Prototype
                    │
                    ▼
              Photos / Video

This separation allows the repository to distinguish between designed geometry and physically demonstrated hardware.

23. CAD Documentation Structure

The HIFLY CAD documentation is divided into five main areas:

CAD Section	Main Content
assembly/	Overall system assembly and component integration
battery/	Battery geometry and packaging
php/	Pulsating Heat Pipe geometry and integration
enclosure/	Protection and enclosure geometry
thermal_management/	Thermal-management structure and interfaces

Each section provides the physical context needed to understand its role within the complete HIFLY system.

24. Assembly Relationship

The CAD assembly connects the individual subsystem models.

                ┌──────────────┐
                │   Battery    │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │     PHP      │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │    Heater    │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │ Thermal Mgmt │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │  Enclosure   │
                └──────┬───────┘
                       │
                       ▼
                Complete Assembly

The assembly provides the common reference for the mechanical relationship between the subsystems.

25. Thermal-Management CAD Relationship

The thermal-management CAD section connects the physical components involved in heat generation, heat transfer, and thermal protection.

Heat Source
    │
    ▼
┌───────────────┐
│   Heater      │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│ Thermal Body  │
└───────┬───────┘
        │
   ┌────┴─────┐
   │          │
   ▼          ▼
Battery      PHP
   │          │
   └────┬─────┘
        │
        ▼
   Heat Transfer
        │
        ▼
     Structure

The CAD geometry supports the analysis and physical implementation of this thermal arrangement.

26. CAD and Electrical Integration

Although CAD primarily represents physical geometry, the assembly also provides the mechanical context for electrical components.

Electrical Components
       │
       ├── Battery
       ├── Controller
       ├── MOSFET
       ├── Sensors
       └── LoRa Module
                │
                ▼
        Physical Packaging
                │
                ▼
             CAD Model

This ensures that the electrical architecture is represented within the same physical system rather than existing only as a schematic.

27. CAD and Communication System

The LoRa communication hardware is physically integrated into the HIFLY assembly.

        HIFLY Controller
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
           Environment

The CAD model provides the physical placement and protection context for the communication subsystem.

28. CAD and Environmental Protection

The physical CAD architecture supports multiple environmental-protection concepts.

              HIGH-ALTITUDE ENVIRONMENT
                         │
          ┌──────────────┼──────────────┐
          │              │              │
          ▼              ▼              ▼
       Low Temp       Low Pressure    Radiation
          │              │              │
          ▼              ▼              ▼
      Insulation      Enclosure     Protective Layer
          │              │              │
          └──────────────┼──────────────┘
                         │
                         ▼
                   Protected System

The CAD model provides the physical geometry for these protective elements.

29. CAD and Thermal Cycling

Thermal cycling can create repeated expansion and contraction across mechanical and electrical interfaces.

The HIFLY CAD architecture includes flexible silicone protection as part of the physical response to this type of mechanical and thermal interface.

Thermal Cycling
      │
      ▼
Expansion / Contraction
      │
      ▼
Mechanical Interface
      │
      ▼
Flexible Silicone
      │
      ▼
Protected Interface

The CAD model represents the placement and geometry of the flexible protection element.

30. CAD Traceability

The CAD repository is connected to the rest of HIFLY through a clear engineering chain.

Problem Statement
       │
       ▼
System Requirements
       │
       ▼
Solution Architecture
       │
       ▼
Hardware Architecture
       │
       ▼
CAD
       │
       ├────────────► Simulation
       │
       └────────────► Prototype
                            │
                            ▼
                         Testing

This structure allows the physical CAD design to be evaluated against the system-level problem and requirements.

31. CAD Design Evidence

The HIFLY CAD evidence can represent:

complete system geometry
subsystem geometry
battery integration
PHP geometry
thermal-management structure
enclosure geometry
component placement
mechanical interfaces
antenna protection
physical integration

CAD geometry should be interpreted together with simulation and physical testing evidence when evaluating actual performance.

32. CAD Status Representation

The repository uses the following status concepts for engineering artifacts:

Status	Meaning
Concept	Design concept established
Design	Geometry or architecture developed
Simulated	Geometry used in analysis
Prototype	Physical implementation exists
Tested	Physical or software test performed
Validated	Evidence supports the corresponding requirement
Planned	Future implementation or analysis

A CAD model is not automatically considered experimentally validated merely because the geometry is complete.

33. CAD Evidence Chain
                   CAD MODEL
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
       Geometry      Assembly      Interfaces
          │            │            │
          └────────────┼────────────┘
                       │
                       ▼
                 Physical Build
                       │
              ┌────────┴────────┐
              │                 │
              ▼                 ▼
          Prototype           Testing
              │                 │
              └────────┬────────┘
                       ▼
                  Engineering
                    Evidence

This approach keeps the repository focused on traceable engineering evidence.

34. Overall HIFLY CAD System
                         HIFLY CAD
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
        ▼                   ▼                   ▼
     ASSEMBLY             BATTERY              PHP
        │                   │                   │
        │                   │                   │
        └───────────────────┼───────────────────┘
                            │
                            ▼
                  THERMAL MANAGEMENT
                            │
                 ┌──────────┼──────────┐
                 │          │          │
                 ▼          ▼          ▼
              Heater       PHP      Insulation
                 │          │          │
                 └──────────┼──────────┘
                            │
                            ▼
                       ENCLOSURE
                            │
                 ┌──────────┼──────────┐
                 │          │          │
                 ▼          ▼          ▼
              Battery    Electronics  Antenna
                            │
                            ▼
                       PROTECTION
                            │
                            ▼
                      COMPLETE SYSTEM
35. Final CAD Role in HIFLY

The CAD system provides the physical representation of HIFLY's integrated architecture.

It connects:

the battery thermal-management system
the Pulsating Heat Pipe
the active heating system
insulation
protection structures
flexible silicone protection
electronics packaging
LoRa communication hardware
antenna protection
the overall mechanical assembly

The CAD models form the geometric foundation for physical integration, thermal simulation, fabrication, and prototype evidence while remaining distinct from measured performance results.
