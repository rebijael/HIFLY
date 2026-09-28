# HIFLY System Architecture

## 1. Architecture Overview

HIFLY is organized as a modular system in which environmental mitigation mechanisms are integrated around critical UAV electrical, electronic, thermal and energy subsystems.

The architecture combines:

- Battery thermal management
- Processor/electronics cooling
- Electrical protection
- Thermal-cycle protection
- Environmental protection
- Communication protection
- Energy and power monitoring
- Ground Control Station monitoring
- Autonomous and fail-safe control

---

## 2. High-Level Architecture

```text
                           HIFLY
                             │
              ┌──────────────┴──────────────┐
              │                             │
        ONBOARD SYSTEM                 GROUND SYSTEM
              │                             │
      ┌───────┼────────┐                    │
      │       │        │                    │
   THERMAL   POWER  PROTECTION              │
    SYSTEM   SYSTEM    SYSTEM               │
      │       │        │                    │
   ┌──┴──┐    │    ┌───┼────────┐           │
   │     │    │    │   │        │           │
  BTMS   PHP   │ Chamber Silicone Protective │
              │          │       Layer       │
              │          │                    │
        Voltage/Current   │              GCS Dashboard
          Monitoring     │                    │
              │          │                    │
              └──────────┼────────────────────┘
                         │
                  LoRa Communication
                         │
                         ↓
                        GCS
```

---

## 3. Onboard Controller

The onboard controller forms the central control and monitoring unit.

The selected controller platform is the LilyGO T3-S3 LoRa.

### Controller Responsibilities

- Sensor data acquisition
- Battery temperature monitoring
- Voltage monitoring
- Current monitoring
- Heater control
- Thermal-control logic
- LoRa communication
- System-status monitoring
- Autonomous operation
- Fail-safe operation

---

## 4. Sensor Layer

The sensing layer provides the measurements required for system monitoring and control.

### Monitored Parameters

- Battery temperature
- Ambient temperature, where implemented
- Battery voltage
- Battery current
- System status

The measured values are processed by the onboard controller and can be transmitted to the Ground Control Station.

---

## 5. Battery Thermal Management System

The battery thermal-management subsystem combines sensing, heating, insulation and electrical control.

```text
                    BATTERY SYSTEM
                          │
              ┌───────────┼───────────┐
              │           │           │
        Temperature    Voltage      Current
          Sensor       Monitor       Monitor
              │           │           │
              └───────────┼───────────┘
                          │
                   Onboard Controller
                          │
                    Thermal Logic
                          │
                       MOSFET
                          │
                    Heating Element
                          │
                     Battery Pack
```

### Main Components

- Li-ion battery pack
- Temperature sensor
- Voltage monitoring
- Current monitoring
- Thermal insulation
- Heating element
- MOSFET driver
- Onboard controller

The controller regulates heater operation according to the implemented temperature-based thermal-control logic.

---

## 6. Pulsating Heat Pipe Cooling

The processor/electronics cooling subsystem uses the proposed Pulsating Heat Pipe architecture.

```text
Processor / Electronics
          │
          ↓
    PHP Heat Input
          │
          ↓
   Heat Transfer Region
          │
          ↓
    PHP Condenser
          │
          ↓
 Heat-Rejection Region
```

The PHP is intended to provide passive heat transfer away from the processor/electronics region.

### Development Approach

The PHP subsystem can be evaluated through:

- CAD development
- Thermal simulation
- Thermal distribution analysis
- Experimental testing, where available

Conventional thermal FEA can evaluate the thermal performance of the PHP-assisted architecture, but it does not by itself reproduce the actual internal pulsating two-phase flow of a PHP.

---

## 7. Electrical Protection

HIFLY proposes a low-pressure protection chamber around sensitive electrical/electronic components.

```text
        External Environment
                │
                ↓
       Protection Boundary
                │
                ↓
       Sensitive Electronics
                │
                ↓
       Insulation / Separation
```

### Protection Considerations

- Electrical insulation
- Physical separation
- Creepage and clearance
- Controlled enclosure
- Reduced exposure of sensitive components to the external environment

The final effectiveness of the protection architecture requires controlled validation.

---

## 8. Thermal Cycling Protection

Flexible silicone-based protection is considered around selected electronic areas.

```text
Electronic Component
        │
        ↓
Flexible Silicone Layer
        │
        ↓
Mechanical / Environmental Protection
```

The flexible protection is intended to accommodate expansion and contraction associated with temperature changes while providing an additional protective layer.

Validation requires repeated heating and cooling tests.

---

## 9. Environmental Protection

A lightweight protective layer is proposed around sensitive electronic areas.

The design objective is to provide environmental protection while considering:

- System weight
- Available installation space
- Component accessibility
- Integration requirements
- Maintainability

The final material and configuration require appropriate simulation and/or experimental validation.

---

## 10. Communication Subsystem

HIFLY uses LoRa communication between the onboard system and the Ground Control Station.

```text
Sensors
   │
   ↓
LilyGO T3-S3
   │
   ↓
LoRa Communication
   │
   ↓
Ground Control Station
```

### Antenna Protection

The antenna protection concept includes:

- Hydrophobic surface protection
- RF-transparent radome
- Protection against water/ice accumulation

Communication performance must be verified through actual link testing.

---

## 11. Ground Control Station

The Ground Control Station provides an operator-facing interface for monitoring and control.

