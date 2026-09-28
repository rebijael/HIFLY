# HIFLY — Ground Control Station (GCS)

## 1. Overview

The HIFLY Ground Control Station (GCS) provides a monitoring and control interface for the thermal-management and power-monitoring system.

The GCS receives telemetry from the onboard controller through the LoRa communication link and presents important system information to the operator.

The GCS is intended to provide:

- Real-time system monitoring
- Battery thermal monitoring
- Electrical parameter monitoring
- Heater status
- Communication status
- Safety status
- Operating-mode indication
- Thermal alerts
- Operator controls where implemented
- Data logging and visualization

---

## 2. GCS Role in HIFLY

The overall system is divided into:

```text
                HIFLY SYSTEM
                     │
        ┌────────────┴────────────┐
        │                         │
        ↓                         ↓
 ONBOARD SYSTEM                 GCS
        │                         │
        │                         │
 Sensors + Control          Monitoring + UI
        │                         │
        └──────── LoRa ───────────┘
```

The onboard controller performs the essential thermal-control functions.

The GCS provides visibility of the system and, where implemented, allows the operator to issue permitted commands.

---

## 3. GCS Architecture

```text
Battery
   │
   ├── Temperature Sensor
   ├── Voltage Monitoring
   └── Current Monitoring
             │
             ↓
       LilyGO T3-S3
             │
      Thermal Control
             │
             ↓
       LoRa Telemetry
             │
             ↓
        Ground Receiver
             │
             ↓
        GCS Application
             │
      ┌──────┼──────┐
      ↓      ↓      ↓
   Status  Graphs  Alerts
```

---

## 4. Telemetry Parameters

The GCS should display the following parameters where the corresponding sensors and firmware are implemented.

| Parameter | Description |
|---|---|
| Battery Temperature | Measured battery temperature |
| Ambient Temperature | Measured surrounding temperature, where available |
| Voltage | Battery/system voltage |
| Current | Battery/system current |
| Heater Status | Current heater state |
| Thermal Status | Current thermal-control condition |
| Operating Mode | AUTO / PRE-HEAT / HEATER OFF / MANUAL where implemented |
| LoRa Status | Communication-link condition |
| Safety Status | Current safety state |
| Sensor Status | Sensor availability/validity |

---

## 5. Main Dashboard

The main GCS screen should provide an operator with a quick overview of the system.

Suggested layout:

```text
+------------------------------------------------------+
|                    HIFLY GCS                         |
+------------------------------------------------------+
| Battery Temp | Ambient Temp | Voltage | Current      |
|     --- °C   |     --- °C   | --- V   | --- A       |
+------------------------------------------------------+
| Heater       | Thermal      | LoRa     | Safety      |
| OFF          | NORMAL       | CONNECTED| SAFE        |
+------------------------------------------------------+
|                 TEMPERATURE GRAPH                    |
|                                                      |
|                                                      |
+------------------------------------------------------+
| MODE: AUTO                                           |
+------------------------------------------------------+
| [AUTO] [PRE-HEAT] [HEATER OFF] [MANUAL]             |
+------------------------------------------------------+
| Alerts:                                              |
| - No active alerts                                   |
+------------------------------------------------------+
```

The final interface may use a different visual layout.

---

## 6. Temperature Monitoring

Battery temperature is one of the primary parameters displayed by the GCS.

The GCS should allow the operator to observe:

- Current battery temperature
- Temperature trend
- Temperature history
- Thermal status
- Temperature-related alerts

Example:

```text
Battery Temperature

Temperature
   ↑
   │
   │       ______
   │      /
   │     /
   │____/
   │
   └────────────────→ Time / Sample
```

The graph should use actual recorded telemetry.

---

## 7. Electrical Monitoring

The GCS can display:

### Voltage

```text
Voltage: --- V
```

### Current

```text
Current: --- A
```

### Power

Where voltage and current measurements are available:

```text
Power = Voltage × Current
```

The calculated power value should be clearly identified as a calculated parameter.

---

## 8. Heater Status

The GCS should clearly show whether the heating element is active.

Example:

```text
HEATER STATUS

OFF
```

or

```text
HEATER STATUS

ON
```

The status must be based on the actual heater-control state reported by the onboard controller.

---

## 9. Thermal Status

The GCS can provide a simple thermal-state indication.

Example states:

```text
NORMAL
HEATING
LOW TEMPERATURE
OVER TEMPERATURE
SENSOR FAULT
```

