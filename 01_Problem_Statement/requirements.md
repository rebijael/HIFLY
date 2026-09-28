# HIFLY System Requirements

## 1. Purpose

This document defines the initial engineering requirements for the HIFLY high-altitude reliability system.

The requirements are intended to guide subsystem design, integration and validation.

---

## 2. Functional Requirements

### FR-01 — Temperature Monitoring

The system shall continuously monitor battery temperature using an onboard temperature sensor.

### FR-02 — Electrical Monitoring

The system shall monitor battery voltage and current.

### FR-03 — Controlled Battery Heating

The system shall control a battery heating element based on measured temperature and the implemented thermal-control logic.

### FR-04 — Heater Switching

The heating element shall be controlled using a suitable switching stage such as a MOSFET driver.

### FR-05 — Processor Cooling

The system shall provide a passive thermal-management mechanism for processor/electronics cooling using the proposed Pulsating Heat Pipe architecture.

### FR-06 — Environmental Protection

The system shall incorporate protection mechanisms for sensitive electrical and electronic components against the identified high-altitude environmental challenges.

### FR-07 — Communication

The onboard system shall support LoRa-based communication between the UAV and the Ground Control Station.

### FR-08 — Ground Monitoring

The GCS shall provide visibility of relevant system parameters including:

- Battery temperature
- Voltage
- Current
- Heater status
- Thermal status
- Communication status
- Operating mode
- Safety status

### FR-09 — Fail-Safe Operation

The onboard controller shall be capable of continuing essential thermal-control operation when communication with the Ground Control Station is unavailable.

### FR-10 — Energy Monitoring

The system shall monitor electrical parameters required to evaluate battery energy usage and thermal-management demand.

---

## 3. Environmental Requirements

The system architecture shall consider operation under:

- Sub-zero temperature conditions
- Reduced atmospheric pressure
- Thermal cycling
- High-altitude environmental exposure
- Communication hardware exposure to moisture/ice
- Increased radiation exposure

Environmental performance shall be validated through appropriate controlled testing or simulation.

---

## 4. Integration Requirements

### IR-01 — Modular Architecture

The seven mitigation mechanisms shall be developed as modular subsystems so that individual mechanisms can be tested independently.

### IR-02 — Compact Integration

Subsystems shall be designed with UAV weight and available installation space in mind.

### IR-03 — Electrical Integration

Sensors, heater, controller, communication hardware and protection modules shall operate as an integrated electrical system.

### IR-04 — Software Integration

Sensor acquisition, thermal control, power monitoring, communication and fail-safe functions shall operate through the onboard controller.

### IR-05 — GCS Integration

Relevant onboard measurements and system states shall be transmitted to the Ground Control Station.

---

## 5. Reliability Requirements

The system should:

- Detect abnormal temperature conditions.
- Monitor relevant electrical parameters.
- Provide autonomous thermal control.
- Maintain essential operation during communication loss.
- Provide system-status information to the operator.
- Support independent subsystem testing before full integration.

---

## 6. Validation Requirements

Each major subsystem shall have an identifiable validation method.

| Subsystem | Validation Method |
|---|---|
| Battery Thermal Management | Temperature and electrical measurements |
| Heater Control | Heater switching and temperature-response testing |
| PHP Cooling | Thermal simulation and/or experimental testing |
| Protection Chamber | Controlled low-pressure testing |
| Thermal Cycling Protection | Repeated heating/cooling testing |
| Antenna Protection | Communication testing |
| LoRa Communication | Range/link reliability testing |
| Energy Monitoring | Voltage/current measurements |
| GCS | Interface and communication testing |
| Fail-Safe Control | Communication-loss test |

---

## 7. Evidence Requirement

Project claims shall be classified according to their available evidence:

| Status | Meaning |
|---|---|
| Designed | Engineering design or CAD exists |
| Simulated | Supported by simulation |
| Prototype | Physical implementation exists |
| Tested | Supported by experimental measurements |
| Planned | Validation has not yet been completed |

Numerical performance claims shall only be included when supported by recorded measurements or simulation results.

---

## 8. Future Requirements

Future validation should progressively evaluate:

1. Individual subsystem performance
2. Environmental effects
3. Integrated system behaviour
4. UAV-level operation
5. Controlled HAA/SHAA environmental conditions
6. Mission-level energy and reliability performance
