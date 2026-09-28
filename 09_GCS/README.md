# HIFLY — Ground Control Station

## 1. Purpose

The HIFLY Ground Control Station (GCS) provides the ground-side interface for monitoring and controlling the onboard thermal and reliability-management system.

The GCS receives telemetry from the HIFLY onboard controller through the LoRa communication link and presents the relevant system state to the operator.

The GCS is designed around the principle that essential thermal-management and safety functions remain available onboard even when the communication link is unavailable.

---

# 2. GCS Role in HIFLY

The GCS provides:

- telemetry monitoring
- thermal-state monitoring
- electrical-state monitoring
- heater-state monitoring
- communication-status monitoring
- safety-status monitoring
- control-mode indication
- supported thermal-control commands
- temperature visualization
- alert indication

The GCS is therefore a monitoring and supervisory interface rather than the sole source of thermal-control logic.

---

# 3. GCS Architecture

```text
                         HIFLY AIRBORNE SYSTEM
                                  │
        ┌─────────────────────────┼─────────────────────────┐
        │                         │                         │
        ▼                         ▼                         ▼
 Temperature Sensors         Voltage Sensor           Current Sensor
        │                         │                         │
        └─────────────────────────┼─────────────────────────┘
                                  ▼
                                ESP32
                                  │
                    ┌─────────────┼─────────────┐
                    │             │             │
                    ▼             ▼             ▼
               Thermal Control  Heater       Safety Logic
                    │                           │
                    └─────────────┬─────────────┘
                                  │
                                  ▼
                              LilyGO T3-S3
                                  │
                                  │ LoRa
                                  ▼
                           Ground Receiver
                                  │
                                  ▼
                                 GCS
                                  │
              ┌───────────────────┼───────────────────┐
              │                   │                   │
              ▼                   ▼                   ▼
           Telemetry           Graphs              Alerts
              │                   │                   │
              └───────────────────┼───────────────────┘
                                  ▼
                           Operator Interface
```
4. Airborne and Ground Responsibilities

The HIFLY architecture separates onboard functions from ground-station functions.

Function	Airborne system	GCS
Temperature sensing	Yes	Display
Voltage sensing	Yes	Display
Current sensing	Yes	Display
Thermal control	Yes	Supervisory control
Heater switching	Yes	Command where supported
Safety logic	Yes	Status display
Communication monitoring	Yes	Yes
Telemetry transmission	Yes	Reception
Temperature graph	Data source	Display
Communication-loss response	Yes	Status indication

The onboard controller remains responsible for autonomous operation when communication with the GCS is unavailable.

5. Telemetry

Telemetry provides the GCS with information about the current onboard state.

The primary telemetry fields include:

battery temperature
ambient temperature
voltage
current
heater status
thermal status
LoRa link status
AUTO/MANUAL state
safety status

The exact packet structure depends on the implemented firmware.

6. Telemetry Flow
Temperature
     │
     ├──────────────┐
     │              │
Voltage              │
     │              │
Current              │
     │              │
Heater State         │
     │              │
Safety State         │
     │              │
Control State        │
     │              │
     └──────┬───────┘
            ▼
          ESP32
            │
            ▼
       Telemetry Packet
            │
            ▼
       LilyGO T3-S3
            │
            │ LoRa
            ▼
       Ground Receiver
            │
            ▼
            GCS
7. Temperature Monitoring

Temperature is one of the primary variables displayed by the GCS.

Relevant temperature channels can include:

battery temperature
ambient temperature
electronics temperature
thermal-management temperature
other defined sensor locations

The GCS should identify the measurement channel clearly so that different physical locations are not confused.

8. Temperature Graph

The GCS provides a temperature-visualization function.

Conceptually:

Temperature
    │
    │             ●
    │          ●     ●
    │       ●           ●
    │    ●
    │ ●
    └──────────────────────── Time

A time-based temperature graph requires timestamped telemetry or a known sampling interval.

If the source data does not contain time information, a graph should use measurement index rather than representing the horizontal axis as physical time.

9. Battery Monitoring

The GCS provides visibility into the battery thermal and electrical state.

Relevant displayed parameters include:

┌──────────────────────────────┐
│        BATTERY STATUS        │
├──────────────────────────────┤
│ Temperature :  --            │
│ Voltage     :  --            │
│ Current     :  --            │
│ Heater      :  --            │
│ Thermal     :  --            │
└──────────────────────────────┘

The values shown by the interface originate from onboard measurements and firmware processing.

10. Electrical Monitoring

The GCS displays the electrical operating condition of the system.

