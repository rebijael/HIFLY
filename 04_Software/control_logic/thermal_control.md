# HIFLY Thermal Control Logic

## 1. Overview

The HIFLY thermal-control system is an onboard temperature-based control system designed to support reliable operation of the battery and associated electrical/electronic hardware under high-altitude thermal conditions.

The thermal-control architecture combines:

- temperature sensing
- thermal-state evaluation
- active heating
- MOSFET-based heater switching
- passive thermal insulation
- Pulsating Heat Pipe (PHP) thermal management
- safety logic
- onboard autonomous operation
- telemetry to the Ground Control Station (GCS)

The software controls the active thermal-management elements while the physical thermal architecture provides passive and passive-assisted heat-transfer functions.

```text
                    HIFLY THERMAL SYSTEM

                         Environment
                              │
                              ▼
                    ┌──────────────────┐
                    │ Thermal Conditions│
                    └─────────┬────────┘
                              │
             ┌────────────────┼────────────────┐
             │                │                │
             ▼                ▼                ▼
        Insulation          PHP            Heater
             │                │                │
             └────────────────┼────────────────┘
                              │
                              ▼
                         Battery/System
                              │
                              ▼
                       Temperature Sensor
                              │
                              ▼
                       HIFLY Controller
                              │
                              ▼
                    Thermal-Control Logic
                              │
                              ▼
                         MOSFET Stage
                              │
                              ▼
                            Heater
                              │
                              └──────────────► Feedback
```
2. Thermal-Control Objective

The thermal-control software provides a controlled response to changing temperature conditions.

The primary objective is to maintain the system within the configured operating thermal condition while avoiding uncontrolled heater operation.

The control loop is therefore based on feedback:

Temperature
     │
     ▼
Measurement
     │
     ▼
Thermal Evaluation
     │
     ▼
Control Decision
     │
     ▼
Heater Command
     │
     ▼
Thermal Response
     │
     ▼
Temperature
     │
     └──────────────► Feedback

The thermal-control software is one part of the complete HIFLY thermal-management architecture and does not replace the physical insulation, PHP, enclosure, or other thermal-protection elements.

3. Thermal-Control Architecture

The HIFLY thermal-control architecture can be represented as four connected layers.

┌──────────────────────────────────────────────────────────┐
│                  1. ENVIRONMENT                          │
│                                                          │
│        High-altitude temperature conditions              │
└─────────────────────────┬────────────────────────────────┘
                          │
                          ▼
┌──────────────────────────────────────────────────────────┐
│                  2. PASSIVE PROTECTION                   │
│                                                          │
│        Thermal insulation / physical enclosure           │
└─────────────────────────┬────────────────────────────────┘
                          │
                          ▼
┌──────────────────────────────────────────────────────────┐
│                  3. THERMAL MANAGEMENT                   │
│                                                          │
│        PHP + heating element + thermal paths             │
└─────────────────────────┬────────────────────────────────┘
                          │
                          ▼
┌──────────────────────────────────────────────────────────┐
│                  4. CONTROL SYSTEM                       │
│                                                          │
│        Temperature sensor + MCU + safety logic           │
└──────────────────────────────────────────────────────────┘

This layered approach combines passive thermal protection with active temperature-based control.

4. Temperature Feedback

Temperature is the primary feedback variable used by the thermal-control logic.

The firmware continuously reads the available temperature measurement and evaluates the current thermal state.

        ┌─────────────────────┐
        │ Temperature Sensor  │
        └──────────┬──────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │ Temperature Reading │
        └──────────┬──────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │ Data Validation     │
        └──────────┬──────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │ Thermal Evaluation  │
        └──────────┬──────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │ Heater Decision     │
        └─────────────────────┘

The same temperature information can also be transmitted to the GCS for monitoring.

5. Thermal States

The software represents the thermal condition using logical states.

┌─────────────────────┐
│       NORMAL        │
└──────────┬──────────┘
           │
           │ Thermal support required
           ▼
┌─────────────────────┐
│      HEATING        │
└──────────┬──────────┘
           │
           │ Thermal condition restored
           ▼
┌─────────────────────┐
│       NORMAL        │
└─────────────────────┘

Unsafe condition / sensor fault
           │
           ▼
┌─────────────────────┐
│   SAFETY / FAULT    │
└─────────────────────┘

The numerical thresholds associated with these states depend on the implemented configuration and validated operating requirements.

No unsupported fixed temperature threshold is assumed by this document.

6. Automatic Thermal Control

AUTO mode allows the onboard controller to operate the heater based on the measured thermal condition.