Only states actually implemented in the firmware should be displayed as active system states.

---

## 10. Operating Modes

The planned operating modes are:

### AUTO

The onboard controller determines heater operation according to the implemented temperature-control logic.

```text
AUTO
 ↓
Temperature Monitoring
 ↓
Thermal Evaluation
 ↓
Automatic Heater Control
```

### PRE-HEAT

The system can provide a dedicated pre-heating mode where implemented.

The exact pre-heating behaviour must be defined by the final firmware.

### HEATER OFF

The heating element is commanded off.

### MANUAL

Manual heater control may be provided for testing or controlled operation where implemented.

Manual operation should remain subject to safety protections.

---

## 11. Alerts

The GCS should provide clear alerts for important system conditions.

### Low Temperature

Indicates that the measured temperature has reached the configured low-temperature condition.

### Over-Temperature

Indicates that the measured temperature has reached the configured safety limit.

### Sensor Fault

Indicates that a required sensor reading is unavailable or invalid.

### Communication Lost

Indicates that expected telemetry is not being received.

---

## 12. Alert Concept

```text
Sensor Data
     │
     ↓
GCS Telemetry Parser
     │
     ↓
Condition Evaluation
     │
 ┌───┼───────────────┐
 ↓   ↓               ↓
LOW  OVER-TEMP     SENSOR
TEMP               FAULT
 │    │               │
 └────┴──────┬────────┘
             ↓
           ALERT
```

The final alert thresholds must be taken from the implemented firmware and test configuration.

---

## 13. Communication Status

The GCS should indicate the current LoRa communication condition.

Example:

```text
LoRa LINK

CONNECTED
```

or

```text
LoRa LINK

NO TELEMETRY
```

The exact timeout used to determine communication loss must be documented after implementation.

---

## 14. Communication-Loss Behaviour

Loss of communication should not automatically mean that onboard thermal control stops.

The intended architecture is:

```text
                 LoRa Link
                    │
             ┌──────┴──────┐
             │             │
        Communication   Communication
           Available       Lost
             │             │
             ↓             ↓
          GCS Data     Onboard Control
             │             │
             └──────┬──────┘
                    ↓
              Thermal System
```

The onboard controller remains responsible for essential thermal-control behaviour.

---

## 15. Safety Status

The GCS should display the current safety condition.

Example:

```text
SAFETY STATUS

SAFE
```

Potential fault conditions include:

- Sensor fault
- Over-temperature
- Abnormal electrical condition
- Communication loss
- Controller fault

The exact safety-state machine must match the implemented firmware.

---

## 16. Telemetry Packet Concept

A telemetry packet may contain fields such as:

```text
PACKET
│
├── Packet ID
├── Battery Temperature
├── Ambient Temperature
├── Voltage
├── Current
├── Heater State
├── Thermal State
├── Operating Mode
├── Safety State
├── Communication State
└── Sensor Status
```

The exact packet format should be documented alongside the final firmware implementation.

---

## 17. Example Telemetry Record

```text
Packet ID: TBD
Battery Temperature: TBD °C
Ambient Temperature: TBD °C
Voltage: TBD V
Current: TBD A
Heater: OFF
Thermal State: NORMAL
Mode: AUTO
LoRa: CONNECTED
Safety: SAFE
Sensor Status: VALID
```

This is an interface example and is not a measured system result.

---

## 18. Data Logging

The GCS should support recording received telemetry where implemented.

Recommended fields:

| Field | Description |
|---|---|
| Timestamp | Time of received record |
| Packet ID | Telemetry packet identifier |
| Battery Temperature | Battery temperature |
| Ambient Temperature | Ambient temperature |
| Voltage | Measured voltage |
| Current | Measured current |
| Heater State | ON/OFF |
| Thermal State | Thermal condition |
| Operating Mode | Current mode |
| LoRa Status | Link condition |
| Safety Status | Safety condition |

Raw telemetry should be preserved whenever possible.

---

## 19. Temperature Graph

The GCS should provide a temperature-history graph.

Example:

```text
Temperature
     ↑
     │
     │                 *
     │             *  *
     │          * *
     │       * *
     │    * *
     │ * *
     └────────────────────────→ Time
```

The graph must be generated from actual recorded data.

If the dataset does not contain timestamps, the x-axis should be labelled using the available sample index rather than being presented as time.

---

## 20. Operator Controls

Where implemented, the GCS may provide:

