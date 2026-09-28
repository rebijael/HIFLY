# HIFLY Ground Control Station (GCS)

## 1. Overview

The HIFLY Ground Control Station (GCS) is the ground-side monitoring and supervisory interface for the HIFLY system.

The GCS receives telemetry from the onboard controller through the LilyGO T3-S3 LoRa communication system and presents the thermal, electrical, communication, and safety condition of the system to the operator.

The GCS also provides permitted supervisory commands for the onboard thermal-control system.

The GCS is therefore designed around two complementary functions:

1. **Monitoring** — displaying the current condition of HIFLY.
2. **Supervision** — providing controlled operator commands while the essential thermal and safety logic remains onboard.

```text
                         HIFLY ONBOARD SYSTEM
                                  │
                                  ▼
                         ┌─────────────────┐
                         │ Sensors / MCU   │
                         └────────┬────────┘
                                  │
                                  ▼
                           Telemetry Data
                                  │
                                  ▼
                         ┌─────────────────┐
                         │ LilyGO T3-S3    │
                         │      LoRa       │
                         └────────┬────────┘
                                  │
                               LoRa Link
                                  │
                                  ▼
                         ┌─────────────────┐
                         │      GCS        │
                         └────────┬────────┘
                                  │
              ┌───────────────────┼───────────────────┐
              │                   │                   │
              ▼                   ▼                   ▼
          Monitoring           Graphs              Alerts
              │                   │                   │
              └───────────────────┼───────────────────┘
                                  │
                                  ▼
                         Operator Controls
                                  │
                                  ▼
                              LoRa
                                  │
                                  ▼
                           HIFLY Controller
```
2. GCS Objectives

The GCS is designed to provide a clear representation of the HIFLY system state.

The main objectives are:

display battery temperature
display ambient temperature
display voltage
display current
display heater status
display thermal status
display LoRa communication status
display AUTO/MANUAL operating mode
display safety status
provide temperature graphs
provide system alerts
provide permitted thermal-control commands

The interface connects the physical HIFLY system with the operator without making the GCS the sole controller of essential thermal functions.

3. GCS Architecture
┌──────────────────────────────────────────────────────────────┐
│                    HIFLY GROUND CONTROL STATION              │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌────────────────────────────────────────────────────────┐  │
│  │                    TELEMETRY LAYER                     │  │
│  │                                                        │  │
│  │ Temperature • Voltage • Current • Heater • Safety     │  │
│  └───────────────────────────┬────────────────────────────┘  │
│                              │                               │
│                              ▼                               │
│  ┌────────────────────────────────────────────────────────┐  │
│  │                 DATA PROCESSING                        │  │
│  │                                                        │  │
│  │ State Detection • Communication Status • Alerts       │  │
│  └───────────────────────────┬────────────────────────────┘  │
│                              │                               │
│              ┌───────────────┼───────────────┐               │
│              ▼               ▼               ▼               │
│       ┌─────────────┐ ┌─────────────┐ ┌─────────────┐       │
│       │   STATUS    │ │   GRAPHS    │ │   ALERTS    │       │
│       │   DISPLAY   │ │             │ │             │       │
│       └─────────────┘ └─────────────┘ └─────────────┘       │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐  │
│  │                  CONTROL INTERFACE                     │  │
│  │                                                        │  │
│  │        AUTO • PRE-HEAT • HEATER OFF                   │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                              │
└──────────────────────────────┬───────────────────────────────┘
                               │
                               ▼
                             LoRa
                               │
                               ▼
                       HIFLY ONBOARD MCU
4. Telemetry

The GCS receives telemetry generated by the onboard firmware.

The primary telemetry fields are:

Telemetry Field	GCS Representation
Battery Temperature	Numeric value
Ambient Temperature	Numeric value
Voltage	Numeric value
Current	Numeric value
Heater Status	ON / OFF
Thermal Status	Current thermal state
LoRa Link	Communication state
Operating Mode	AUTO / MANUAL
Safety Status	Current safety state

These values allow the operator to observe the relationship between thermal behavior, electrical operation, heater activity, and communication status.

5. Main GCS Dashboard

The primary GCS dashboard is organized around the current system condition.