The control sequence is:

START
  │
  ▼
Read Temperature
  │
  ▼
Validate Reading
  │
  ▼
Evaluate Thermal State
  │
  ├───────────────┐
  │               │
  ▼               ▼
Heating Required  Normal
  │               │
  ▼               ▼
Heater ON      Heater OFF
  │               │
  └───────┬───────┘
          ▼
   Safety Evaluation
          │
          ▼
   Telemetry Update
          │
          ▼
      Next Cycle

The controller repeatedly performs this process while the system is operating in automatic mode.

7. Heater-Control Path

The software does not drive the heating element directly.

The controller generates a heater-control signal that is passed through the MOSFET switching stage.

Thermal-State Decision
          │
          ▼
   Heater Command
          │
          ▼
      MCU Output
          │
          ▼
   MOSFET Switch
          │
          ▼
   Heating Element
          │
          ▼
    Thermal System
          │
          ▼
 Temperature Sensor
          │
          └────────────► Feedback

This separates low-power controller logic from the electrical switching path used by the heater.

8. Heater States

The software represents the heater using a simple operational state.

HEATER OFF
     │
     │ Heating required
     ▼
HEATER ON
     │
     │ Thermal condition no longer requires heating
     ▼
HEATER OFF

A safety or fault condition can also cause the control system to enter a safe heater state according to the implemented firmware logic.

9. Temperature-Based Decision Flow

The thermal decision process is:

                  Temperature Reading
                          │
                          ▼
                  ┌───────────────┐
                  │ Valid Reading?│
                  └───────┬───────┘
                          │
                    ┌─────┴─────┐
                    │           │
                   YES          NO
                    │           │
                    ▼           ▼
             Thermal State   Sensor Fault
               Evaluation       │
                    │           ▼
                    │      Safety Logic
                    │
          ┌─────────┼─────────┐
          │         │         │
          ▼         ▼         ▼
       Heating     Normal   Unsafe
       Required   Condition Condition
          │         │         │
          ▼         ▼         ▼
       Heater ON  Heater OFF Safety Logic

This structure keeps sensor validation and safety evaluation within the control path.

10. Thermal Control and PHP

The Pulsating Heat Pipe is a physical thermal-management element within HIFLY.

The PHP provides a passive-assisted heat-transfer path, while the software-controlled heater provides active thermal input where required.

The relationship is:

                  Thermal Input
                       │
              ┌────────┴────────┐
              │                 │
              ▼                 ▼
          Heating Element       PHP
              │                 │
              │                 │
              └────────┬────────┘
                       │
                       ▼
                Thermal Structure
                       │
                       ▼
                    Battery
                       │
                       ▼
                Temperature Sensor
                       │
                       ▼
                  Controller

The firmware does not directly control the internal pulsating behavior of the PHP.

The PHP is therefore treated as a physical thermal-management subsystem, while the firmware controls the active heating function.

11. PHP-Assisted Thermal Architecture

The complete thermal path can be represented as:

       ┌───────────────────────────────────┐
       │        High-Altitude Environment  │
       └──────────────────┬────────────────┘
                          │
                          ▼
                ┌─────────────────┐
                │    Insulation   │
                └────────┬────────┘
                         │
                         ▼
              ┌──────────────────────┐
              │ PHP Thermal Structure│
              └──────────┬───────────┘
                         │
                         ▼
                  ┌─────────────┐
                  │   Battery   │
                  └──────┬──────┘
                         │
                         ▼
                Temperature Sensor
                         │
                         ▼
                  HIFLY Controller
                         │
                         ▼
                   Heater Control
                         │
                         ▼
                     MOSFET
                         │
                         ▼
                      Heater

The PHP and insulation operate as physical thermal-management elements, while the controller provides active temperature-based support.

12. Thermal Control During Low-Temperature Conditions

When the measured thermal condition indicates that additional heating is required, the controller can command the heating element.

The sequence is:

Low / Cold Thermal Condition
            │
            ▼
    Temperature Measurement
            │
            ▼
     Thermal-State Evaluation
            │
            ▼
       Heating Required
            │
            ▼
         Heater ON
            │
            ▼
     Thermal Response
            │
            ▼
    Temperature Measurement
            │
            ▼
      Updated State

The actual thermal response depends on the physical thermal design, environmental conditions, battery characteristics, heater characteristics, insulation, and PHP behavior.

13. Thermal Control During Normal Conditions

When the measured thermal condition is within the configured operating state, active heating is not required by the automatic control logic.

