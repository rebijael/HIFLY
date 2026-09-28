# HIFLY — Software Overview

## 1. Software Architecture

The HIFLY software system connects onboard sensing, thermal control, power monitoring, communication, Ground Control Station monitoring and fail-safe operation.

The software is designed around the LilyGO T3-S3 LoRa onboard controller.

---

## 2. Software Functions

The main software functions are:

- Sensor data acquisition
- Battery temperature monitoring
- Voltage monitoring
- Current monitoring
- Temperature-based thermal control
- Heater control
- Power monitoring
- LoRa communication
- Ground Control Station data handling
- System-status monitoring
- Fail-safe operation

---

## 3. Software Architecture

```text
                 SENSOR INPUTS
                      │
        ┌─────────────┼─────────────┐
        │             │             │
   Temperature     Voltage       Current
      Sensor       Monitor        Sensor
        │             │             │
        └─────────────┼─────────────┘
                      ↓
               DATA ACQUISITION
                      │
                      ↓
             LILYGO T3-S3 CONTROLLER
                      │
       ┌──────────────┼──────────────┐
       │              │              │
       ↓              ↓              ↓
 Thermal Control  Power Monitor   System Status
       │              │              │
       ↓              │              ↓
    MOSFET            │         Safety / Alerts
       │              │
       ↓              │
    Heater            │
                      │
       └──────────────┼──────────────┐
                      ↓              │
                LoRa Communication   │
                      │              │
                      ↓              │
                     GCS ←───────────┘
```

---

## 4. Embedded Firmware

The embedded firmware runs on the onboard controller.

### Main Responsibilities

The firmware is responsible for:

1. Initializing connected hardware
2. Reading sensors
3. Processing temperature measurements
4. Monitoring voltage
5. Monitoring current
6. Controlling the heating element
7. Updating system status
8. Communicating data through LoRa
9. Handling communication loss
10. Maintaining autonomous thermal-control operation

---

## 5. Sensor Acquisition

The firmware periodically reads the connected sensors.

### Inputs

- Battery temperature
- Battery voltage
- Battery current
- Ambient temperature, where implemented

### Processing

```text
Sensor
  ↓
Sensor Reading
  ↓
Validation / Processing
  ↓
System Variable
  ↓
Thermal Control / Monitoring
```

The actual sensor sampling interval should be documented in the firmware configuration.

---

## 6. Temperature-Based Thermal Control

The thermal-control logic uses measured battery temperature to determine heater operation.

Conceptually:

```text
Battery Temperature
        ↓
Compare with Control Condition
        ↓
   ┌────┴────┐
   │         │
Heating    Heating
Required   Not Required
   │         │
   ↓         ↓
Heater ON  Heater OFF
```

The exact temperature thresholds must match the implemented firmware.

### Important

The repository should document the actual control thresholds once finalized.

No specific threshold should be claimed here until it is confirmed from the implemented firmware.

---

## 7. Heater Control

The heating element is controlled through a MOSFET switching stage.

```text
Thermal Control Logic
          ↓
     Control Signal
          ↓
        MOSFET
          ↓
   Heating Element
          ↓
        Battery
```

The firmware determines the required heater state.

The MOSFET provides the electrical switching interface between the controller and heating element.

---

## 8. Power Monitoring

The software processes voltage and current measurements to monitor electrical power.

The basic relationship is:

```text
Power = Voltage × Current
```

The measured data can be used for:

- Heater power analysis
- System power monitoring
- Energy-consumption analysis
- Mission-level energy assessment

Raw measurements should be preserved before processing.

---

## 9. LoRa Communication

LoRa provides the communication link between the onboard controller and the Ground Control Station.

```text
Onboard Sensors
      ↓
LilyGO T3-S3
      ↓
LoRa
      ↓
Ground Control Station
```

The communication system can transmit relevant measurements and system-status information.

### Example Data

- Battery temperature
- Voltage
- Current
- Heater status
- Thermal status
- Operating mode
- Safety status
- Communication status

The exact communication packet structure should be documented in the firmware implementation.

---

## 10. Ground Control Station

The Ground Control Station receives and displays information from the onboard system.

### Planned Dashboard Information

- Battery temperature
- Ambient temperature
- Voltage
- Current
- Heater status
- Thermal status
- LoRa link status
- Operating mode
- Safety status
- Alerts

### Planned Controls

