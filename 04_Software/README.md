# 04 — Software

## 1. Software Overview

The HIFLY software layer provides the onboard sensing, thermal-control, power-monitoring, communication, safety, and ground-monitoring functions required to operate the high-altitude reliability system.

The software is designed around an onboard controller connected to temperature, voltage, and current sensing elements, a heating element through a MOSFET switching stage, and a LilyGO T3-S3 LoRa communication module.

The software architecture separates two major functions:

1. **Onboard autonomous operation** — sensing, thermal control, safety logic, and communication are performed on the HIFLY hardware.
2. **Ground monitoring and control** — the Ground Control Station (GCS) receives telemetry, displays system status, generates alerts, and provides permitted control commands.

The onboard thermal-control logic remains capable of operating when the communication link is temporarily unavailable. This prevents thermal management from depending entirely on continuous ground communication.

---

## 2. Software Architecture

```text
                         HIFLY SOFTWARE ARCHITECTURE

                    ┌──────────────────────────────┐
                    │        SENSOR LAYER          │
                    │                              │
                    │  Temperature Sensors        │
                    │  Voltage Monitoring          │
                    │  Current Monitoring          │
                    └──────────────┬───────────────┘
                                   │
                                   ▼
                    ┌──────────────────────────────┐
                    │     ONBOARD PROCESSING        │
                    │                              │
                    │  Sensor Acquisition          │
                    │  Data Validation             │
                    │  Thermal State Evaluation    │
                    │  Power/Energy Monitoring     │
                    │  Safety Evaluation           │
                    └──────────────┬───────────────┘
                                   │
                ┌──────────────────┼──────────────────┐
                │                  │                  │
                ▼                  ▼                  ▼
       ┌────────────────┐ ┌────────────────┐ ┌────────────────┐
       │ Thermal Control│ │ Safety Logic   │ │ Telemetry      │
       │                │ │                │ │ Processing     │
       │ Heater Control │ │ Fault Handling │ │ Data Packaging │
       │ AUTO Mode      │ │ Fail-safe      │ │ LoRa           │
       └───────┬────────┘ └───────┬────────┘ └───────┬────────┘
               │                  │                  │
               ▼                  ▼                  ▼
       ┌────────────────┐ ┌────────────────┐ ┌────────────────┐
       │ MOSFET / Heater│ │ Autonomous     │ │ LilyGO T3-S3   │
       │ Switching Stage│ │ Operation      │ │ LoRa Link      │
       └────────────────┘ └────────────────┘ └───────┬────────┘
                                                     │
                                                     ▼
                                          ┌────────────────────┐
                                          │ Ground Control     │
                                          │ Station            │
                                          │                    │
                                          │ Telemetry          │
                                          │ Graphs             │
                                          │ Alerts             │
                                          │ Control Commands   │
                                          └────────────────────┘
```
3. Onboard Software

The onboard software is responsible for maintaining local awareness of the thermal and electrical state of the system.

Its main functions are:

temperature acquisition
voltage monitoring
current monitoring
thermal-state evaluation
temperature-based heater control
safety-state evaluation
telemetry generation
LoRa communication
communication-loss handling
operating-mode management

The onboard controller does not require the GCS to continuously make thermal decisions. The GCS provides monitoring and permitted control functions, while the essential thermal and safety logic remains onboard.

3.1 Sensor Acquisition

The controller periodically reads the available sensors and converts the measurements into software variables used by the control and monitoring functions.

The monitored parameters include:

Parameter	Purpose
Battery temperature	Thermal condition of the battery
Ambient temperature	Environmental reference
Voltage	Electrical system monitoring
Current	Load and heater/power monitoring
Heater state	Confirmation of heating command
Communication state	LoRa link monitoring
Safety state	Overall operating condition

The sensor-acquisition layer provides the input data for both onboard control and telemetry.

3.2 Temperature Monitoring

Temperature is a primary control variable in HIFLY because high-altitude operation can expose the battery and electronics to low-temperature conditions.

The software evaluates the measured temperature and determines the current thermal state.

The control structure is:

Temperature Sensor
       │
       ▼
Temperature Reading
       │
       ▼
Sensor/Data Validation
       │
       ▼
Thermal State Evaluation
       │
       ├──────────────► Safe Thermal Condition
       │
       ├──────────────► Heating Required
       │
       └──────────────► Over-temperature / Fault State
                              │
                              ▼
                         Safety Response

The software architecture supports temperature-based decisions without requiring a fixed external command for every heating action.

3.3 Temperature-Based Thermal Control

HIFLY uses temperature-based thermal control to operate the heating element.

The controller continuously reads the relevant temperature and evaluates the thermal condition.

In automatic operation:

                 ┌─────────────────────┐
                 │ Read Temperature     │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Validate Reading    │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Evaluate Thermal    │
                 │ Condition           │
                 └──────────┬──────────┘
                            │
              ┌─────────────┼─────────────┐
              │             │             │
              ▼             ▼             ▼
          Heating       Normal       Unsafe /
          Required      Condition     Fault
              │             │             │
              ▼             ▼             ▼
          Heater ON      Heater OFF    Safety Logic
              │             │             │
              └─────────────┼─────────────┘
                            ▼
                     Update Telemetry

The heating element is controlled through the MOSFET switching stage rather than being driven directly by the controller.

The software therefore separates:

thermal decision
heater command
electrical switching
monitoring of the resulting system state

This architecture allows the thermal-control logic to remain independent from the high-current switching path.

3.4 Heater Control

The heater is controlled using a digital control signal connected to the MOSFET switching stage.

The software maintains a heater state such as:

HEATER OFF
HEATER ON

The corresponding physical path is:

Software Thermal Decision
          │
          ▼
    Heater Command
          │
          ▼
   MOSFET Driver/Switch
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
          └──────────────► Feedback

This creates a closed thermal-control loop in which the measured temperature influences the heater state.

3.5 Power Monitoring

HIFLY monitors voltage and current to provide visibility into the electrical operating condition.

The software can use these measurements for:

battery-state observation
load monitoring
heater operation monitoring
abnormal electrical-condition detection
telemetry
later energy analysis

Electrical telemetry is represented as:

Voltage Sensor ───────┐
                      │
                      ▼
                 ┌─────────┐
Current Sensor ─►│ MCU /   │
                 │ Software│
                 └────┬────┘
                      │
                      ├────► Electrical Status
                      │
                      ├────► Telemetry
                      │
                      └────► Data Logging

Where voltage and current measurements are available simultaneously, electrical power can be represented as:

Power = Voltage × Current

The measured values remain the source of truth for experimental power and energy analysis.

3.6 Battery Thermal Management

The battery thermal-management software is integrated with the temperature-monitoring and heater-control functions.

The software observes battery temperature and uses the temperature state to determine whether thermal support is required.

The intended control relationship is:

Battery Temperature
        │
        ▼
Thermal State
        │
        ├─────────────► Normal → Maintain Heater State
        │
        ├─────────────► Cold Condition → Heating Control
        │
        └─────────────► Unsafe Condition → Safety Response

This software layer complements the physical battery thermal-management system, which includes insulation, heating, sensing, and thermal protection.

The software does not replace the physical thermal-management hardware. It coordinates the active part of the thermal-management system.

3.7 Thermal State

The software uses a thermal state abstraction so that the same information can be represented both onboard and at the GCS.

A conceptual state model is:

                ┌───────────────┐
                │ NORMAL        │
                └───────┬───────┘
                        │
             Temperature requires
                 thermal support
                        │
                        ▼
                ┌───────────────┐
                │ HEATING       │
                └───────┬───────┘
                        │
                Thermal condition
                     restored
                        │
                        ▼
                ┌───────────────┐
                │ NORMAL        │
                └───────────────┘

Any detected unsafe condition
             │
             ▼
     ┌─────────────────┐
     │ SAFETY / FAULT  │
     └─────────────────┘

The exact numerical thresholds are configuration-dependent and are not represented as fixed values in this repository unless supported by an implemented or validated system configuration.

