# HIFLY — Thermal Control Logic

## 1. Overview

The HIFLY thermal-control system uses battery temperature as the primary control input for regulating the battery heating element.

The control system combines:

- Temperature sensing
- Onboard processing
- Temperature-based control logic
- MOSFET switching
- Heating element
- Battery thermal insulation
- System monitoring
- Optional GCS monitoring and control

---

## 2. Control Architecture

```text
Battery
   │
   ↓
Temperature Sensor
   │
   ↓
LilyGO T3-S3
   │
   ↓
Temperature-Based Control Logic
   │
   ├───────────────┐
   │               │
   ↓               ↓
Heater ON       Heater OFF
   │               │
   ↓               ↓
MOSFET          MOSFET
   │               │
   ↓               ↓
Heating Element    No Heating
   │
   ↓
Battery Temperature
   │
   └──────────→ Feedback
```

The measured temperature provides feedback to the control system.

---

## 3. Control Objective

The primary objective is to provide controlled thermal support to the battery during low-temperature operation.

The controller should:

- Monitor battery temperature
- Determine whether heating is required
- Activate the heating element when required
- Stop heating when heating is no longer required
- Continue monitoring during operation
- Provide system status to the GCS where communication is available

---

## 4. Basic Control Sequence

```text
START
  ↓
Initialize System
  ↓
Read Battery Temperature
  ↓
Validate Temperature Reading
  ↓
Evaluate Thermal Condition
  ↓
Is Heating Required?
  │
  ├── YES → Heater ON
  │           ↓
  │      Continue Monitoring
  │
  └── NO  → Heater OFF
              ↓
         Continue Monitoring
              ↓
          Repeat Cycle
```

---

## 5. Temperature Feedback

Temperature feedback is used to determine the current thermal state of the battery.

```text
Temperature Measurement
          ↓
     Controller
          ↓
   Thermal Evaluation
          ↓
      Heater State
          ↓
   Battery Temperature
          ↓
      New Reading
```

This creates a closed monitoring and control loop.

---

## 6. Control Thresholds

The final firmware should define the temperature conditions used to control the heater.

The exact values are intentionally not specified in this document until they are confirmed from:

- Battery specifications
- Actual prototype requirements
- Experimental testing
- Final firmware implementation

### Required Parameters

| Parameter | Value |
|---|---|
| Heating ON threshold | TBD |
| Heating OFF threshold | TBD |
| Hysteresis | TBD |
| Sensor sampling interval | TBD |
| Maximum allowed battery temperature | TBD |
| Fault temperature threshold | TBD |

> Do not replace TBD values with assumed values. Enter the actual values once the control system has been finalized.

---

## 7. Hysteresis

If hysteresis is implemented, separate temperature conditions should be used for heater activation and heater deactivation.

Conceptually:

```text
Temperature
     ↑
     │
 ON  ├──────── Heating Required
     │
     │
 OFF ├──────── Heating Not Required
     │
     └────────────────────────→ Time
```

The purpose of hysteresis is to prevent unnecessary rapid switching of the heater around a single temperature threshold.

The actual hysteresis value must be documented from the implemented firmware.

---

## 8. Heater Switching

The controller does not directly provide the heating element's power.

Instead, the controller provides a control signal to the MOSFET switching stage.

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
      │
      ↓
Battery Thermal System
```

The exact MOSFET and driver configuration must match the physical prototype.

---

## 9. Thermal Insulation

Thermal insulation supports the heater by reducing unwanted heat transfer from the battery thermal-management assembly to the surrounding environment.

```text
Heating Element
       ↓
Battery
       ↓
Thermal Insulation
       ↓
External Environment
```

The actual insulation material, thickness and measured thermal performance should be documented separately.

---

## 10. Power-Aware Thermal Management

The heater is an electrical load and therefore contributes to overall energy consumption.

HIFLY monitors:

- Battery voltage
- Battery current
- Heater status
- Battery temperature

The resulting measurements can be used to analyze the relationship between thermal support and electrical energy consumption.

```text
Battery Temperature
        ↓
Thermal Requirement
        ↓
Heater Operation
        ↓
Electrical Power
        ↓
Energy Consumption
```

This allows thermal management to be considered together with mission energy requirements.

---

## 11. GCS Interaction

The Ground Control Station can provide visibility of the thermal-control system.

### Displayed Information

- Battery temperature
- Heater status
- Thermal status
- Voltage
- Current
- Operating mode
- Communication status
- Safety status

### Possible Controls

Where implemented:

```text
AUTO
PRE-HEAT
HEATER OFF
MANUAL OVERRIDE
```

The GCS should not be treated as the sole thermal-control mechanism.

Essential onboard control remains available through the embedded system.

---

## 12. Autonomous Operation

The thermal-control logic is implemented onboard.

```text
              GCS
               │
               │ LoRa
               ↓
        Monitoring / Commands
               │
               ↓
        Onboard Controller
               │
               ↓
       Thermal Control Logic
               │
               ↓
             Heater
```

The onboard controller can continue its implemented thermal-control behaviour when the GCS communication link is unavailable.

---

## 13. Communication-Loss Behaviour

The intended behaviour during communication loss is:

```text
Communication Lost
        ↓