Where implemented:

- AUTO
- PRE-HEAT
- HEATER OFF
- Manual override

The GCS software and interface are maintained in the GCS section of the repository.

---

## 11. Fail-Safe Software Behaviour

The onboard controller is designed to maintain essential thermal-control functions during communication loss.

### Normal Condition

```text
Sensors
   ↓
Controller
   ↓
Thermal Control
   ↓
Heater
   ↓
LoRa
   ↓
GCS
```

### Communication Loss

```text
Communication Lost
        ↓
Onboard Detection
        ↓
Continue Autonomous Control
        ↓
Maintain Implemented Thermal Logic
        ↓
Communication Restored
        ↓
Resume GCS Communication
```

The exact communication-loss timeout and recovery behaviour must be documented using the implemented firmware.

---

## 12. Software State Concept

The system can be organized around operating states such as:

```text
INITIALIZATION
      ↓
   MONITORING
      ↓
   THERMAL CONTROL
      ↓
 COMMUNICATION
      ↓
   FAIL-SAFE
      ↓
   RECOVERY
```

The exact state-machine implementation will depend on the final firmware.

---

## 13. Software Data Flow

```text
Temperature ─┐
Voltage ──────┼──→ Data Acquisition
Current ──────┘           │
                          ↓
                 Onboard Controller
                          │
             ┌────────────┼────────────┐
             ↓            ↓            ↓
        Thermal Logic  Power Data   System Status
             │            │            │
             ↓            │            │
          Heater           │            │
                          └──────┬─────┘
                                 ↓
                            LoRa Packet
                                 ↓
                                GCS
```

---

## 14. Alerts and Fault Conditions

The software architecture supports monitoring for conditions such as:

- Low temperature
- Over-temperature
- Sensor fault
- Communication loss
- Abnormal voltage
- Abnormal current

The exact alert thresholds must be defined using the actual implemented system requirements and firmware.

---

## 15. Autonomous Operation

The onboard controller is intended to provide essential thermal-control operation without requiring continuous operator intervention.

This supports operation when:

- Communication is temporarily unavailable
- The UAV is outside continuous GCS coverage
- Manual intervention is not immediately available

The actual autonomous behaviour is limited to functions implemented and tested in the firmware.

---

## 16. Firmware Development Structure

The firmware can be organized into functional modules:

```text
firmware/
├── sensor acquisition
├── temperature monitoring
├── voltage monitoring
├── current monitoring
├── thermal control
├── heater control
├── power monitoring
├── LoRa communication
├── fault handling
└── system status
```

The final source-code organization should reflect the actual implementation.

---

## 17. Software Testing

Software testing should be performed progressively.

### Stage 1 — Sensor Testing

Verify:

- Temperature readings
- Voltage readings
- Current readings

### Stage 2 — Heater Control

Verify:

- Heater ON command
- Heater OFF command
- MOSFET switching
- Temperature response

### Stage 3 — Communication

Verify:

- LoRa transmission
- Data reception
- Data integrity
- Communication-loss detection

### Stage 4 — Fail-Safe

Verify:

- Communication loss
- Autonomous controller response
- Recovery after communication restoration

### Stage 5 — Integrated System

Verify:

- Sensor acquisition
- Thermal control
- Heater operation
- Power monitoring
- LoRa communication
- GCS monitoring
- Fail-safe behaviour

---

## 18. Software Evidence

Software evidence should include:

- Firmware source code
- Configuration files
- Pin assignments
- Communication protocol
- Screenshots
- Test logs
- GCS interface
- Recorded system outputs

These should be added as the implementation progresses.

---

## 19. Software Status

| Function | Status |
|---|---|
| Sensor Acquisition | Development / Prototype |
| Temperature Monitoring | Prototype |
| Voltage Monitoring | Prototype |
| Current Monitoring | Prototype |
| Thermal Control Logic | Development |
| Heater Control | Prototype / Development |
| Power Monitoring | Development |
| LoRa Communication | Prototype |
| GCS | Development |
| Fail-Safe Control | Development |
| Integrated Firmware | Development |

Status should be updated according to the actual implementation.

---

## 20. Software Documentation Rule

Only implemented firmware functionality should be described as operational.

Control thresholds, communication timings, GPIO assignments, packet formats and fault-handling behaviour should be added only after they are confirmed from the actual firmware and prototype configuration.