Normal Thermal Condition
            │
            ▼
    Temperature Measurement
            │
            ▼
      Thermal Evaluation
            │
            ▼
       Heating Not Required
            │
            ▼
         Heater OFF
            │
            ▼
      Continue Monitoring

The controller continues monitoring rather than terminating the thermal-control loop.

14. Thermal Safety

Thermal safety is evaluated independently from the normal heating command.

The safety path is:

Temperature
     │
     ▼
Thermal Evaluation
     │
     ├──────────────► Normal
     │
     ├──────────────► Heating
     │
     └──────────────► Unsafe
                           │
                           ▼
                     Safety Response

This prevents normal heating logic from being treated as the only thermal decision mechanism.

15. Over-Temperature Handling

An over-temperature condition is treated as a safety condition.

The conceptual response is:

Temperature Measurement
          │
          ▼
Thermal Evaluation
          │
          ▼
Unsafe / Over-temperature
          │
          ▼
Safety State
          │
          ▼
Heater Safety Response
          │
          ▼
GCS Alert / Telemetry

The exact numerical over-temperature limit belongs to the validated system configuration and is not fixed by this architecture document.

16. Sensor Fault Handling

A temperature sensor fault is important because the thermal-control loop depends on temperature feedback.

The software therefore follows a separate fault path:

Temperature Sensor
        │
        ▼
Measurement
        │
        ▼
Validity Check
        │
   ┌────┴────┐
   │         │
 Valid     Invalid
   │         │
   ▼         ▼
Normal     Sensor Fault
Control       │
              ▼
        Safety Evaluation
              │
              ▼
        Safe Response

The system should not interpret an invalid sensor reading as a normal thermal measurement.

17. Communication Loss and Thermal Control

Thermal control is designed to remain onboard even if the LoRa communication link is unavailable.

                  NORMAL OPERATION
                         │
                         ▼
                    LoRa Link
                         │
                  ┌──────┴──────┐
                  │             │
              Available        Lost
                  │             │
                  ▼             ▼
             GCS Telemetry   Link Fault
                  │             │
                  │             ▼
                  │       Local Thermal
                  │          Control
                  │             │
                  └──────┬──────┘
                         │
                         ▼
                    Safety Logic

This prevents communication loss from automatically disabling the onboard thermal-management decision loop.

18. AUTO / MANUAL Relationship

The thermal-control architecture supports both autonomous and supervised operating concepts.

AUTO
Temperature
     │
     ▼
Thermal Logic
     │
     ▼
Heater Decision
     │
     ▼
MOSFET
     │
     ▼
Heater
MANUAL
GCS
 │
 ▼
Operator Command
 │
 ▼
LoRa
 │
 ▼
Onboard Validation
 │
 ▼
Thermal / Safety Logic
 │
 ▼
Heater Control

The onboard safety layer remains active in both cases.

19. PRE-HEAT Control

PRE-HEAT provides a supervised method for initiating thermal conditioning.

GCS
 │
 ▼
PRE-HEAT
 │
 ▼
LoRa Command
 │
 ▼
Onboard Command Validation
 │
 ▼
Safety Evaluation
 │
 ▼
Thermal Control
 │
 ▼
Heater

The resulting temperature is monitored through the same feedback system used during automatic operation.

20. Heater OFF Control

The HEATER OFF command provides a direct operator command to disable active heating.

GCS
 │
 ▼
HEATER OFF
 │
 ▼
LoRa
 │
 ▼
Firmware
 │
 ▼
Command Validation
 │
 ▼
Heater OFF
 │
 ▼
Continue Temperature Monitoring

The thermal-control system continues operating after the heater is switched off.

21. Thermal-Control Feedback Loop

The complete feedback loop is:

                ┌──────────────────────┐
                │     HEATING ELEMENT  │
                └──────────┬───────────┘
                           │
                           ▼
                ┌──────────────────────┐
                │    THERMAL SYSTEM    │
                │                      │
                │ Battery + PHP +      │
                │ Insulation + Thermal │
                │ Structure            │
                └──────────┬───────────┘
                           │
                           ▼
                ┌──────────────────────┐
                │ TEMPERATURE SENSOR   │
                └──────────┬───────────┘
                           │
                           ▼
                ┌──────────────────────┐
                │ HIFLY CONTROLLER     │
                │                      │
                │ Thermal Evaluation   │
                │ Safety Logic         │
                └──────────┬───────────┘
                           │
                           ▼
                ┌──────────────────────┐
                │ MOSFET CONTROL       │
                └──────────┬───────────┘
                           │
                           └──────────────► Heater

