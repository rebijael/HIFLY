# HIFLY — Hardware Overview

## 1. Hardware Architecture

HIFLY combines sensing, thermal management, power monitoring, control, communication and physical protection hardware into a modular onboard architecture.

The main hardware elements are:

- Li-ion battery pack
- Temperature sensor
- Voltage sensor/monitoring circuit
- Current sensor/monitoring circuit
- Heating element
- MOSFET driver
- Thermal insulation
- LilyGO T3-S3 LoRa controller
- Pulsating Heat Pipe
- Low-pressure protection chamber
- Flexible silicone protection
- Lightweight protective layer
- Antenna/radome protection

---

## 2. Hardware Block Diagram

```text
                         HIFLY HARDWARE
                              │
             ┌────────────────┼────────────────┐
             │                │                │
          BATTERY          CONTROLLER       PROTECTION
             │                │                │
        ┌────┼────┐           │          ┌─────┼─────┐
        │    │    │           │          │     │     │
      Temp  Volt Current   LilyGO T3-S3  Chamber Silicone
     Sensor Monitor Monitor      │       Layer   Layer
        │    │    │              │
        └────┼────┘              │
             │                   │
             └──────────┬────────┘
                        │
                  Thermal Logic
                        │
                      MOSFET
                        │
                  Heating Element
                        
             PHP → Processor Cooling
             
             Radome → Antenna Protection
             
             LoRa → GCS Communication
```

---

## 3. Li-ion Battery Pack

The Li-ion battery pack serves as the primary electrical energy source for the onboard system.

### Role

The battery provides power for:

- Onboard controller
- Sensors
- Heating system
- Communication hardware
- Other connected electrical loads

### Monitoring

The battery subsystem is monitored using:

- Temperature
- Voltage
- Current

Battery photographs and physical documentation are maintained separately in the project media directory.

---

## 4. Temperature Sensing

Temperature sensing is used as a key input for the thermal-management system.

### Primary Function

The temperature sensor provides battery temperature information to the onboard controller.

### Application

The measured temperature is used by the thermal-control logic to determine the required heater state.

```text
Battery
   ↓
Temperature Sensor
   ↓
Onboard Controller
   ↓
Thermal Control Logic
   ↓
Heater Control
```

The exact sensor model and measurement characteristics should be documented once the final hardware configuration is confirmed.

---

## 5. Voltage Monitoring

Battery voltage monitoring provides information about the electrical state of the power source.

### Purpose

Voltage data can be used for:

- Battery-state monitoring
- Power analysis
- Energy analysis
- System-status monitoring

The exact sensing circuit and component specification should be documented according to the implemented hardware.

---

## 6. Current Monitoring

Current monitoring measures electrical current drawn by the system.

### Purpose

Current measurements support:

- Power calculation
- Heater power analysis
- Energy-consumption analysis
- Mission-level energy assessment

Electrical power can be calculated from measured voltage and current:

```text
Power = Voltage × Current
```

Measured values should be stored as raw data before processing and graph generation.

---

## 7. Heating Element

The heating element provides controlled thermal energy to the battery system.

### Function

The heater is used to support battery temperature management during low-temperature operation.

### Control

```text
Temperature Sensor
        ↓
Controller
        ↓
Thermal Control Logic
        ↓
MOSFET Driver
        ↓
Heating Element
        ↓
Battery
```

The heater should operate according to the implemented thermal-control logic rather than remaining continuously active.

---

## 8. MOSFET Driver

The MOSFET switching stage provides electrical control of the heating element.

### Function

The controller produces the required control signal, while the MOSFET switching stage handles the heater's electrical load.

```text
LilyGO T3-S3
      │
      ↓
Control Signal
      │
      ↓
MOSFET
      │
      ↓
Heating Element
```

The final MOSFET and driver specifications should be documented according to the actual hardware used in the prototype.

---

## 9. Thermal Insulation

Thermal insulation is used around the battery thermal-management assembly to reduce unwanted heat transfer to the surrounding environment.

### Intended Functions

- Reduce heat loss
- Support battery thermal management
- Improve heater effectiveness
- Reduce unnecessary thermal energy demand

The actual insulation material, thickness and measured thermal performance should be added once confirmed through the prototype documentation.

---

## 10. LilyGO T3-S3 LoRa

The LilyGO T3-S3 LoRa acts as the onboard controller and communication platform.

### Main Functions

- Sensor acquisition
- Thermal-control logic
- Heater control
- Voltage/current monitoring
- System-status monitoring
- LoRa communication
- Autonomous operation
- Fail-safe functions

### Communication