Primary parameters are:

voltage
current
instantaneous electrical power where calculated
heater state

Instantaneous electrical power is calculated from:

$$ P = VI $$

For example, the available measurement:

$$ V = 12.0\text{ V} $$ $$ I = 1.54\text{ A} $$

corresponds to:

$$ P = 18.48\text{ W} $$

This represents an instantaneous power value rather than total mission energy.

11. Heater Status

The GCS displays the current heater state.

Example state representation:

HEATER
────────────────
OFF

or:

HEATER
────────────────
ON

The displayed heater state should correspond to the actual onboard control state.

12. Thermal Status

The GCS provides a thermal-status indicator summarizing the current thermal-control state.

Conceptually:

┌──────────────────────────────┐
│       THERMAL STATUS         │
├──────────────────────────────┤
│ Monitoring                   │
│ Heater: OFF                  │
│ Control: AUTO                │
└──────────────────────────────┘

The exact status labels depend on the firmware and GCS implementation.

13. AUTO Mode

AUTO mode allows the onboard thermal-control system to operate according to its programmed control logic.

The conceptual path is:

Temperature
     │
     ▼
Onboard Sensor
     │
     ▼
Control Logic
     │
     ▼
Heater Decision
     │
     ▼
Heater

The GCS displays the AUTO state and may provide the corresponding mode-selection command where implemented.

14. PRE-HEAT Mode

PRE-HEAT is a supported control state for initiating the intended pre-heating operation where implemented.

Conceptually:

GCS
 │
 ▼
PRE-HEAT Command
 │
 ▼
Onboard Controller
 │
 ▼
Thermal Control
 │
 ▼
Heater
 │
 ▼
Temperature Response

The actual behaviour depends on the implemented firmware logic and defined operating conditions.

15. HEATER OFF

The GCS may provide a heater-off control state.

GCS
 │
 ▼
HEATER OFF
 │
 ▼
Onboard Controller
 │
 ▼
Heater Disabled

This command controls the heater state where manual control is supported.

Safety logic remains an onboard responsibility.

16. Manual Override

The GCS architecture may support manual override for thermal-control operation.

The conceptual control hierarchy is:

                Control Mode
                     │
            ┌────────┴────────┐
            │                 │
            ▼                 ▼
           AUTO             MANUAL
            │                 │
            ▼                 ▼
     Control Algorithm    Operator Command
            │                 │
            └────────┬────────┘
                     ▼
                  Heater

Manual override should remain subject to the safety logic implemented onboard.

17. Safety Status

The GCS displays safety-related system information.

Potential states include:

normal
sensor fault
communication lost
thermal warning
heater state
safety-control state

The GCS displays the safety condition received from the onboard controller.

Critical safety decisions should not depend exclusively on continuous GCS availability.

18. Alerts

The GCS architecture includes alert states for important system conditions.

Primary alerts include:

Low Temp
Over-temperature
Sensor Fault
Communication Lost

Conceptual representation:

┌──────────────────────────────────┐
│             ALERTS               │
├──────────────────────────────────┤
│ ✓ Low Temp                       │
│ ✓ Over-temperature               │
│ ✓ Sensor Fault                   │
│ ✓ Communication Lost             │
└──────────────────────────────────┘

Only active conditions should be presented as active alerts.

19. Communication Status

The GCS displays the LoRa communication state.

Conceptually:

┌──────────────────────────────┐
│       COMMUNICATION          │
├──────────────────────────────┤
│ Link: CONNECTED              │
│ Protocol: LoRa               │
└──────────────────────────────┘

When communication is unavailable:

┌──────────────────────────────┐
│       COMMUNICATION          │
├──────────────────────────────┤
│ Link: LOST                   │
│ Mode: ONBOARD AUTONOMOUS     │
└──────────────────────────────┘

The exact displayed terminology depends on the implemented GCS.

20. Communication-Loss Behaviour

Communication loss is an important HIFLY reliability condition.

The intended system relationship is:

                 LoRa Link
                    │
              ┌─────┴─────┐
              │           │
          Available       Lost
              │           │
              ▼           ▼
         Normal GCS    Onboard
          operation    autonomous
              │        control
              │           │
              └─────┬─────┘
                    ▼
                Safety Logic
                    │
                    ▼
             Thermal Control

The onboard thermal-control system is not dependent on continuous GCS communication for basic autonomous operation.

21. Communication Recovery

When the LoRa link becomes available again, the GCS can resume telemetry reception.

Conceptually:

Communication Lost
        │
        ▼
Onboard Autonomous Control
        │
        ▼
