# HIFLY — System Requirements

## 1. Purpose

The HIFLY system requirements define the functional behaviour expected from the integrated high-altitude reliability system.

The requirements are derived from the environmental challenges identified for high-altitude electrical and electronic systems and are organized around five primary functions:

- Thermal management
- Battery management
- Environmental protection
- Electrical and energy monitoring
- Communication and monitoring

---

# 2. System-Level Requirement

HIFLY shall provide an integrated system for monitoring and managing the thermal condition of a battery-powered electrical/electronic system operating under high-altitude environmental constraints.

The system combines passive thermal management, active temperature-based heating, environmental protection, electrical monitoring and wireless telemetry.

---

# 3. Thermal Management Requirements

## TH-01 — Temperature Monitoring

The system shall measure the battery temperature using an onboard temperature sensor.

The measured temperature shall be available to the onboard controller for thermal-state evaluation.

---

## TH-02 — Temperature-Based Thermal Control

The system shall use measured temperature as the primary feedback variable for controlling the heating element.

The heater shall not operate solely as an uncontrolled continuous load.

---

## TH-03 — Controlled Heating

The system shall provide an electrically controlled heating element for active battery thermal support.

The heating element shall be switched by the onboard control system through an appropriate electronic switching stage.

---

## TH-04 — Passive Thermal Management

The thermal-management architecture shall incorporate a Pulsating Heat Pipe (PHP) as a passive thermal-transfer component.

The PHP shall form part of the physical thermal path of the system.

---

## TH-05 — Thermal Insulation

The battery thermal-management assembly shall incorporate thermal insulation to reduce unwanted heat transfer between the managed thermal region and the surrounding environment.

---

## TH-06 — Thermal Feedback

The thermal-control system shall continuously use available temperature measurements to determine the current thermal condition.

The thermal-control loop shall operate as:

```text
Temperature Measurement
          ↓
Thermal Evaluation
          ↓
Heater Decision
          ↓
Heater State
          ↓
Battery Temperature
          ↓
New Measurement
```

---

# 4. Battery Management Requirements

## BT-01 — Battery Temperature

Battery temperature shall be monitored during thermal-management operation.

---

## BT-02 — Battery Electrical Monitoring

The system shall monitor the electrical parameters associated with the battery and thermal-management system.

The primary electrical measurements are:

- Voltage
- Current

---

## BT-03 — Thermal and Electrical Correlation

Temperature measurements shall be recorded together with electrical measurements where the required sensors are available.

This allows the thermal condition and electrical load to be evaluated together.

---

## BT-04 — Heater Energy Awareness

The thermal-management system shall account for the electrical load introduced by the heating element.

Measured voltage and current shall provide the basis for electrical power calculation:

```text
P = V × I
```

Where time-resolved measurements are available, the recorded power can also be used to evaluate energy consumption.

---

# 5. Environmental Protection Requirements

## EP-01 — Low-Pressure Protection

The system shall incorporate a protection chamber for selected electrical and electronic components exposed to reduced atmospheric pressure.

The chamber shall form part of the environmental-protection architecture.

---

## EP-02 — Electrical Protection

The environmental-protection architecture shall consider electrical insulation under reduced-pressure conditions.

The design shall not rely solely on normal ground-level atmospheric conditions for electrical protection.

---

## EP-03 — Thermal-Cycling Protection

The system shall incorporate flexible silicone protection at selected interfaces where flexible mechanical/environmental protection is required.

---

## EP-04 — Protective Outer Layer

The system shall incorporate a lightweight protective layer as part of the environmental-protection architecture.

The protective layer is intended to provide additional environmental protection while maintaining the overall system's mass constraints.

---

## EP-05 — Antenna Protection

The communication antenna shall include hydrophobic protection as part of the environmental-protection approach.

The antenna protection shall remain compatible with the intended LoRa communication arrangement.

---

# 6. Communication Requirements

## COM-01 — LoRa Communication