```text
Onboard Sensors
       ↓
LilyGO T3-S3
       ↓
LoRa
       ↓
Ground Control Station
```

The exact firmware implementation is documented in the software section of the repository.

---

## 11. Pulsating Heat Pipe

The Pulsating Heat Pipe is a passive cooling mechanism proposed for processor/electronics thermal management.

### Main Function

The PHP transfers heat from the processor/electronics region toward a heat-rejection region.

```text
Processor
   ↓
Heat Input
   ↓
PHP
   ↓
Heat Transport
   ↓
Condenser / Heat-Rejection Region
```

The PHP hardware/design is documented separately in:

```text
05_CAD/php/
```

and its simulation work is maintained in:

```text
06_Simulation/
```

---

## 12. Low-Pressure Protection Chamber

The protection chamber is intended to protect sensitive electrical/electronic components under reduced-pressure operating conditions.

### Design Considerations

- Electrical insulation
- Physical separation
- Creepage and clearance
- Component accessibility
- Enclosure geometry
- Weight and space constraints

The chamber is currently treated as a design/prototype-development element until appropriate validation is completed.

---

## 13. Flexible Silicone Protection

Flexible silicone protection is considered for selected electronic assemblies exposed to repeated thermal changes.

### Intended Functions

- Provide an additional protective layer
- Accommodate thermal expansion and contraction
- Reduce direct environmental exposure
- Support thermal-cycle durability

The material specification and final application method should be documented after confirmation through the actual prototype.

---

## 14. Lightweight Protective Layer

A lightweight protective layer is proposed around selected sensitive electronic areas.

### Design Considerations

- Protection effectiveness
- Weight
- Available space
- Thermal behaviour
- Component accessibility
- Integration with the UAV structure

The final material and performance should be supported by appropriate testing or simulation before performance claims are made.

---

## 15. Antenna and Radome Protection

The communication subsystem uses an antenna protection concept involving:

- Hydrophobic surface protection
- RF-transparent radome
- Protection against water/ice accumulation

The purpose is to reduce environmental effects on the exposed antenna while maintaining communication performance.

Actual communication performance must be established through testing.

---

## 16. Ground Control Station Hardware Interface

The onboard hardware provides data to the Ground Control Station through the LoRa communication link.

The GCS can be used to monitor:

- Battery temperature
- Ambient temperature
- Voltage
- Current
- Heater status
- Thermal status
- Communication status
- Safety status

Where implemented, operator controls may include:

- AUTO
- PRE-HEAT
- HEATER OFF
- Manual override

---

## 17. Hardware-to-Software Interaction

```text
              HARDWARE
                 │
     ┌───────────┼───────────┐
     │           │           │
   Sensors     Battery     Heater
     │           │           │
     └───────────┼───────────┘
                 │
                 ↓
          LilyGO T3-S3
                 │
        ┌────────┼────────┐
        │        │        │
     Thermal    Power    System
      Logic    Monitor   Status
        │        │        │
        └────────┼────────┘
                 │
                 ↓
                LoRa
                 │
                 ↓
                GCS
```

---

## 18. Hardware Development Status

| Hardware | Current Status |
|---|---|
| Li-ion Battery Pack | Prototype |
| Temperature Monitoring | Prototype |
| Voltage Monitoring | Prototype |
| Current Monitoring | Prototype |
| Heating Element | Prototype |
| MOSFET Heater Control | Prototype / Development |
| Thermal Insulation | Prototype |
| LilyGO T3-S3 LoRa | Prototype |
| Pulsating Heat Pipe | CAD / Simulation |
| Low-Pressure Protection Chamber | Design |
| Flexible Silicone Protection | Design |
| Lightweight Protective Layer | Concept / Design |
| Antenna/Radome Protection | Design / Prototype |
| GCS Hardware Interface | Development |

> Status should be updated whenever a subsystem moves from design to prototype, testing or validation.

---

## 19. Hardware Documentation

Detailed hardware evidence should be stored in the following repository locations:

```text
03_Hardware/
├── hardware_overview.md
├── bill_of_materials.md
├── wiring/
├── pcb/
└── datasheets/
```

CAD files are maintained separately:

```text
05_CAD/
```

Prototype photographs are maintained in:

```text
10_Media/
```

Experimental measurements are maintained in:

```text
07_Testing/
08_Data/
```

---

## 20. Hardware Evidence Principle

Every hardware component should eventually be supported by appropriate documentation such as:

- Component photograph
- Part number
- Datasheet
- Electrical specification
- CAD model, where applicable
- Wiring information
- Prototype evidence
- Test results, where applicable

Only the hardware actually used or confirmed for the HIFLY prototype should be marked as implemented.