Communication Restored
        │
        ▼
Telemetry Reception
        │
        ▼
GCS State Update

The recovery process should preserve the current onboard safety state.

22. GCS User Interface

A conceptual HIFLY GCS layout is:

┌─────────────────────────────────────────────────────────┐
│                    HIFLY GROUND CONTROL                 │
├───────────────────────┬─────────────────────────────────┤
│ SYSTEM STATUS         │ TEMPERATURE                     │
│                       │                                 │
│ Thermal: --           │              Temperature        │
│ Safety: --            │                  ▲              │
│ Control: AUTO         │                  │              │
│ Heater: OFF           │              ────┼────          │
│ LoRa: CONNECTED       │                  │              │
│                       │                  └──────── Time  │
├───────────────────────┼─────────────────────────────────┤
│ BATTERY               │ ALERTS                          │
│                       │                                 │
│ Temp: --              │ Low Temp:        --             │
│ Voltage: --           │ Over-temp:       --             │
│ Current: --           │ Sensor Fault:    --             │
│ Power: --             │ Communication:   --             │
├───────────────────────┴─────────────────────────────────┤
│ CONTROL                                                 │
│                                                         │
│ [ AUTO ]   [ PRE-HEAT ]   [ HEATER OFF ]               │
│                                                         │
│                 [ MANUAL OVERRIDE ]                     │
└─────────────────────────────────────────────────────────┘

This represents the intended information architecture rather than a claim about the final software interface.

23. Telemetry Packet Concept

A telemetry message can conceptually contain:

{
    temperature,
    ambient_temperature,
    voltage,
    current,
    heater_status,
    thermal_status,
    control_mode,
    safety_status,
    communication_status
}

The exact serialization, packet format, field names, and data types depend on the implemented firmware and communication software.

24. Data Flow From Sensor to GCS
Physical Condition
       │
       ▼
Sensor
       │
       ▼
Analog / Digital Measurement
       │
       ▼
ESP32 Firmware
       │
       ├──────────────► Local Thermal Control
       │
       ▼
Telemetry Formatting
       │
       ▼
LilyGO T3-S3
       │
       ▼
LoRa Transmission
       │
       ▼
Ground Receiver
       │
       ▼
GCS Software
       │
       ▼
Operator Display
25. Onboard Control Priority

The HIFLY architecture gives priority to onboard control for essential thermal and safety functions.

             OPERATOR / GCS
                    │
                    │ supervisory command
                    ▼
             ONBOARD CONTROLLER
                    │
          ┌─────────┼─────────┐
          │         │         │
          ▼         ▼         ▼
       Sensors    Control   Safety
                    │         │
                    └────┬────┘
                         ▼
                       Heater

This architecture prevents the GCS from becoming a single point of failure for basic thermal management.

26. GCS and Fail-Safe Architecture

The fail-safe relationship can be represented as:

                NORMAL STATE
                     │
                     ▼
                LoRa Active
                     │
                     ▼
              GCS Monitoring
                     │
                     ▼
              Onboard Control
                     │
                     │
              Communication Lost
                     │
                     ▼
             GCS unavailable
                     │
                     ▼
          Onboard autonomous mode
                     │
                     ▼
               Safety logic
                     │
                     ▼
             Thermal control
                     │
                     │
              Link restored
                     │
                     ▼
              GCS reconnects

The onboard controller remains responsible for maintaining the defined autonomous behaviour.

27. GCS Monitoring of the Seven Subproblems

The GCS provides visibility into several of the HIFLY reliability challenges.

HIFLY challenge	GCS-related information
Reduced cooling efficiency	Thermal measurements
Insulation breakdown / arcing	Safety and electrical state
Battery degradation	Battery temperature and electrical state
Thermal cycling damage	Temperature history where timestamped data exists
Radiation exposure	Environmental/test status where instrumented
Communication effects	LoRa link state
Energy and endurance	Voltage, current, and power monitoring

The GCS provides monitoring information; physical qualification of these functions requires corresponding tests.

28. GCS and Data Logging

Where logging is implemented, the GCS can provide a record of received telemetry.

A logged record may contain:

Timestamp
Temperature
Ambient Temperature
Voltage
Current
Power
Heater State
Thermal State
Control Mode
Safety State
Communication State

The actual fields depend on the final telemetry implementation.

29. Timestamped Telemetry

Timestamped telemetry is important for analysing:

temperature rise
temperature fall
heater response
thermal settling
communication events
control transitions
power consumption
energy usage

Without timestamps, sequential telemetry can establish ordering but not physical elapsed time.