┌─────────────────────────────────────────────────────────────────┐
│                         HIFLY GCS                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  BATTERY TEMP     AMBIENT TEMP       VOLTAGE       CURRENT      │
│  ┌────────────┐   ┌────────────┐   ┌──────────┐  ┌──────────┐ │
│  │            │   │            │   │          │  │          │ │
│  │   TEMP     │   │   TEMP     │   │  VOLTAGE │  │ CURRENT  │ │
│  │            │   │            │   │          │  │          │ │
│  └────────────┘   └────────────┘   └──────────┘  └──────────┘ │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  HEATER STATUS       THERMAL STATUS       LoRa LINK             │
│  ┌──────────────┐   ┌──────────────┐   ┌──────────────┐       │
│  │              │   │              │   │              │       │
│  │   ON / OFF   │   │    STATE     │   │   CONNECTED  │       │
│  │              │   │              │   │   / LOST     │       │
│  └──────────────┘   └──────────────┘   └──────────────┘       │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  OPERATING MODE              SAFETY STATUS                      │
│  ┌───────────────────┐       ┌──────────────────────────────┐  │
│  │ AUTO / MANUAL     │       │                              │  │
│  └───────────────────┘       │          SAFETY STATE        │  │
│                              │                              │  │
│                              └──────────────────────────────┘  │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                       TEMPERATURE GRAPH                         │
│                                                                 │
│             Temperature vs. Time / Reading Index                │
│                                                                 │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ ALERTS                                                          │
│                                                                 │
│  • Low Temperature                                              │
│  • Over-temperature                                             │
│  • Sensor Fault                                                 │
│  • Communication Lost                                           │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ CONTROLS                                                        │
│                                                                 │
│       [ AUTO ]       [ PRE-HEAT ]       [ HEATER OFF ]          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
6. Battery Temperature Display

The battery-temperature field provides the primary thermal measurement for the battery thermal-management system.

The GCS displays the received value so the operator can observe:

current battery temperature
changes in temperature
thermal response during heater operation
thermal condition during different operating states

The value is received from the onboard sensing and firmware layer.

Battery
   │
   ▼
Temperature Sensor
   │
   ▼
MCU
   │
   ▼
LoRa Telemetry
   │
   ▼
GCS
   │
   ▼
Battery Temperature Display
7. Ambient Temperature Display

Ambient temperature provides environmental context for the battery temperature.

The relationship is:

Ambient Temperature
        │
        ▼
       GCS
        │
        ├──────────────► Environmental Reference
        │
        ▼
Battery Temperature
        │
        ▼
Thermal Condition

Displaying both measurements allows the operator to distinguish the battery thermal state from the surrounding environmental condition.

8. Voltage Display

The GCS displays the measured system voltage received through telemetry.

Voltage Sensor
      │
      ▼
Onboard MCU
      │
      ▼
LoRa Telemetry
      │
      ▼
GCS Voltage Display

Voltage monitoring provides visibility into the electrical state of the system during thermal operation.

9. Current Display

The GCS displays the measured current received from the onboard controller.

Current Sensor
      │
      ▼
Onboard MCU
      │
      ▼
LoRa Telemetry
      │
      ▼
GCS Current Display

Current information is particularly relevant when the heating element is active because heater operation contributes to the electrical load.

10. Heater Status

The GCS provides a direct indication of the current heater state.

The primary states are:

HEATER ON
HEATER OFF

The displayed heater state is based on the onboard control state rather than only the command sent from the GCS.

Thermal Control
      │
      ▼
Heater State
      │
      ├──────────────► Heater ON
      │
      └──────────────► Heater OFF
                         │
                         ▼
                     Telemetry
                         │
                         ▼
                         GCS

This allows the operator to observe the actual reported control state.

11. Thermal Status

The thermal-status field provides a software-level representation of the current thermal condition.

Representative states are:

NORMAL
HEATING
SAFETY / FAULT

The GCS receives this state from the onboard firmware.

Temperature
     │
     ▼
Thermal Evaluation
     │
     ▼
Thermal State
     │
     ▼
Telemetry
     │
     ▼
GCS Thermal Status

The exact numerical boundaries between states are determined by the implemented thermal-control configuration.

12. LoRa Link Status

The GCS displays the status of the communication link between the ground station and the onboard HIFLY system.

The basic communication states are:

LINK AVAILABLE
LINK LOST

The communication path is:

HIFLY MCU
    │
    ▼
LilyGO T3-S3
    │
    ▼
   LoRa
    │
    ▼
GCS Receiver
    │
    ▼
Link Status