This feedback arrangement allows the active heater to respond to measured temperature rather than operating as an uncontrolled continuous load.

22. Thermal and Electrical Monitoring

Thermal control is linked with electrical monitoring because heater operation affects system power consumption.

Temperature
     │
     ▼
Thermal Control
     │
     ▼
Heater State
     │
     ▼
Electrical Load
     │
 ┌───┴──────────────┐
 ▼                  ▼
Voltage            Current
 │                  │
 └────────┬─────────┘
          ▼
      Power Data
          │
          ▼
       Telemetry

This allows thermal operation to be observed together with electrical behavior.

23. Thermal Control and Energy Management

The active heater contributes to the system's electrical energy consumption.

The control architecture therefore connects thermal decisions with power monitoring:

Thermal Requirement
        │
        ▼
    Heater State
        │
        ▼
   Electrical Load
        │
        ▼
 Voltage / Current
        │
        ▼
   Power Monitoring
        │
        ▼
      Telemetry
        │
        ▼
       GCS

This relationship is important for evaluating the trade-off between thermal support and available mission energy.

24. Thermal Control and Battery Protection

The battery is both an energy source and a temperature-sensitive component of the system.

The HIFLY architecture therefore connects:

Battery
  │
  ├── Electrical Monitoring
  │       │
  │       ├── Voltage
  │       └── Current
  │
  └── Thermal Monitoring
          │
          ├── Battery Temperature
          ├── Heating
          ├── Insulation
          └── PHP-Assisted Thermal Path

The firmware coordinates the active heating function while the physical design provides the supporting thermal environment.

25. Thermal Control State Machine

A simplified state-machine representation is:

                    ┌─────────────┐
                    │   STARTUP   │
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │    NORMAL   │
                    └──────┬──────┘
                           │
                Heating required
                           │
                           ▼
                    ┌─────────────┐
                    │   HEATING   │
                    └──────┬──────┘
                           │
                  Thermal condition
                       restored
                           │
                           ▼
                    ┌─────────────┐
                    │    NORMAL   │
                    └─────────────┘

Sensor fault / unsafe condition
                │
                ▼
         ┌─────────────┐
         │ SAFETY/FAULT│
         └──────┬──────┘
                │
          Safe recovery
                │
                ▼
         Thermal Monitoring

The exact transition conditions are determined by the implemented firmware configuration.

26. Control-Loop Relationship With GCS

The GCS provides remote visibility and permitted commands, but it does not replace the onboard feedback loop.

                  ONBOARD SYSTEM
                       │
                       ▼
                 Temperature
                       │
                       ▼
                 Thermal Logic
                       │
                       ▼
                    Heater
                       │
                       ▼
                  Temperature
                       │
                       └──────────► Feedback

                       ▲
                       │
                  GCS Commands
                       │
                       │
                      LoRa
                       │
                       ▼
                  ONBOARD MCU

The onboard controller therefore remains the central element of the thermal-control loop.

27. Thermal-Control Data

The thermal-control system can generate data suitable for later analysis.

Relevant fields include:

Data	Use
Battery temperature	Thermal response
Ambient temperature	Environmental reference
Heater status	Control-state correlation
Voltage	Electrical operating state
Current	Heater/system load
Thermal status	Software control state
Safety status	Fault/state correlation
Communication state	Link-condition correlation

When timestamps are available, these values can be plotted against time.

When timestamps are not available, measurements can still be represented by reading index for experimental inspection.

28. Relationship to Experimental Data

HIFLY includes temperature, voltage, and current measurements that can be used to examine the relationship between electrical operation and thermal response.

The available sample measurements are:

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

These measurements are represented by reading index because timestamps are not available in the supplied dataset.

The dataset does not by itself establish a time-dependent thermal-performance curve or a validated thermal-control threshold.

29. Control-System Limitations of the Available Data

The available sample data provides measured current, voltage, and temperature values but does not contain timestamps or a complete record of operating conditions.

Therefore, the data can support:

inspection of measured electrical values
inspection of measured temperature values
reading-index plots
calculation of instantaneous power from corresponding voltage and current readings

The data alone does not establish:

heating rate per unit time
cooling rate per unit time
steady-state thermal performance
validated temperature-control thresholds
long-duration endurance
PHP performance improvement
mission-level energy consumption

Such conclusions require appropriately timestamped and controlled experimental measurements.

30. Thermal-Control Verification Path

The thermal-control system can be evaluated through a staged verification process.

Firmware Logic
      │
      ▼
Sensor Reading
      │
      ▼
Control Decision
      │
      ▼
Heater Command
      │
      ▼
