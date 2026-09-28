# HIFLY — Embedded Firmware

## 1. Overview

The HIFLY embedded firmware runs on the onboard LilyGO T3-S3 LoRa controller.

The firmware connects the physical sensing and control hardware with the communication and monitoring system.

Its main functions are:

- Sensor acquisition
- Temperature monitoring
- Voltage monitoring
- Current monitoring
- Thermal-control logic
- Heater control
- Power monitoring
- LoRa communication
- System-status monitoring
- Fail-safe operation

---

## 2. Firmware Architecture

```text
                    HIFLY FIRMWARE
                           │
                  ┌────────┴────────┐
                  │                 │
             SENSOR LAYER       CONTROL LAYER
                  │                 │
        ┌─────────┼─────────┐       │
        │         │         │       │
   Temperature  Voltage   Current    │
        │         │         │        │
        └─────────┼─────────┘        │
                  │                  │
                  ↓                  ↓
            Data Processing    Thermal Control
                  │                  │
                  │                  ↓
                  │               MOSFET
                  │                  │
                  │                  ↓
                  │               Heater
                  │
                  └────────┬─────────┐
                           │         │
                           ↓         ↓
                     Power Monitor  System Status
                           │         │
                           └────┬────┘
                                ↓
                           LoRa Communication
                                │
                                ↓
                               GCS
```

---

## 3. Firmware Responsibilities

### Sensor Acquisition

The firmware reads data from connected sensors.

### Thermal Control

The firmware processes battery temperature and determines the required heater state according to the implemented control logic.

### Power Monitoring

Voltage and current measurements are processed to provide electrical and energy information.

### Communication

Relevant system data is transmitted through the LoRa communication link.

### Fail-Safe Control

Essential onboard thermal-control functions can continue when communication with the Ground Control Station is unavailable.

---

## 4. Main Firmware Loop

The general firmware sequence is:

```text
System Initialization
        ↓
Initialize Sensors
        ↓
Initialize LoRa
        ↓
Read Sensors
        ↓
Process Measurements
        ↓
Evaluate Thermal Condition
        ↓
Control Heater
        ↓
Calculate / Monitor Power
        ↓
Update System Status
        ↓
Transmit Data
        ↓
Check Communication Status
        ↓
Repeat
```

The exact implementation may change as the firmware develops.

---

## 5. System Initialization

During startup, the firmware should initialize the required hardware and software functions.

### Initialization Tasks

- Start serial communication, where used
- Initialize temperature sensor
- Initialize voltage monitoring
- Initialize current monitoring
- Initialize heater-control output
- Initialize MOSFET control
- Initialize LoRa communication
- Initialize system-status variables
- Set the initial safe heater state

The final initialization sequence should match the implemented firmware.

---

## 6. Temperature Monitoring

The firmware continuously or periodically reads the battery temperature.

```text
Temperature Sensor
        ↓
Sensor Reading
        ↓
Firmware Processing
        ↓
Battery Temperature Variable
        ↓
Thermal Control
```

The temperature value is also available for system monitoring and GCS transmission.

---

## 7. Voltage Monitoring

Battery voltage is measured and processed by the firmware.

The value can be used for:

- Battery monitoring
- Power calculation
- Energy analysis
- System-status reporting

The actual voltage-sensing implementation should be documented using the final hardware configuration.

---

## 8. Current Monitoring

The firmware reads the current-monitoring input.

Current information can be used for:

- System power calculation
- Heater power analysis
- Energy-consumption analysis
- GCS display

The exact current-sensor implementation should match the actual prototype.

---

## 9. Thermal Control Logic

The thermal-control system uses battery temperature as a primary control input.

```text
Battery Temperature
        ↓
Read Sensor
        ↓
Evaluate Control Condition
        ↓
 ┌──────┴──────┐
 │             │
Heating      Heating
Required     Not Required
 │             │
 ↓             ↓
Heater ON    Heater OFF
```

The exact temperature thresholds, hysteresis and control timing must be documented from the implemented firmware.

No specific threshold is claimed in this document until it has been confirmed in the final firmware.

---

## 10. Heater Control

The firmware controls the heating element through the MOSFET switching stage.

```text
Firmware
   ↓
Heater Control Signal
   ↓
MOSFET
   ↓
Heating Element
   ↓
Battery
```

The firmware should provide defined ON and OFF states.

Where PWM or another modulation method is implemented, the actual implementation should be documented here.

---

## 11. Power Monitoring

The firmware can calculate electrical power from measured voltage and current.

```text
Voltage ─────┐
             ├──→ Power Calculation
Current ─────┘
```

The basic relationship is:

```text
Power = Voltage × Current
```

Power data can be used for:

- Heater power monitoring
- System power monitoring
- Energy analysis
- Mission-level assessment

---