A communication-loss condition is displayed as an alert while the onboard thermal-control system remains capable of local operation.

13. Operating Mode

The GCS displays the current operating mode.

The primary modes are:

AUTO
MANUAL
AUTO

In AUTO mode, the onboard controller manages the heater using the temperature-based thermal-control logic.

MANUAL

In MANUAL mode, the operator can issue permitted thermal-control commands through the GCS.

The onboard safety logic remains active in both modes.

14. Safety Status

The safety-status field provides an overall indication of whether the onboard firmware has detected a safety-related condition.

Representative states are:

NORMAL
FAULT

The GCS receives this information through telemetry.

Sensors
  │
  ▼
Firmware
  │
  ▼
Safety Evaluation
  │
  ├──────────► NORMAL
  │
  └──────────► FAULT
                  │
                  ▼
               Telemetry
                  │
                  ▼
                 GCS
15. Temperature Graph

The temperature graph provides a visual representation of thermal behavior.

The graph can represent:

battery temperature
ambient temperature
temperature change during heating
temperature response after heater operation
thermal behavior during experimental operation

A conceptual graph structure is:

Temperature
    │
    │             ●
    │          ●     ●
    │       ●           ●
    │    ●
    │ ●
    └──────────────────────────────►
       Time / Reading Index

Where timestamped data is available, the horizontal axis can represent time.

Where timestamps are not available, the available HIFLY sample data is represented by reading index rather than an invented time scale.

16. Temperature Data Display

The GCS temperature graph is connected to the same telemetry stream used by the numerical temperature display.

                    Temperature Sensor
                           │
                           ▼
                     Onboard Firmware
                           │
                           ▼
                     Telemetry Packet
                           │
                           ▼
                          LoRa
                           │
                           ▼
                           GCS
                      ┌────┴────┐
                      │         │
                      ▼         ▼
                Numeric Value   Graph

This ensures that the numerical display and graph originate from the same telemetry data source.

17. GCS Alerts

The GCS provides operator-facing alerts for important system conditions.

The primary alert categories are:

┌──────────────────────────────────────┐
│              HIFLY ALERTS            │
├──────────────────────────────────────┤
│                                      │
│  LOW TEMPERATURE                     │
│                                      │
│  OVER-TEMPERATURE                    │
│                                      │
│  SENSOR FAULT                        │
│                                      │
│  COMMUNICATION LOST                  │
│                                      │
└──────────────────────────────────────┘

The alerts are generated from onboard telemetry and status information.

18. Low-Temperature Alert

The low-temperature alert indicates that the measured thermal condition requires attention according to the configured thermal-control logic.

Temperature
     │
     ▼
Thermal Evaluation
     │
     ▼
Low-Temperature Condition
     │
     ├──────────────► Thermal Control
     │
     └──────────────► GCS Alert

The alert does not replace the onboard thermal-control decision.

19. Over-Temperature Alert

An over-temperature condition is treated as a safety-related event.

Temperature
     │
     ▼
Thermal Evaluation
     │
     ▼
Over-temperature Condition
     │
     ├──────────────► Safety Logic
     │
     └──────────────► GCS Alert

The GCS communicates the condition to the operator while the onboard controller handles the local safety response.

20. Sensor-Fault Alert

A sensor-fault alert indicates that a monitored measurement cannot be treated as valid according to the onboard firmware's data-validation logic.

Sensor
  │
  ▼
Measurement
  │
  ▼
Validation
  │
  ▼
Invalid
  │
  ├──────────────► Onboard Safety Logic
  │
  └──────────────► GCS SENSOR FAULT

This alert is important because the thermal-control system depends on valid temperature measurements.

21. Communication-Lost Alert

A communication-lost alert is generated when the GCS no longer receives the expected communication from the onboard system according to the implemented link-monitoring logic.

LoRa Link
   │
   ▼
Communication Monitor
   │
   ▼
No Valid Link
   │
   ├──────────────► GCS COMMUNICATION LOST
   │
   └──────────────► Onboard Autonomous Control

The onboard thermal-control system continues operating independently of the GCS display.

22. Control Interface

The GCS provides three primary operator controls:

┌──────────────┐
│     AUTO     │
└──────────────┘

┌──────────────┐
│   PRE-HEAT   │
└──────────────┘

┌──────────────┐
│  HEATER OFF  │
└──────────────┘

These commands are transmitted to the onboard controller through LoRa.