The onboard system shall provide a LoRa-based wireless communication link between the onboard controller and the Ground Control Station.

---

## COM-02 — Telemetry

The communication system shall transmit available system telemetry.

The intended telemetry parameters include:

- Battery temperature
- Ambient temperature where available
- Voltage
- Current
- Heater status
- Thermal status
- Operating mode
- Safety status
- Communication status

---

## COM-03 — Communication Status

The onboard system and GCS shall provide an indication of the communication-link condition.

The system shall distinguish between normal telemetry reception and communication loss.

---

## COM-04 — Communication-Loss Behaviour

Loss of the LoRa communication link shall not by itself terminate essential onboard thermal monitoring and control.

The onboard controller shall continue its implemented local thermal-control behaviour using available sensor measurements.

---

# 7. Ground Control Station Requirements

## GCS-01 — System Monitoring

The Ground Control Station shall provide an operator interface for observing the thermal and electrical state of the HIFLY system.

---

## GCS-02 — Temperature Display

The GCS shall display the battery temperature received from the onboard system.

Where an ambient-temperature sensor is available, the GCS shall also display the ambient temperature.

---

## GCS-03 — Electrical Display

The GCS shall display available:

- Voltage
- Current

Power may also be displayed when calculated from the measured voltage and current.

---

## GCS-04 — Heater Status

The GCS shall indicate the current heater state.

The displayed heater state shall correspond to the onboard controller's reported heater state.

---

## GCS-05 — Thermal Status

The GCS shall provide a thermal-status indication representing the implemented thermal-control condition.

---

## GCS-06 — Safety Status

The GCS shall provide an indication of the system safety state.

Relevant conditions include:

- Sensor fault
- Over-temperature
- Low-temperature condition
- Communication loss

---

## GCS-07 — Temperature Visualization

The GCS shall provide a temperature-history visualization when sufficient telemetry data is available.

The graph shall represent the actual recorded data.

---

# 8. Operating Mode Requirements

HIFLY defines the following operating modes within the thermal-control architecture.

## MODE-01 — AUTO

In AUTO mode, the onboard controller determines heater operation from the measured temperature and implemented thermal-control logic.

---

## MODE-02 — PRE-HEAT

PRE-HEAT provides a dedicated operating mode for controlled pre-heating where implemented by the firmware.

---

## MODE-03 — HEATER OFF

HEATER OFF provides a defined state in which the heating element remains disabled.

---

## MODE-04 — MANUAL

MANUAL operation may provide direct operator control of the heater where implemented.

Manual operation remains subject to onboard safety logic.

---

# 9. Safety Requirements

## SAF-01 — Sensor Fault

The system shall identify an invalid or unavailable temperature measurement as a sensor fault.

The thermal-control system shall enter its defined safe behaviour when the required temperature measurement cannot be trusted.

---

## SAF-02 — Over-Temperature

The system shall monitor the battery temperature against the configured safety condition.

When the defined over-temperature condition is reached, the heating element shall be disabled according to the implemented safety logic.

---

## SAF-03 — Safe Heater Control

The heater shall be controlled through the onboard controller and switching stage.

The system shall prevent unsafe heater operation through the implemented thermal and safety logic.

---

## SAF-04 — Local Safety

Essential thermal safety behaviour shall remain available on the onboard controller and shall not depend entirely on the Ground Control Station.

---

# 10. Data Requirements

## DATA-01 — Thermal Data

The system shall record or transmit battery temperature measurements during thermal-management operation.

---

## DATA-02 — Electrical Data

The system shall record or transmit voltage and current measurements where the corresponding monitoring hardware is available.

---

## DATA-03 — Heater-State Data

The system shall associate thermal measurements with the corresponding heater state where logging is implemented.

---

## DATA-04 — Operating-State Data

The system shall associate telemetry with the current operating mode and thermal/safety state where supported by the firmware.

---

# 11. Hardware Requirements

The integrated system architecture includes the following major hardware elements:

| Function | Hardware |
|---|---|
| Main controller | LilyGO T3-S3 / ESP32 |
| Wireless communication | LoRa |
| Battery thermal sensing | Temperature sensor |
| Electrical monitoring | Voltage/current monitoring |
| Active thermal support | Heating element |
| Heater switching | MOSFET switching stage |
| Passive thermal management | Pulsating Heat Pipe |
| Thermal protection | Thermal insulation |
| Environmental protection | Protection chamber |
| Flexible protection | Silicone protection |
| Antenna protection | Hydrophobic protection |
| Environmental protection layer | Lightweight protective layer |

The final hardware configuration is determined by the implemented prototype.

---

# 12. Software Requirements

The onboard firmware shall provide the software functions required for:

- Sensor acquisition
- Temperature monitoring
- Thermal-state evaluation
- Heater control
- Voltage monitoring
- Current monitoring
- Power calculation
- Energy monitoring
- LoRa telemetry
- Operating-mode management
- Fault handling
- Safety behaviour

---

# 13. Thermal-Control Parameters

The thermal controller uses defined temperature conditions to determine heater operation.

The final control parameters are associated with:

- Heating activation condition
- Heating deactivation condition
- Hysteresis
- Maximum safe temperature
- Sensor sampling interval
- Communication timeout where applicable

These values are part of the implemented firmware configuration and are not assumed from generic operating conditions.

---

# 14. System Response to Communication Loss

The intended system response is:

```text
LoRa Communication Lost
          ↓
Onboard Controller Detects Loss
          ↓
Local Temperature Monitoring Continues
          ↓
Thermal-Control Logic Continues
          ↓
Safety Logic Remains Active
          ↓
Communication Restored
          ↓
Telemetry Resumes
```

This architecture prevents the Ground Control Station from becoming a single point of failure for essential thermal-control behaviour.

---

# 15. Verification Structure

The HIFLY requirements are verified through multiple evidence types.

| Requirement Area | Primary Evidence |
|---|---|
| Temperature sensing | Sensor readings |
| Heater control | Firmware + heater-state data |
| PHP integration | CAD + prototype |
| Thermal behaviour | Simulation + measured temperature |
| Electrical monitoring | Voltage/current data |
| Energy monitoring | Power/energy calculations |
| Low-pressure protection | Enclosure design + pressure measurements where available |
| Communication | LoRa telemetry |
| GCS | Interface + telemetry display |
| Safety behaviour | Controlled fault testing |
| Prototype integration | CAD-to-prototype comparison |

A requirement is considered experimentally demonstrated only when corresponding evidence exists.

---

# 16. Requirement Status

The HIFLY system contains requirements at different stages of implementation.

The current development status is represented using:

| Status | Meaning |
|---|---|
| Concept | Requirement defined at architecture level |
| Design | Engineering solution developed |
| Development | Implementation in progress |
| Prototype | Physical implementation exists |
| Tested | Behaviour tested under documented conditions |
| Validated | Requirement supported by defined evidence |

The project does not treat a design description as equivalent to experimental validation.

---

# 17. Overall Requirement Summary

HIFLY brings together the following requirements into one integrated system:

```text
HIGH-ALTITUDE ENVIRONMENT
          │
          ▼
┌───────────────────────────────┐
│ Environmental Protection      │
│ Low Pressure                  │
│ Thermal Cycling               │
│ Radiation Exposure            │
│ Antenna Protection            │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ Thermal Management             │
│ PHP + Insulation + Heating    │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ Battery Monitoring             │
│ Temperature + Voltage +       │
│ Current                        │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ Onboard Control                │
│ ESP32 + Thermal Logic          │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ LoRa Communication             │
│ Telemetry + System Status      │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ Ground Control Station         │
│ Monitoring + Operator Interface│
└───────────────────────────────┘
```

These requirements define the engineering basis for the HIFLY hardware, software, CAD, simulation and testing activities.