4. Communication Software

HIFLY uses a LilyGO T3-S3 LoRa platform for low-power wireless telemetry and command communication.

The communication software is responsible for:

preparing telemetry packets
transmitting system status
receiving permitted commands
identifying communication availability
detecting communication loss
restoring normal communication operation when the link returns

The communication path is:

Onboard Sensors
      │
      ▼
Onboard Controller
      │
      ▼
Telemetry Packet
      │
      ▼
LilyGO T3-S3
      │
      ▼
      LoRa
      │
      ▼
Ground Receiver
      │
      ▼
GCS
4.1 Telemetry Data

The telemetry structure is designed around the parameters required to understand the thermal and electrical condition of HIFLY.

Representative telemetry fields include:

Telemetry Field	Description
Battery temperature	Current battery thermal condition
Ambient temperature	Environmental temperature
Voltage	Measured electrical voltage
Current	Measured electrical current
Heater status	Current heater state
Thermal status	Software thermal state
Communication status	Link condition
Operating mode	AUTO or MANUAL
Safety status	Current safety state

The same telemetry values can be displayed numerically and graphically at the GCS.

4.2 Communication-Loss Handling

Communication loss is treated as a communication fault rather than as a command to stop autonomous thermal management.

The intended sequence is:

                 LoRa Link
                    │
          ┌─────────┴─────────┐
          │                   │
       Available            Lost
          │                   │
          ▼                   ▼
    Normal Telemetry     Detect Link Loss
          │                   │
          │                   ▼
          │          Onboard Autonomous
          │          Thermal Control
          │                   │
          │                   ▼
          │             Safety Logic
          │                   │
          └───────────┬───────┘
                      │
                Link Restored
                      │
                      ▼
               Resume Telemetry

This ensures that communication failure does not automatically remove the onboard controller's ability to perform its local thermal-control function.

5. Operating Modes

The HIFLY software supports the operating concepts required for autonomous and supervised operation.

5.1 AUTO Mode

AUTO mode allows the onboard controller to perform temperature-based thermal control according to the implemented thermal-control logic.

The basic sequence is:

Sensor Reading
     │
     ▼
Thermal Evaluation
     │
     ▼
Automatic Heater Decision
     │
     ▼
MOSFET / Heater Control
     │
     ▼
Temperature Feedback

AUTO mode is intended for normal autonomous thermal operation.

5.2 MANUAL Mode

MANUAL mode provides operator control of the heater through the GCS when permitted by the system safety state.

Manual control is intended for controlled testing, commissioning, and supervised operation.

The GCS command path is:

Operator
   │
   ▼
GCS Command
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
Heater Control

Safety logic remains active independently of the selected operating mode.

5.3 PRE-HEAT

PRE-HEAT is a supervised operating function for initiating thermal conditioning before or during a mission phase where additional thermal support is required.

The command path follows the same validated GCS-to-controller communication path.

GCS
 │
 └──► PRE-HEAT Command
             │
             ▼
       Onboard Validation
             │
             ▼
       Thermal Control
             │
             ▼
           Heater
5.4 HEATER OFF

HEATER OFF provides a direct command for disabling the heater during permitted manual or supervisory operation.

GCS
 │
 └──► HEATER OFF
          │
          ▼
   Command Validation
          │
          ▼
    Heater Disabled

A safety condition can independently disable or restrict heater operation where required by the implemented safety logic.

6. Safety Logic

Safety logic operates as an independent software layer around the thermal-control and communication functions.

The safety layer considers conditions such as:

sensor faults
invalid measurements
communication loss
unsafe thermal conditions
invalid or conflicting commands

The general structure is:

                  ┌─────────────────────┐
                  │ Sensor / System     │
                  │ Measurements        │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │ Safety Evaluation   │
                  └──────────┬──────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              ▼              ▼
           NORMAL         WARNING         FAULT
              │              │              │
              ▼              ▼              ▼
        Normal Control   Alert / Log     Safe Response

The software architecture keeps safety evaluation separate from the normal operator interface so that the GCS does not become the sole safety controller.