Electrical Response
      │
      ▼
Thermal Response
      │
      ▼
Measured Temperature
      │
      ▼
Recorded Data
      │
      ▼
Analysis

The software architecture is therefore connected directly to measurable physical behavior.

31. Fault and Safety Relationship

The thermal-control system treats faults as separate from ordinary thermal states.

                 SENSOR DATA
                      │
                      ▼
               ┌──────────────┐
               │   VALIDATE   │
               └──────┬───────┘
                      │
               ┌──────┴──────┐
               │             │
             VALID         INVALID
               │             │
               ▼             ▼
        Thermal Control   Sensor Fault
               │             │
               ▼             ▼
        Heater Decision   Safety Logic
               │             │
               └──────┬──────┘
                      ▼
                 System State

This structure avoids treating a faulty sensor measurement as an ordinary thermal-control input.

32. Thermal Control With Communication Recovery

When communication is restored after a communication-loss event, the system resumes telemetry while the onboard controller continues operating from its current local state.

Communication Lost
       │
       ▼
Local Thermal Control
       │
       ▼
Safety Monitoring
       │
       ▼
LoRa Link Restored
       │
       ▼
Telemetry Resumes
       │
       ▼
GCS Displays Current State

The communication layer therefore reconnects to the onboard control system rather than becoming the source of thermal control itself.

33. Overall Thermal-Control Flow
                         START
                           │
                           ▼
                 Initialize Controller
                           │
                           ▼
                  Initialize Sensors
                           │
                           ▼
                 Initialize Heater I/O
                           │
                           ▼
                    Initialize LoRa
                           │
                           ▼
                  ┌────────────────┐
                  │  READ SENSOR   │
                  └───────┬────────┘
                          │
                          ▼
                  ┌────────────────┐
                  │ VALIDATE DATA  │
                  └───────┬────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │ THERMAL EVALUATE │
                 └────────┬─────────┘
                          │
              ┌───────────┼───────────┐
              │           │           │
              ▼           ▼           ▼
          HEATING       NORMAL      UNSAFE
          REQUIRED      STATE        STATE
              │           │           │
              ▼           ▼           ▼
          HEATER ON    HEATER OFF  SAFETY LOGIC
              │           │           │
              └───────────┼───────────┘
                          │
                          ▼
                  POWER MONITORING
                          │
                          ▼
                  COMMUNICATION CHECK
                          │
                          ▼
                    TELEMETRY
                          │
                          ▼
                    NEXT CYCLE
34. Design Principles

The HIFLY thermal-control logic follows these principles:

Temperature-based control
Heater operation is based on measured thermal condition.
Local autonomy
Essential thermal-control logic remains onboard.
Feedback control
The heater is operated using measured temperature rather than an uncontrolled fixed command.
Hardware/software separation
The firmware controls the MOSFET switching stage while the hardware handles the corresponding electrical power path.
Safety separation
Safety evaluation remains distinct from normal thermal-control decisions.
Communication independence
Loss of LoRa communication does not inherently terminate local thermal control.
Electrical awareness
Voltage and current monitoring provide visibility into the electrical cost of active thermal management.
Physical and software integration
The software operates together with insulation, PHP, heating, sensing, and protection hardware as one thermal-management system.
35. Final Thermal-Control Architecture
                 HIGH-ALTITUDE ENVIRONMENT
                           │
                           ▼
                  ┌─────────────────┐
                  │     INSULATION  │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │      PHP        │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │     BATTERY     │
                  └────────┬────────┘
                           │
                           ▼
                 TEMPERATURE SENSOR
                           │
                           ▼
                  ┌─────────────────┐
                  │ HIFLY FIRMWARE  │
                  │                 │
                  │ Sensor Check    │
                  │ Thermal State   │
                  │ Safety Logic    │
                  │ Mode Handling   │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │ HEATER CONTROL  │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │ MOSFET SWITCH   │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │ HEATING ELEMENT │
                  └────────┬────────┘
                           │
                           └──────────────► Thermal Feedback


        VOLTAGE / CURRENT
               │
               ▼
        POWER MONITORING
               │
               ▼
           TELEMETRY
               │
               ▼
             LoRa
               │
               ▼
              GCS
               │
       ┌───────┼────────┐
       ▼       ▼        ▼
    Display   Graphs   Alerts
                         │
                         ▼
                      Commands
                         │
                         └──────────────► LoRa

The HIFLY thermal-control system therefore combines physical thermal management, temperature feedback, active heating, electrical monitoring, onboard safety logic, and LoRa-based ground supervision into a single integrated control architecture.