The onboard firmware validates and processes the commands before affecting the physical heater.

23. AUTO Button

The AUTO control requests automatic temperature-based thermal control.

Operator
   │
   ▼
AUTO
   │
   ▼
GCS
   │
   ▼
LoRa
   │
   ▼
Onboard Controller
   │
   ▼
AUTO Mode
   │
   ▼
Temperature-Based Control

Once AUTO mode is active, the onboard controller performs the thermal-control loop.

24. PRE-HEAT Button

The PRE-HEAT control requests the configured pre-heating function.

Operator
   │
   ▼
PRE-HEAT
   │
   ▼
GCS
   │
   ▼
LoRa
   │
   ▼
Onboard Controller
   │
   ▼
Command Validation
   │
   ▼
Thermal / Safety Logic
   │
   ▼
Heater

The onboard controller remains responsible for applying the command within the active safety logic.

25. HEATER OFF Button

The HEATER OFF control requests that active heating be disabled.

Operator
   │
   ▼
HEATER OFF
   │
   ▼
GCS
   │
   ▼
LoRa
   │
   ▼
Onboard Controller
   │
   ▼
Command Validation
   │
   ▼
Heater OFF

Temperature monitoring continues after the heater is disabled.

26. Command Validation

The GCS is not directly connected to the heater.

All commands follow the communication and onboard-validation path.

GCS Command
     │
     ▼
LoRa Packet
     │
     ▼
Onboard Receiver
     │
     ▼
Command Validation
     │
     ▼
Safety Evaluation
     │
     ▼
Control Action

This architecture keeps safety decisions within the onboard system.

27. Telemetry Packet Concept

The GCS can receive a telemetry packet containing the following logical fields:

HIFLY TELEMETRY
├── Battery Temperature
├── Ambient Temperature
├── Voltage
├── Current
├── Heater Status
├── Thermal Status
├── Operating Mode
├── Safety Status
└── LoRa / Communication Status

The actual packet encoding depends on the implemented firmware and communication interface.

28. GCS Data Flow
              HIFLY ONBOARD SYSTEM
                       │
                       ▼
                    Sensors
                       │
                       ▼
                  MCU Firmware
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
       Thermal       Power        Safety
       Status        Data          State
          │            │            │
          └────────────┼────────────┘
                       │
                       ▼
                   Telemetry
                       │
                       ▼
                  LilyGO T3-S3
                       │
                       ▼
                      LoRa
                       │
                       ▼
                     GCS
                       │
       ┌───────────────┼───────────────┐
       ▼               ▼               ▼
    Display          Graphs           Alerts
       │               │               │
       └───────────────┼───────────────┘
                       │
                       ▼
                  Operator
                       │
                       ▼
                   Commands
                       │
                       ▼
                     LoRa
                       │
                       ▼
                 HIFLY Firmware
29. GCS and Autonomous Operation

The GCS is a supervisory interface rather than the sole source of thermal-control decisions.

The system hierarchy is:

                 HIFLY ONBOARD
                       │
            ┌──────────┼──────────┐
            │          │          │
            ▼          ▼          ▼
         Sensors    Thermal     Safety
                    Control      Logic
            │          │          │
            └──────────┼──────────┘
                       │
                       ▼
                    LoRa
                       │
                       ▼
                      GCS
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
       Monitor       Alert        Command

Essential thermal control remains onboard.

30. Communication-Loss Behavior

When communication is lost, the GCS cannot display current telemetry until the link is restored.

The onboard controller continues local sensing and thermal-control operation.

                 NORMAL LINK
                     │
                     ▼
             Telemetry Received
                     │
                     ▼
                    GCS
                     │
                     │
                Link Lost
                     │
                     ▼
            GCS Communication
                 Lost Alert
                     │
                     │
             ┌───────┴───────┐
             │               │
             ▼               ▼
        GCS Waiting      Onboard System
        for Recovery     Continues Local
                         Thermal Control
             │               │
             └───────┬───────┘
                     │
              Link Restored
                     │
                     ▼
             Telemetry Resumes
31. GCS Thermal Monitoring During Testing

The GCS can be used as the operator-facing interface during controlled thermal experiments.

The monitored relationship is:

Thermal Input
     │
     ▼
Heater State
     │
     ▼
Temperature Response
     │
     ▼
Voltage / Current
     │
     ▼
LoRa Telemetry
     │
     ▼