### Dashboard Parameters

- Battery temperature
- Ambient temperature
- Voltage
- Current
- Heater status
- Thermal status
- LoRa link status
- Operating mode
- Safety status
- System alerts

### Planned Alerts

- Low temperature
- Over-temperature
- Sensor fault
- Communication lost

### Operator Controls

Where implemented, the GCS may provide:

- AUTO mode
- PRE-HEAT
- HEATER OFF
- Manual override

---

## 12. Autonomous and Fail-Safe Operation

Essential thermal-control functions are designed to operate onboard so that continuous GCS connectivity is not required for basic thermal control.

### Normal Operation

```text
Sensors
   ↓
Onboard Controller
   ↓
Thermal Control Logic
   ↓
Heater Control
   ↓
Battery Thermal Management
   ↓
LoRa
   ↓
Ground Control Station
```

### Communication Loss

```text
Communication Lost
        ↓
Onboard Controller Continues
        ↓
Autonomous Thermal Control
        ↓
Implemented Safety Logic
        ↓
Communication Re-established
        ↓
GCS Monitoring Resumes
```

The exact fail-safe thresholds and behaviours will be documented with the implemented firmware.

---

## 13. Energy and Power Monitoring

HIFLY monitors electrical parameters associated with battery operation and system power demand.

### Monitored Parameters

- Battery voltage
- Battery current
- Battery temperature
- Heater status
- Thermal-management demand

These measurements provide the data required to evaluate electrical consumption and support mission-level energy analysis.

---

## 14. Data Flow

```text
                 SENSOR LAYER
                      │
                      ↓
               DATA ACQUISITION
                      │
                      ↓
              ONBOARD CONTROLLER
                      │
          ┌───────────┼───────────┐
          │           │           │
          ↓           ↓           ↓
      THERMAL       POWER      SYSTEM
       CONTROL    MONITORING    STATUS
          │           │           │
          └───────────┼───────────┘
                      │
                      ↓
              LoRa Communication
                      │
                      ↓
                     GCS
```

---

## 15. Seven-Mechanism Integration

| No. | Environmental Challenge | HIFLY Mechanism |
|---|---|---|
| 1 | Reduced Cooling Efficiency | Pulsating Heat Pipe |
| 2 | Insulation Breakdown & Electrical Arcing | Low-Pressure Protection Chamber |
| 3 | Battery Degradation | Smart Battery Thermal Management System |
| 4 | Thermal Cycling Damage | Flexible Silicone Protection |
| 5 | Increased Radiation Exposure | Lightweight Protective Layer |
| 6 | Communication System Effects | Hydrophobic Antenna Protection + LoRa |
| 7 | Mission Energy & Endurance | Power & Energy Monitoring |

---

## 16. Modular Architecture

Each mitigation mechanism is treated as an independent subsystem.

```text
                  HIFLY
                    │
       ┌────────────┼────────────┐
       │            │            │
    THERMAL     ELECTRICAL   COMMUNICATION
       │         PROTECTION        │
   ┌───┴───┐         │             │
   │       │      Protection       LoRa
  BTMS    PHP      Chamber          │
                               Antenna/Radome
       │
       └──────────────┐
                      │
              Energy Monitoring
                      │
                      ↓
                     GCS
```

This modular approach allows individual subsystems to be developed and validated before complete system integration.

---

## 17. Development Sequence

HIFLY follows a progressive development approach.

```text
Problem Identification
        ↓
System Requirements
        ↓
Subsystem Design
        ↓
CAD / Simulation
        ↓
Prototype Development
        ↓
Individual Testing
        ↓
Subsystem Validation
        ↓
System Integration
        ↓
UAV Integration
        ↓
Environmental Validation
```

---

## 18. Architecture Status

The HIFLY architecture is under active development.

Individual modules may be at different development stages:

| Module | Current Development Stage |
|---|---|
| Smart Battery Thermal Management | Prototype |
| Battery Monitoring | Prototype |
| Pulsating Heat Pipe | CAD / Simulation |
| Low-Pressure Protection Chamber | Design |
| Thermal Cycling Protection | Design |
| Radiation Protection | Concept / Design |
| Antenna Protection | Design / Prototype |
| LoRa Communication | Prototype |
| Ground Control Station | Development |
| Full UAV Integration | Planned |

Status will be updated as additional design, prototype, simulation and testing evidence becomes available.

---

## 19. Architecture Principle

> **Seven mitigation mechanisms → One integrated high-altitude reliability system**

HIFLY is designed as a modular reliability architecture rather than a single-purpose subsystem.

The individual mechanisms can be developed and validated independently and subsequently integrated into the complete UAV system.

---

## 20. Validation Principle

Every major subsystem will be associated with appropriate evidence.

| Subsystem | Evidence |
|---|---|
| Battery Thermal Management | Temperature and electrical measurements |
| PHP Cooling | CAD, thermal simulation and/or experimental testing |
| Electrical Protection | Low-pressure and insulation evaluation |
| Thermal Cycling Protection | Repeated thermal-cycle testing |
| Environmental Protection | Material/design evaluation |
| Communication Protection | LoRa communication testing |
| Energy Management | Voltage/current measurements |
| GCS | Interface and communication testing |
| Fail-Safe System | Communication-loss testing |

Numerical performance claims will only be added when supported by documented measurements or simulation results.