6.1 Sensor Fault Handling

A sensor fault can affect the reliability of temperature-based thermal control.

The software therefore treats sensor validity as part of the control process.

Sensor Reading
      │
      ▼
Validity Check
      │
 ┌────┴────┐
 │         │
Valid    Invalid
 │         │
 ▼         ▼
Control   Fault State
Logic       │
            ▼
       Safety Response

The exact response depends on the sensor and the implemented safety configuration.

6.2 Communication Fault Handling

The software monitors the communication state independently from the thermal-control loop.

If the LoRa link is unavailable:

the onboard controller continues local sensing
the onboard thermal-control logic remains available
the system enters a communication-fault state
the GCS cannot provide live telemetry until communication is restored

This separates communication availability from essential onboard thermal operation.

7. Ground Control Station Software

The Ground Control Station provides the operator interface for observing and supervising HIFLY.

The GCS is designed around four primary functions:

Telemetry display
Thermal monitoring
Safety and fault indication
Permitted operator control

A conceptual interface is:

┌─────────────────────────────────────────────────────────────┐
│                         HIFLY GCS                           │
├─────────────────────────────────────────────────────────────┤
│ Battery Temp │ Ambient Temp │ Voltage │ Current             │
├─────────────────────────────────────────────────────────────┤
│ Heater       │ Thermal      │ LoRa Link │ Safety Status    │
│ Status       │ Status       │           │                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│                 TEMPERATURE GRAPH                           │
│                                                             │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│ Alerts:                                                     │
│ • Low Temp                                                  │
│ • Over-temperature                                          │
│ • Sensor Fault                                              │
│ • Communication Lost                                        │
├─────────────────────────────────────────────────────────────┤
│ Mode: AUTO / MANUAL                                        │
│                                                             │
│ [ AUTO ] [ PRE-HEAT ] [ HEATER OFF ]                       │
└─────────────────────────────────────────────────────────────┘

The GCS is intended to provide a clear view of the system condition without replacing the onboard autonomous control logic.

7.1 GCS Telemetry Display

The primary GCS telemetry fields are:

Field	Display Purpose
Battery Temperature	Observe battery thermal condition
Ambient Temperature	Observe surrounding thermal condition
Voltage	Observe electrical supply
Current	Observe electrical load
Heater Status	Confirm heater state
Thermal Status	Show current thermal condition
LoRa Link	Show communication availability
AUTO / MANUAL	Show operating mode
Safety Status	Show system safety condition
7.2 Temperature Graph

The GCS temperature graph provides a visual representation of thermal behavior.

The graph can be used to observe:

battery-temperature variation
ambient-temperature variation
thermal response to heater operation
thermal stabilization
changes during different operating conditions

The graph is intended to support both real-time monitoring and later analysis when timestamped data is available.

7.3 GCS Alerts

The GCS alert system represents abnormal or important operating conditions.

The primary alert categories are:

LOW TEMPERATURE
      │
      ▼
Thermal condition requires attention

OVER-TEMPERATURE
      │
      ▼
Unsafe thermal condition indicated

SENSOR FAULT
      │
      ▼
Measurement reliability affected

COMMUNICATION LOST
      │
      ▼
Ground telemetry unavailable

Alerts provide operator awareness while the onboard controller continues to execute local safety and thermal-control logic.

7.4 GCS Controls

The primary operator controls are:

Control	Function
AUTO	Select onboard automatic thermal control
PRE-HEAT	Initiate the configured pre-heating function
HEATER OFF	Disable the heater through the validated command path

Commands are transmitted through the LoRa communication link and processed by the onboard controller.

8. Firmware Structure

The firmware is organized around modular functional blocks.

Firmware
│
├── Initialization
│
├── Sensor Acquisition
│   ├── Temperature
│   ├── Voltage
│   └── Current
│
├── Thermal Control
│   ├── Thermal State
│   └── Heater Control
│
├── Safety Logic
│   ├── Sensor Validation
│   ├── Fault Handling
│   └── Communication Monitoring
│
├── Power Monitoring
│
├── Telemetry
│
├── LoRa Communication
│
└── Command Handling
    ├── AUTO
    ├── PRE-HEAT
    └── HEATER OFF