GCS
     │
     ├── Temperature
     ├── Heater Status
     ├── Voltage
     ├── Current
     ├── Thermal Status
     └── Alerts

This provides a common interface for observing thermal and electrical behavior during experiments.

32. GCS and Experimental Data

The available HIFLY sample data contains current, voltage, and temperature measurements.

The supplied readings are:

Reading Index	Current	Voltage	Temperature
1	0.00	0.0	16
2	0.60	5.3	17
3	0.80	7.6	21
4	1.26	10.1	25
5	1.54	12.0	28
6	1.23	10.1	33
7	0.86	7.1	34
8	0.60	5.3	35
9	0.00	0.0	36

The dataset does not contain timestamps.

Therefore, if this data is plotted in the GCS/data-analysis layer without additional timing information, the horizontal axis should be identified as Reading Index rather than time.

33. GCS Power Information

Where voltage and current are available in the same telemetry record, the GCS or data-processing layer can derive electrical power using:

Power = Voltage × Current

For example, the recorded pair:

Voltage = 12.0
Current = 1.54

corresponds to an instantaneous calculated electrical power of:

Power = 12.0 × 1.54
      = 18.48

The calculated value represents the product of the recorded voltage and current at that reading. It should not be interpreted as total energy consumption without time information.

34. GCS State Relationships

The dashboard combines multiple system states into one operator view.

                 HIFLY STATE
                     │
      ┌──────────────┼──────────────┐
      │              │              │
      ▼              ▼              ▼
   Thermal         Electrical    Communication
      │              │              │
      ▼              ▼              ▼
 Battery Temp     Voltage        LoRa Link
 Ambient Temp     Current
 Heater State     Power
      │              │              │
      └──────────────┼──────────────┘
                     │
                     ▼
                   GCS
                     │
          ┌──────────┼──────────┐
          ▼          ▼          ▼
       Display     Graphs     Alerts

This provides a unified view of the system rather than treating thermal and electrical behavior as isolated measurements.

35. GCS Safety Relationship

The GCS displays safety information but does not replace the onboard safety system.

             ONBOARD SAFETY LOGIC
                      │
             ┌────────┼────────┐
             │        │        │
             ▼        ▼        ▼
          Thermal   Sensor   Communication
           Fault     Fault      Fault
             │        │        │
             └────────┼────────┘
                      │
                      ▼
                  Telemetry
                      │
                      ▼
                     GCS
                      │
                      ▼
                   Alert

The GCS therefore acts as the operator notification layer while the onboard controller remains responsible for local fault handling.

36. GCS Interface With Thermal Control

The complete relationship between the GCS and thermal-control system is:

                     OPERATOR
                         │
                         ▼
                       GCS
                         │
              ┌──────────┼──────────┐
              │          │          │
              ▼          ▼          ▼
             AUTO     PRE-HEAT   HEATER OFF
              │          │          │
              └──────────┼──────────┘
                         │
                         ▼
                       LoRa
                         │
                         ▼
                 Onboard Controller
                         │
                         ▼
                  Command Validation
                         │
                         ▼
                   Safety Evaluation
                         │
                         ▼
                  Thermal-Control Logic
                         │
                         ▼
                       MOSFET
                         │
                         ▼
                       Heater
                         │
                         ▼
                     Temperature
                         │
                         ▼
                      Telemetry
                         │
                         ▼
                        LoRa
                         │
                         ▼
                         GCS

This creates a complete bidirectional monitoring and supervisory path.

37. GCS Interface With Power Monitoring

The electrical-monitoring path is:

Voltage Sensor ───────┐
                      │
                      ▼
                   MCU
                      │
Current Sensor ───────┤
                      │
                      ▼
                 Telemetry
                      │
                      ▼
                     LoRa
                      │
                      ▼
                     GCS
                      │
             ┌────────┼────────┐
             ▼        ▼        ▼
          Voltage   Current   Power

The GCS can therefore associate electrical measurements with the thermal state and heater state.

38. GCS Interface With Experimental Validation

The GCS provides a practical interface between the HIFLY prototype and recorded system evidence.

Prototype
   │
   ▼
Sensors
   │
   ▼
Firmware
   │
   ▼
LoRa Telemetry
   │
   ▼
GCS
   │
   ├── Live Status
   ├── Temperature Graph
   ├── Alerts
   └── Operating Controls
   │
   ▼
Experimental Record