Onboard Controller Detects Loss
        ↓
Continue Temperature Monitoring
        ↓
Continue Implemented Thermal Logic
        ↓
Maintain Safe Heater Behaviour
        ↓
Communication Restored
        ↓
Resume GCS Communication
```

The exact communication-loss timeout and recovery behaviour must be documented after implementation.

---

## 14. Fault Handling

Potential thermal-control faults include:

### Sensor Fault

If the temperature sensor provides an invalid or unavailable reading, the system should enter the defined safe state.

### Over-Temperature

If the measured temperature exceeds the defined safe limit, the heater should be disabled according to the implemented safety logic.

### Communication Loss

Loss of GCS communication should not prevent essential onboard thermal monitoring and control.

### Abnormal Electrical Conditions

Voltage and current measurements can be monitored for abnormal operating conditions.

The exact fault thresholds and responses must be defined in the final firmware.

---

## 15. Thermal Control State Concept

The control system can be represented using the following states:

```text
          ┌──────────────┐
          │ INITIALIZE   │
          └──────┬───────┘
                 ↓
          ┌──────────────┐
          │   MONITOR    │
          └──────┬───────┘
                 ↓
       ┌─────────┴─────────┐
       ↓                   ↓
Heating Required       No Heating
       ↓                   ↓
┌──────────────┐     ┌──────────────┐
│ HEATER ON    │     │ HEATER OFF   │
└──────┬───────┘     └──────┬───────┘
       │                     │
       └──────────┬──────────┘
                  ↓
              MONITOR
```

Additional fault and fail-safe states may be added during firmware development.

---

## 16. Pseudocode Concept

The following represents the control concept rather than the final firmware implementation:

```text
START

Initialize sensors
Initialize controller
Initialize heater control
Initialize communication

LOOP

Read battery temperature
Read battery voltage
Read battery current

Validate sensor readings

IF temperature reading is invalid:
    Apply defined safe state

ELSE:

    Evaluate battery temperature

    IF heating is required:
        Heater = ON

    ELSE:
        Heater = OFF

    Monitor voltage and current
    Calculate power where required
    Update system status
    Transmit data to GCS where communication is available

    Check communication status

    IF communication is lost:
        Continue onboard thermal control

REPEAT
```

The final firmware may use a different implementation while maintaining the same functional requirements.

---

## 17. Data Required for Validation

Thermal-control validation should record at minimum:

- Time or sample index
- Battery temperature
- Voltage
- Current
- Heater state
- Operating mode
- Ambient temperature, where available
- Communication status

Example data structure:

| Sample | Temperature (°C) | Voltage (V) | Current (A) | Heater | Mode |
|---:|---:|---:|---:|---|---|
| 1 | TBD | TBD | TBD | OFF | AUTO |
| 2 | TBD | TBD | TBD | ON | AUTO |
| 3 | TBD | TBD | TBD | ON | AUTO |
| 4 | TBD | TBD | TBD | OFF | AUTO |

Replace the example values with actual recorded measurements.

---

## 18. Validation Procedure

A basic thermal-control validation sequence can be:

```text
Prepare Battery System
        ↓
Connect Sensors
        ↓
Verify Electrical Connections
        ↓
Record Initial Temperature
        ↓
Start Data Logging
        ↓
Enable Thermal Control
        ↓
Record Temperature Response
        ↓
Record Voltage / Current
        ↓
Record Heater State
        ↓
Evaluate Control Behaviour
        ↓
Repeat Under Defined Conditions
```

Testing conditions must be documented for every experiment.

---

## 19. Required Validation Outputs

The following outputs should be generated from actual testing:

### Temperature Response

Battery temperature versus time or sample index.

### Heater Behaviour

Heater ON/OFF state versus temperature.

### Electrical Behaviour

Voltage and current during thermal-management operation.

### Power Consumption

Electrical power associated with heater operation.

### Energy Consumption

Energy used by the thermal-management system over the defined test period.

---

## 20. Validation Status

| Feature | Status |
|---|---|
| Temperature Sensing | Prototype |
| Heater Control | Prototype / Development |
| Temperature-Based Logic | Development |
| Voltage Monitoring | Prototype |
| Current Monitoring | Prototype |
| Power Monitoring | Development |
| GCS Monitoring | Development |
| Autonomous Thermal Control | Development |
| Communication-Loss Handling | Development |
| Full Thermal Validation | Planned / In Progress |

Update the status according to the actual prototype and test evidence.

---

## 21. Evidence Rule

The thermal-control system should not be described as validated until the corresponding measurements have been recorded and documented.

Required evidence may include:

- Prototype photographs
- Firmware source code
- Temperature data
- Voltage/current data
- Heater-state data
- Graphs
- Test conditions
- Test setup photographs
- Validation results

All raw measurements should be preserved in the project repository.

---

## 22. Related Files

Firmware overview:

```text
04_Software/README.md
```

Firmware documentation:

```text
04_Software/firmware/README.md
```

Hardware overview:

```text
03_Hardware/hardware_overview.md
```

Wiring:

```text
03_Hardware/wiring/README.md
```

Testing:

```text
07_Testing/
```

Data:

```text
08_Data/
```