```text
[AUTO]
[PRE-HEAT]
[HEATER OFF]
[MANUAL]
```

Commands should be transmitted through the LoRa communication link.

The onboard controller must validate commands before applying them.

---

## 21. Command Safety

GCS commands should not bypass essential onboard safety logic.

Conceptually:

```text
GCS Command
     ↓
LoRa
     ↓
Onboard Controller
     ↓
Command Validation
     ↓
Safety Check
     ↓
Command Accepted?
   ┌───────┴───────┐
   ↓               ↓
 YES              NO
   ↓               ↓
Execute        Reject / Safe State
```

This prevents the user interface from becoming the only safety mechanism.

---

## 22. GCS Software Structure

A possible software structure is:

```text
GCS
│
├── Communication
│   ├── LoRa Receiver
│   └── Packet Parser
│
├── Data
│   ├── Telemetry Storage
│   └── Data Processing
│
├── Dashboard
│   ├── Temperature
│   ├── Voltage
│   ├── Current
│   ├── Heater Status
│   └── Safety Status
│
├── Graphs
│   └── Temperature History
│
├── Alerts
│   ├── Low Temperature
│   ├── Over Temperature
│   ├── Sensor Fault
│   └── Communication Lost
│
└── Controls
    ├── AUTO
    ├── PRE-HEAT
    ├── HEATER OFF
    └── MANUAL
```

The final directory structure may change according to the selected GCS implementation.

---

## 23. GCS Development Requirements

The GCS implementation should provide:

- Clear telemetry display
- Reliable packet parsing
- Communication-status indication
- Thermal-status indication
- Alert handling
- Data logging
- Temperature visualization
- Operator controls where implemented
- Safe handling of missing telemetry

---

## 24. Testing Checklist

### Communication

- [ ] GCS receives telemetry
- [ ] Packet fields are decoded correctly
- [ ] Invalid packets are handled
- [ ] Communication loss is detected
- [ ] Communication recovery is detected

### Sensors

- [ ] Battery temperature is displayed
- [ ] Ambient temperature is displayed where available
- [ ] Voltage is displayed
- [ ] Current is displayed
- [ ] Sensor faults are indicated

### Thermal Control

- [ ] Heater status is displayed
- [ ] Thermal status is displayed
- [ ] AUTO mode is displayed
- [ ] PRE-HEAT mode is displayed where implemented
- [ ] HEATER OFF state is displayed
- [ ] Manual control is tested only where implemented

### Alerts

- [ ] Low-temperature alert
- [ ] Over-temperature alert
- [ ] Sensor-fault alert
- [ ] Communication-loss alert

### Data

- [ ] Telemetry logging works
- [ ] Temperature graph works
- [ ] Raw data can be preserved
- [ ] Test records can be traced to test conditions

---

## 25. Evidence for Repository

The following evidence should be added as the GCS becomes available:

```text
09_GCS/
├── README.md
├── screenshots/
├── telemetry/
├── graphs/
└── source/
```

Useful evidence includes:

- GCS screenshot
- Live telemetry screenshot
- Temperature graph
- Communication-status screenshot
- Alert screenshot
- Recorded telemetry file
- GCS source code
- Demonstration video

Do not place placeholder screenshots in the repository and present them as working GCS evidence.

---

## 26. Implementation Status

| Component | Status |
|---|---|
| GCS Concept | Design |
| Dashboard Layout | Design |
| Telemetry Parameters | Defined |
| LoRa Telemetry | Development |
| Temperature Display | Development |
| Voltage Display | Development |
| Current Display | Development |
| Heater Status | Development |
| Thermal Status | Development |
| Alert System | Development |
| Data Logging | Planned / Development |
| Temperature Graph | Planned / Development |
| Operator Controls | Planned / Development |
| GCS Validation | Planned |

Update these statuses as actual implementation evidence becomes available.

---

## 27. Evidence Classification

Use the following terminology throughout the repository:

- **Concept** — proposed architecture or feature
- **Design** — specified but not necessarily implemented
- **Development** — implementation underway
- **Prototype** — implemented on prototype hardware
- **Tested** — tested under a documented condition
- **Validated** — supported by defined test evidence

A GCS feature should not be described as tested or validated without corresponding evidence.

---

## 28. Related Files

Thermal-control logic:

```text
04_Software/control_logic/thermal_control.md
```

Firmware:

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

Dedicated GCS repository section:

```text
09_GCS/
```