The software interface and the experimental evidence remain separate: displaying a value does not by itself establish system performance or validation.

39. GCS Design Principles

The HIFLY GCS follows the following principles:

Clear monitoring
Important thermal and electrical information is visible in one interface.
Thermal awareness
Battery and ambient temperature are treated as primary system variables.
Electrical awareness
Voltage and current are displayed alongside thermal information.
Communication awareness
LoRa link status is explicitly represented.
Safety visibility
Safety and fault conditions are shown to the operator.
Supervised control
AUTO, PRE-HEAT, and HEATER OFF functions provide a clear operator interface.
Onboard autonomy
The GCS does not replace onboard thermal and safety logic.
Data traceability
Displayed information originates from onboard measurements and firmware states.
40. GCS Functional Summary
GCS Function	HIFLY Implementation Concept
Battery temperature	Numeric telemetry display
Ambient temperature	Numeric telemetry display
Voltage	Electrical telemetry display
Current	Electrical telemetry display
Heater status	ON / OFF status
Thermal status	Thermal-state display
LoRa link	Communication-state display
AUTO/MANUAL	Operating-mode display
Safety status	Safety-state display
Temperature graph	Thermal trend visualization
Low-temperature alert	Operator warning
Over-temperature alert	Safety warning
Sensor-fault alert	Measurement warning
Communication-lost alert	Link warning
AUTO control	Automatic onboard thermal control
PRE-HEAT control	Supervised heating command
HEATER OFF control	Supervised heater-disable command
41. Overall GCS Data and Control Flow
                         HIFLY SYSTEM
                              │
                              ▼
                         ┌──────────┐
                         │ Sensors  │
                         └────┬─────┘
                              │
                              ▼
                         ┌──────────┐
                         │ Firmware │
                         └────┬─────┘
                              │
            ┌─────────────────┼─────────────────┐
            │                 │                 │
            ▼                 ▼                 ▼
        Thermal            Power             Safety
         State              Data              State
            │                 │                 │
            └─────────────────┼─────────────────┘
                              │
                              ▼
                         ┌──────────┐
                         │Telemetry │
                         └────┬─────┘
                              │
                              ▼
                         ┌──────────┐
                         │  LoRa    │
                         └────┬─────┘
                              │
                              ▼
                         ┌──────────┐
                         │   GCS    │
                         └────┬─────┘
                              │
             ┌────────────────┼────────────────┐
             │                │                │
             ▼                ▼                ▼
          Display           Graphs           Alerts
             │                │                │
             └────────────────┼────────────────┘
                              │
                              ▼
                           Operator
                              │
                              ▼
                    AUTO / PRE-HEAT /
                       HEATER OFF
                              │
                              ▼
                             LoRa
                              │
                              ▼
                        HIFLY Firmware
42. Final GCS Architecture
┌──────────────────────────────────────────────────────────────┐
│                         HIFLY GCS                            │
│                                                              │
│  ┌──────────────┐ ┌──────────────┐ ┌─────────────────────┐  │
│  │ Battery Temp │ │ Ambient Temp │ │ Voltage / Current   │  │
│  └──────────────┘ └──────────────┘ └─────────────────────┘  │
│                                                              │
│  ┌──────────────┐ ┌──────────────┐ ┌─────────────────────┐  │
│  │ Heater State │ │ Thermal State│ │ LoRa Link           │  │
│  └──────────────┘ └──────────────┘ └─────────────────────┘  │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐  │
│  │                 TEMPERATURE GRAPH                      │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐  │
│  │                       ALERTS                           │  │
│  │                                                        │  │
│  │ Low Temperature • Over-temperature • Sensor Fault     │  │
│  │ Communication Lost                                     │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐  │
│  │                     CONTROLS                           │  │
│  │                                                        │  │
│  │      AUTO       PRE-HEAT       HEATER OFF              │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                              │
└──────────────────────────────┬───────────────────────────────┘
                               │
                               ▼
                             LoRa
                               │
                               ▼
                     HIFLY ONBOARD CONTROLLER
                               │
             ┌─────────────────┼─────────────────┐
             │                 │                 │
             ▼                 ▼                 ▼
          Sensors          Thermal            Safety
                            Control             Logic

The HIFLY GCS therefore serves as the operator-facing layer for telemetry, thermal visualization, electrical monitoring, alerts, and supervised commands while maintaining a clear separation between ground supervision and onboard autonomous thermal-control functions.
