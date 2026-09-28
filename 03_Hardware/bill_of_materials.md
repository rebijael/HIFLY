# HIFLY — Bill of Materials

## 1. Purpose

This document provides the current hardware inventory for the HIFLY prototype and development system.

Component specifications should be updated using the exact part numbers and datasheets of the hardware actually used by the team.

---

## 2. Core Hardware

| No. | Component | Function | Status |
|---|---|---|---|
| 1 | Li-ion Battery Pack | Primary electrical power source | Prototype |
| 2 | LilyGO T3-S3 LoRa | Onboard controller and LoRa communication | Prototype |
| 3 | Temperature Sensor | Battery/system temperature measurement | Prototype |
| 4 | Voltage Sensor / Monitoring Circuit | Battery voltage measurement | Prototype |
| 5 | Current Sensor / Monitoring Circuit | Battery/system current measurement | Prototype |
| 6 | Heating Element | Controlled battery heating | Prototype |
| 7 | MOSFET Driver / Switching Stage | Heater power switching | Prototype / Development |
| 8 | Thermal Insulation | Reduces unwanted heat loss | Prototype |
| 9 | Pulsating Heat Pipe | Passive processor/electronics cooling | CAD / Simulation |
| 10 | Low-Pressure Protection Chamber | Protection of sensitive electrical/electronic components | Design |
| 11 | Flexible Silicone Protection | Thermal-cycle/environmental protection | Design |
| 12 | Lightweight Protective Layer | Environmental protection for sensitive electronics | Concept / Design |
| 13 | Antenna / Radome Assembly | Communication and environmental protection | Design / Prototype |

---

## 3. Battery Thermal Management Hardware

### Battery Pack

**Function:** Primary energy source for the HIFLY electrical system.

**Required Documentation:**

- Battery photograph
- Battery configuration
- Rated voltage
- Capacity
- Manufacturer/model, if applicable
- Datasheet, if available

**Status:** Prototype

---

### Temperature Sensor

**Function:** Measures battery temperature for thermal monitoring and control.

**Required Documentation:**

- Sensor model
- Measurement range
- Accuracy
- Wiring
- Datasheet

**Status:** Prototype

---

### Heating Element

**Function:** Provides controlled thermal energy to support battery temperature management.

**Required Documentation:**

- Heater type
- Rated voltage
- Rated power
- Physical dimensions
- Datasheet, if available

**Status:** Prototype

---

### MOSFET Driver / Switching Stage

**Function:** Controls electrical power delivered to the heating element.

**Required Documentation:**

- MOSFET part number
- Voltage/current rating
- Gate-control arrangement
- Driver circuit
- Datasheet

**Status:** Prototype / Development

---

### Thermal Insulation

**Function:** Reduces unwanted heat transfer from the battery thermal-management assembly.

**Required Documentation:**

- Material
- Thickness
- Physical dimensions
- Temperature rating
- Datasheet, if available

**Status:** Prototype

---

## 4. Electrical Monitoring Hardware

### Voltage Monitoring

**Function:** Measures battery voltage for electrical-state and energy monitoring.

**Required Documentation:**

- Sensor/circuit type
- Measurement range
- Connection diagram
- Calibration information

**Status:** Prototype

---

### Current Monitoring

**Function:** Measures electrical current for power and energy analysis.

**Required Documentation:**

- Sensor type
- Measurement range
- Connection diagram
- Calibration information

**Status:** Prototype

---

## 5. Controller and Communication Hardware

### LilyGO T3-S3 LoRa

**Function:**

- Sensor data acquisition
- Thermal-control logic
- Heater control
- Power monitoring
- LoRa communication
- System-status monitoring
- Autonomous control

**Required Documentation:**

- Board model
- Pin configuration
- Firmware
- Wiring
- Communication configuration
- Datasheet/product documentation

**Status:** Prototype

---

## 6. Pulsating Heat Pipe Hardware

### Pulsating Heat Pipe

**Function:** Passive heat-transfer mechanism for processor/electronics cooling.

**Required Documentation:**