## 12. Energy Monitoring

Energy consumption can be estimated by integrating measured power over time.

Conceptually:

```text
Voltage + Current
       ↓
Power
       ↓
Power over Time
       ↓
Energy Consumption
```

Actual energy calculations should use recorded measurements and a documented sampling interval.

---

## 13. LoRa Communication

The LilyGO T3-S3 provides the onboard LoRa communication interface.

The firmware prepares relevant system information for transmission to the GCS.

### Example Data

- Battery temperature
- Ambient temperature, where implemented
- Battery voltage
- Battery current
- Heater status
- Thermal status
- Operating mode
- Safety status
- Fault status

The final packet structure should be documented once the communication protocol is finalized.

---

## 14. Communication Status

The firmware should maintain a communication-status indicator.

Conceptually:

```text
LoRa Communication
        ↓
Communication Status
        ↓
 ┌──────┴──────┐
 │             │
CONNECTED    LOST
 │             │
 ↓             ↓
Normal       Fail-Safe
Operation    Operation
```

The actual timeout and detection mechanism must be documented according to the implemented firmware.

---

## 15. Fail-Safe Behaviour

The firmware is designed around the principle that essential thermal control should remain available during temporary communication loss.

```text
Communication Lost
        ↓
Onboard Detection
        ↓
Continue Sensor Monitoring
        ↓
Continue Thermal Control
        ↓
Maintain Implemented Safety Logic
        ↓
Communication Restored
        ↓
Resume Normal Communication
```

The exact fail-safe behaviour must be tested before being described as validated.

---

## 16. Fault Monitoring

The firmware architecture can monitor conditions such as:

- Sensor fault
- Low temperature
- Over-temperature
- Abnormal voltage
- Abnormal current
- Communication loss

The exact detection thresholds must be defined using the actual system requirements.

---

## 17. Operating Modes

The software architecture supports operating modes such as:

### AUTO

The onboard thermal-control logic controls heater operation automatically.

### PRE-HEAT

The heater is operated according to the implemented pre-heating procedure.

### HEATER OFF

The heating element is commanded off.

### MANUAL

Where implemented, the operator can control the heater from the GCS.

The exact mode implementation must match the final firmware.

---

## 18. Data Logging

The firmware should support recording or transmitting measurements required for later analysis.

Recommended logged parameters include:

```text
Timestamp
Battery Temperature
Ambient Temperature
Voltage
Current
Power
Heater Status
Operating Mode
Communication Status
Fault Status
```

Raw data should be preserved for traceability.

---

## 19. Firmware Testing

Firmware testing should be performed progressively.

### Test 1 — Sensor Reading

Verify that:

- Temperature readings are received correctly.
- Voltage readings are received correctly.
- Current readings are received correctly.

### Test 2 — Heater Control

Verify that:

- Heater ON command works.
- Heater OFF command works.
- MOSFET switching operates correctly.
- Temperature response is recorded.

### Test 3 — Power Monitoring

Verify:

- Voltage measurement
- Current measurement
- Power calculation
- Data transmission

### Test 4 — LoRa Communication

Verify:

- Data transmission
- Data reception
- Packet integrity
- Communication status

### Test 5 — Communication Loss

Verify:

- Communication-loss detection
- Autonomous thermal-control operation
- System recovery after communication restoration

### Test 6 — Integrated Firmware

Verify:

```text
Sensor Reading
      ↓
Thermal Control
      ↓
Heater Control
      ↓
Power Monitoring
      ↓
LoRa Communication
      ↓
GCS
```

---

## 20. Firmware Evidence

The firmware development directory should eventually contain:

```text
04_Software/firmware/
├── README.md
├── source/
├── configuration/
├── libraries/
├── communication/
├── control_logic/
└── test_logs/
```

The actual directory structure should be updated according to the final codebase.

---

## 21. Firmware Status

| Function | Status |
|---|---|
| Sensor Acquisition | Development / Prototype |
| Temperature Monitoring | Prototype |
| Voltage Monitoring | Prototype |
| Current Monitoring | Prototype |
| Thermal Control | Development |
| Heater Control | Prototype / Development |
| Power Monitoring | Development |
| LoRa Communication | Prototype |
| Fault Monitoring | Development |
| Fail-Safe Control | Development |
| GCS Integration | Development |
| Full Integrated Firmware | Development |

---

## 22. Firmware Documentation Rule

Only functionality that exists in the actual firmware should be marked as implemented.

The following must be verified before being documented as final:

- GPIO assignments
- Sensor configuration
- Temperature thresholds
- Hysteresis
- Sampling interval
- Heater-control method
- LoRa configuration
- Communication timeout
- Packet format
- Fault thresholds
- Fail-safe behaviour

This keeps the HIFLY repository synchronized with the actual prototype.