This modular structure allows the sensing, control, communication, and safety functions to be developed and tested as related but distinguishable software components.

8.1 Main Firmware Loop

The conceptual firmware loop is:

START
  │
  ▼
Initialize Hardware
  │
  ▼
Initialize Sensors
  │
  ▼
Initialize LoRa
  │
  ▼
Initialize Control / Safety States
  │
  ▼
┌───────────────────────────────┐
│          MAIN LOOP            │
│                               │
│  Read Sensors                 │
│       │                       │
│       ▼                       │
│  Validate Measurements        │
│       │                       │
│       ▼                       │
│  Evaluate Thermal Condition   │
│       │                       │
│       ▼                       │
│  Apply Thermal Control        │
│       │                       │
│       ▼                       │
│  Evaluate Safety              │
│       │                       │
│       ▼                       │
│  Process Commands             │
│       │                       │
│       ▼                       │
│  Prepare Telemetry            │
│       │                       │
│       ▼                       │
│  Transmit / Receive LoRa      │
│       │                       │
│       └──────────► Repeat     │
└───────────────────────────────┘
9. Data Flow

The complete software data flow connects physical measurements to onboard control and ground monitoring.
```

          PHYSICAL SYSTEM
                │
                ▼
        ┌───────────────┐
        │    Sensors    │
        └───────┬───────┘
                │
                ▼
        ┌───────────────┐
        │ MCU / Firmware│
        └───────┬───────┘
                │
       ┌────────┼────────┐
       │        │        │
       ▼        ▼        ▼
   Thermal    Safety   Power
   Control    Logic   Monitoring
       │        │        │
       └────────┼────────┘
                │
                ▼
          Telemetry Data
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
       ┌────────┼────────┐
       ▼        ▼        ▼
    Display   Graphs   Alerts
                         │
                         ▼
                      Control
                      Commands
                         │
                         ▼
                       LoRa
                         │
                         ▼
                    MCU Firmware
```
10. Control and Communication Separation

One of the important software design principles in HIFLY is the separation between:

onboard control
communication
ground supervision

The GCS can request an operating mode or permitted action, but the onboard controller remains responsible for evaluating the current local system state.

This architecture reduces dependence on the communication link for basic thermal operation.

                 GROUND
                   │
                   ▼
                 GCS
                   │
              Commands /
              Telemetry
                   │
                  LoRa
                   │
                   ▼
              ONBOARD MCU
                   │
        ┌──────────┼──────────┐
        │          │          │
        ▼          ▼          ▼
     Thermal     Safety     Sensors
     Control      Logic        │
        │          │           │
        └──────────┼───────────┘
                   ▼
             Physical System
11. Software Evidence

The software portion of HIFLY is intended to be represented in the repository through the firmware, control logic, telemetry structure, GCS implementation, and associated experimental data.

The repository software evidence is connected to the physical system through:

Firmware
   │
   ├── Sensor Acquisition
   │
   ├── Thermal Control
   │
   ├── Heater Control
   │
   ├── Power Monitoring
   │
   ├── Safety Logic
   │
   └── LoRa Communication
             │
             ▼
            GCS
             │
             ├── Telemetry
             ├── Temperature Graph
             ├── Alerts
             └── Controls

This software architecture forms the control and monitoring layer connecting the HIFLY hardware with its ground-side operator interface.

12. Software Status

The software implementation is represented by the following functional areas:

Software Area	HIFLY Function
Firmware	Onboard controller operation
Sensor Acquisition	Temperature, voltage and current measurement
Thermal Control	Temperature-based heater control
Power Monitoring	Electrical-state observation
Safety Logic	Fault and unsafe-state handling
LoRa Communication	Telemetry and command link
GCS	Ground monitoring and supervision
Data Handling	Experimental and telemetry data support

The implementation status of individual software components is tracked separately from the architectural description so that implemented, simulated, tested, and planned functions are not presented as equivalent evidence.