- CAD model
- Dimensions
- Material
- Working-fluid information, where applicable
- Thermal simulation
- Experimental results, where available

**Status:** CAD / Simulation

---

## 7. Protection Hardware

### Low-Pressure Protection Chamber

**Function:** Provides a controlled protective enclosure for sensitive electrical/electronic components.

**Design Considerations:**

- Enclosure geometry
- Insulation
- Creepage and clearance
- Component accessibility
- Weight
- Available space

**Status:** Design

---

### Flexible Silicone Protection

**Function:** Provides a flexible protective layer around selected electronics and accommodates thermal expansion/contraction.

**Required Documentation:**

- Material specification
- Application method
- Thickness
- Temperature rating
- Thermal-cycle testing

**Status:** Design

---

### Lightweight Protective Layer

**Function:** Provides additional environmental protection for sensitive electronic components.

**Design Considerations:**

- Material
- Thickness
- Weight
- Protection characteristics
- Thermal behaviour
- Integration constraints

**Status:** Concept / Design

---

## 8. Antenna Protection Hardware

### Antenna / Radome Assembly

**Function:** Supports communication while reducing environmental exposure of the antenna.

**Protection Concept:**

- Hydrophobic surface
- RF-transparent radome
- Protection against water/ice accumulation

**Required Documentation:**

- Antenna model
- Radome material
- Radome dimensions
- Communication test results
- Environmental test results, where available

**Status:** Design / Prototype

---

## 9. Hardware Procurement Status

| Category | Status |
|---|---|
| Battery | Available / Prototype |
| Sensors | Available / Prototype |
| Heating System | Available / Prototype |
| Controller | Available / Prototype |
| LoRa Communication | Available / Prototype |
| Thermal Insulation | Prototype |
| PHP | CAD / Development |
| Protection Chamber | Design |
| Silicone Protection | Design |
| Protective Layer | Design |
| Antenna Protection | Design / Development |

> Update these statuses according to the actual project inventory.

---

## 10. Part Number Register

Exact part numbers should be entered here after confirming the physical components used by the team.

| Component | Manufacturer | Part Number | Quantity | Datasheet |
|---|---|---|---:|---|
| Li-ion Battery | TBD | TBD | TBD | TBD |
| Temperature Sensor | TBD | TBD | TBD | TBD |
| Voltage Monitoring | TBD | TBD | TBD | TBD |
| Current Sensor | TBD | TBD | TBD | TBD |
| Heating Element | TBD | TBD | TBD | TBD |
| MOSFET | TBD | TBD | TBD | TBD |
| LilyGO T3-S3 LoRa | LilyGO | T3-S3 | TBD | TBD |
| Thermal Insulation | TBD | TBD | TBD | TBD |
| PHP | HIFLY Design | Prototype | TBD | Internal Design |
| Protection Chamber | HIFLY Design | Prototype/Design | TBD | Internal Design |
| Silicone Protection | TBD | TBD | TBD | TBD |
| Protective Layer | TBD | TBD | TBD | TBD |
| Antenna/Radome | TBD | TBD | TBD | TBD |

---

## 11. Procurement Notes

Before final procurement documentation is completed:

1. Confirm the exact component physically used.
2. Record the manufacturer and part number.
3. Save the corresponding datasheet.
4. Record the quantity used.
5. Record the electrical specifications.
6. Record the purchase/procurement source where appropriate.
7. Update the status when the component is received and integrated.

---

## 12. Evidence

Supporting hardware evidence should be stored in:

```text
03_Hardware/
├── datasheets/
├── wiring/
└── pcb/
```

Prototype photographs should be stored in:

```text
10_Media/prototype_photos/
10_Media/battery_photos/
```

CAD files should be stored in:

```text
05_CAD/
```

Testing data should be stored in:

```text
07_Testing/
08_Data/
```

---

## 13. Important Documentation Rule

This Bill of Materials is a living document.

Only components that are actually selected, purchased, built or confirmed for HIFLY should be marked as implemented.

Do not add a component specification, part number, rating or performance value unless it has been verified from the actual component or its documentation.