30. GCS Alerts and Safety

Alerts provide operator awareness of system conditions.

The alert path is:

Onboard Measurement
        │
        ▼
Onboard Evaluation
        │
        ▼
System State
        │
        ▼
Telemetry
        │
        ▼
GCS
        │
        ▼
Alert Display

The GCS alert should reflect the onboard state received through telemetry.

31. GCS Control Path

Commands from the GCS follow a controlled path.

Operator
   │
   ▼
GCS Command
   │
   ▼
LoRa Transmission
   │
   ▼
LilyGO T3-S3
   │
   ▼
ESP32
   │
   ▼
Command Validation
   │
   ▼
Onboard Control Logic
   │
   ▼
Actuator / System State

Command validation occurs onboard before the requested action affects the system.

32. GCS Command and Safety Separation

The GCS may request a control action, but the onboard controller remains responsible for applying the command within the defined safety architecture.

GCS Request
     │
     ▼
Communication
     │
     ▼
Onboard Command Validation
     │
 ┌───┴────┐
 │        │
 ▼        ▼
Allowed  Rejected
 │
 ▼
Control Action

This prevents a communication or operator-interface fault from automatically bypassing onboard safety logic.

33. GCS Testing

The GCS itself should be tested as a subsystem.

Testing can verify:

telemetry reception
value display
graph operation
alert display
control commands
communication-loss indication
communication recovery
onboard/GCS state consistency

A GCS test does not replace the corresponding onboard hardware test.

34. GCS and Simulation

The GCS may be used to display data generated during simulation or test-data playback when such functionality is implemented.

The distinction should remain visible:

SIMULATION DATA ──► GCS DISPLAY

EXPERIMENTAL DATA ─► GCS DISPLAY

LIVE TELEMETRY ────► GCS DISPLAY

The source of the displayed data should be known so that simulated values are not mistaken for live measurements.

35. GCS and Physical Testing

During physical testing, the GCS provides a remote view of the onboard state.

             TEST HARDWARE
                  │
                  ▼
               Sensors
                  │
                  ▼
                ESP32
                  │
                  ▼
                 LoRa
                  │
                  ▼
                 GCS
                  │
       ┌──────────┼──────────┐
       ▼          ▼          ▼
   Temperature  Electrical  Safety

The GCS can therefore support test observation while the actual measurements remain associated with their onboard sensors and acquisition system.

36. Communication Reliability

The GCS communication architecture is designed to identify link availability.

Important states include:

CONNECTED
    │
    ▼
TELEMETRY ACTIVE
    │
    ▼
COMMUNICATION LOST
    │
    ▼
ONBOARD AUTONOMOUS MODE
    │
    ▼
COMMUNICATION RESTORED

A communication-status indicator should distinguish loss of telemetry from loss of onboard control.

37. GCS Data Integrity

The GCS should preserve the identity of received data.

Relevant information includes:

source device
telemetry packet
data field
received state
timestamp where available
communication state

Data should not be silently interpreted as valid if the communication system reports an invalid or incomplete packet.

38. GCS Architecture and HIFLY Reliability

The GCS contributes to HIFLY reliability by providing:

operator awareness
telemetry visibility
thermal-state observation
electrical-state observation
communication-state indication
controlled supervisory commands
alerts

The GCS does not replace:

onboard sensing
onboard thermal control
onboard safety logic
local fail-safe operation
39. Overall GCS Architecture
                         HIFLY AIRBORNE SYSTEM
                                  │
          ┌───────────────────────┼───────────────────────┐
          │                       │                       │
          ▼                       ▼                       ▼
    Temperature              Electrical              Control
      Sensors                Monitoring               Logic
          │                       │                       │
          └───────────────────────┼───────────────────────┘
                                  ▼
                                ESP32
                                  │
                 ┌────────────────┼────────────────┐
                 │                │                │
                 ▼                ▼                ▼
             Telemetry         Heater           Safety
                 │
                 ▼
            LilyGO T3-S3
                 │
                 │ LoRa
                 ▼
          Ground Communication
                 │
                 ▼
                 GCS
                 │
      ┌──────────┼──────────┐
      │          │          │
      ▼          ▼          ▼
   Monitor     Alerts     Controls
      │          │          │
      └──────────┼──────────┘
                 ▼
              Operator
40. Status

GCS Status: Architecture Defined

The HIFLY GCS architecture defines the monitoring, telemetry, alert, communication-status, and supervisory-control functions required by the integrated reliability system.

The onboard controller remains responsible for autonomous thermal management and safety behaviour when the ground communication link is unavailable.
