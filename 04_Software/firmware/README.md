# 04.1 — Firmware

## 1. Firmware Overview

The HIFLY firmware is the onboard software responsible for sensing, thermal management, electrical monitoring, safety evaluation, telemetry, and communication.

The firmware runs on the onboard controller associated with the HIFLY sensing and control hardware and interfaces with the LilyGO T3-S3 LoRa communication platform.

The firmware connects the physical system to the software architecture:

```text
Sensors
   │
   ├── Temperature
   ├── Voltage
   └── Current
          │
          ▼
┌───────────────────────┐
│   HIFLY Controller    │
│                       │
│ Sensor Acquisition    │
│ Thermal Evaluation    │
│ Heater Control        │
│ Power Monitoring      │
│ Safety Logic          │
│ Telemetry             │
│ Command Handling      │
└───────────┬───────────┘
            │
       ┌────┴────┐
       │         │
       ▼         ▼
    Heater      LoRa
    Control       │
                  ▼
                 GCS
```
The firmware is designed so that essential thermal and safety functions remain onboard rather than depending completely on the ground communication link.

2. Firmware Responsibilities

The firmware performs the following major functions:

Function	Firmware Responsibility
Sensor acquisition	Read temperature, voltage and current measurements
Temperature monitoring	Determine the current thermal condition
Thermal control	Operate the heating element based on temperature
Heater switching	Generate the control signal for the MOSFET stage
Power monitoring	Process voltage and current information
Safety logic	Detect abnormal or unsafe operating states
LoRa communication	Exchange telemetry and commands
Mode management	Handle AUTO and supervised operating modes
Telemetry	Package onboard measurements for the GCS
Fault handling	Maintain safe local operation when faults occur
3. Firmware Architecture

The firmware is organized into functional software blocks.

┌─────────────────────────────────────────────────────────────┐
│                       HIFLY FIRMWARE                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌───────────────────────────────────────────────────────┐  │
│  │                 INITIALIZATION                        │  │
│  │                                                       │  │
│  │ MCU • Sensors • GPIO • LoRa • Control States         │  │
│  └──────────────────────────┬────────────────────────────┘  │
│                             │                               │
│                             ▼                               │
│  ┌───────────────────────────────────────────────────────┐  │
│  │                 SENSOR ACQUISITION                    │  │
│  │                                                       │  │
│  │ Temperature • Voltage • Current                      │  │
│  └──────────────────────────┬────────────────────────────┘  │
│                             │                               │
│                             ▼                               │
│  ┌───────────────────────────────────────────────────────┐  │
│  │                DATA VALIDATION                         │  │
│  │                                                       │  │
│  │ Validity • Fault Detection • State Update            │  │
│  └──────────────────────────┬────────────────────────────┘  │
│                             │                               │
│             ┌───────────────┼───────────────┐               │
│             ▼               ▼               ▼               │
│       ┌────────────┐  ┌────────────┐  ┌────────────┐       │
│       │  THERMAL   │  │   SAFETY   │  │   POWER    │       │
│       │  CONTROL   │  │   LOGIC    │  │ MONITORING │       │
│       └─────┬──────┘  └─────┬──────┘  └─────┬──────┘       │
│             │               │               │               │
│             └───────────────┼───────────────┘               │
│                             ▼                               │
│                    ┌────────────────┐                       │
│                    │   TELEMETRY    │                       │
│                    └───────┬────────┘                       │
│                            │                                │
│                            ▼                                │
│                    ┌────────────────┐                       │
│                    │      LoRa      │                       │
│                    └───────┬────────┘                       │
│                            │                                │
└────────────────────────────┼────────────────────────────────┘
                             ▼
                            GCS
4. Initialization

During startup, the firmware establishes the basic operating state of the controller.

The initialization sequence includes:

Microcontroller initialization
GPIO configuration
Sensor initialization
Heater-control output initialization
Communication initialization
Initial operating-mode configuration
Initial safety-state configuration

The heater-control output is initialized to a safe state before normal thermal-control operation begins.

The firmware then enters the main control loop.

5. Sensor Acquisition

Sensor acquisition is the first stage of the recurring firmware cycle.

The firmware obtains measurements from the connected sensing elements and stores the values for use by the thermal-control, power-monitoring, safety, and telemetry functions.

             ┌─────────────────────┐
             │ Temperature Sensor  │
             └──────────┬──────────┘
                        │
             ┌──────────▼──────────┐
             │ Voltage Measurement │
             └──────────┬──────────┘
                        │
             ┌──────────▼──────────┐
             │ Current Measurement │
             └──────────┬──────────┘
                        │
                        ▼
               Firmware Variables
                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼
       Thermal        Safety        Telemetry
       Control         Logic

The firmware does not treat a sensor value as valid merely because a reading has been received. Sensor validity forms part of the safety and control processing.

6. Temperature Acquisition

Temperature measurements are used as the primary input to the active thermal-control function.

The firmware monitors:

battery temperature
ambient temperature
other connected temperature measurements where implemented

The measured temperature is passed to the thermal-state evaluation layer.

Temperature Sensor
       │
       ▼
Raw Measurement
       │
       ▼
Firmware Processing
       │
       ▼
Temperature State
       │
       ├──────────────► Thermal Control
       │
       ├──────────────► Safety Logic
       │
       └──────────────► Telemetry
7. Voltage and Current Acquisition

Voltage and current measurements provide electrical information for the HIFLY system.

The firmware processes these measurements for:

electrical-state monitoring
heater-operation observation
power calculation where both values are available
telemetry
experimental data collection

The electrical relationship used for power calculation is:

P = V × I

where:

P = electrical power
V = measured voltage
I = measured current

Measured voltage and current values remain the underlying data used for experimental electrical analysis.

8. Data Validation

Before measurements are used for control, the firmware evaluates whether the readings are usable.

The conceptual process is:

Sensor Reading
      │
      ▼
Validity Check
      │
   ┌──┴──┐
   │     │
Valid   Invalid
   │       │
   ▼       ▼
Normal   Fault State
Control     │
            ▼
       Safety Logic

Data validation is important because an invalid temperature measurement could otherwise produce an inappropriate heater command.

The firmware therefore treats sensor validity as part of the control and safety architecture.

9. Thermal-State Evaluation

The thermal-control layer converts the measured thermal condition into an operating state.

The general sequence is:

Measured Temperature
        │
        ▼
Temperature Validation
        │
        ▼
Thermal-State Evaluation
        │
        ├──────────► Normal
        │
        ├──────────► Heating Required
        │
        └──────────► Unsafe / Fault

The software architecture uses temperature-based control rather than relying on a continuously commanded heater state from the GCS.

The numerical control values are configuration-dependent and are not represented here as fixed experimental values unless supported by the implemented system configuration.

10. Heater Control

The heater is controlled through the MOSFET switching stage.

The firmware generates the control signal while the MOSFET provides the electrical switching interface between the controller and heating element.

Thermal Evaluation
       │
       ▼
Heater Command
       │
       ▼
MCU GPIO / Control Signal
       │
       ▼
MOSFET Switching Stage
       │
       ▼
Heating Element
       │
       ▼
Thermal Response
       │
       ▼
Temperature Sensor
       │
       └──────────────► Firmware Feedback

This creates a closed-loop thermal-control structure.

11. Automatic Thermal Control

AUTO mode allows the onboard controller to determine heater operation from the measured thermal condition.

The control cycle is:

START CONTROL CYCLE
        │
        ▼
Read Temperature
        │
        ▼
Validate Measurement
        │
        ▼
Evaluate Thermal State
        │
        ├──────────────┐
        │              │
        ▼              ▼
 Heating Required    Normal
        │              │
        ▼              ▼
   Heater ON       Heater OFF
        │              │
        └──────┬───────┘
               ▼
        Safety Evaluation
               │
               ▼
          Update Telemetry
               │
               ▼
          Next Cycle

The controller repeatedly evaluates the thermal state so that heater operation follows the measured condition.

12. Manual and Supervised Control

The firmware can process permitted commands originating from the GCS.

The main control concepts are:

AUTO
PRE-HEAT
HEATER OFF

The command path is:

GCS
 │
 ▼
LoRa Command
 │
 ▼
Onboard Receiver
 │
 ▼
Command Validation
 │
 ▼
Operating-State Update
 │
 ▼
Thermal / Heater Control

Commands are evaluated onboard rather than being directly connected to the heater without software processing.

13. AUTO Mode

In AUTO mode, the onboard firmware remains responsible for thermal control.

AUTO
 │
 ▼
Read Temperature
 │
 ▼
Evaluate Thermal State
 │
 ▼
Apply Thermal-Control Logic
 │
 ▼
Control Heater
 │
 ▼
Monitor Result
 │
 └──────────────► Repeat

The GCS provides monitoring and supervisory visibility while the essential thermal-control loop remains local to the HIFLY controller.

14. PRE-HEAT Mode

PRE-HEAT is a supervised function for applying the configured heating operation.

The firmware receives the command through the communication layer and evaluates it before applying the corresponding heater-control state.

GCS PRE-HEAT
      │
      ▼
LoRa Reception
      │
      ▼
Command Validation
      │
      ▼
Thermal / Safety Evaluation
      │
      ▼
Heater Control
15. HEATER OFF Command

The HEATER OFF command provides a direct software path for disabling active heating during permitted operation.

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
Heater Disabled

The onboard safety layer remains active independently of this command.

16. Power Monitoring

The firmware continuously integrates available voltage and current measurements into the system-monitoring layer.

Voltage ─────┐
             │
             ▼
        ┌───────────┐
Current ─► Firmware │
        │ Processing│
        └─────┬─────┘
              │
      ┌───────┼────────┐
      ▼       ▼        ▼
   Voltage  Current   Power
   Status   Status   Estimate
      │       │        │
      └───────┼────────┘
              ▼
           Telemetry

This provides visibility into electrical operation and supports later analysis of energy consumption.

17. Safety Logic

Safety logic is evaluated alongside the thermal-control process.

The firmware considers conditions including:

invalid sensor readings
unsafe thermal conditions
communication faults
invalid commands
abnormal system states

The general structure is:

Sensors
  │
  ▼
Measurements
  │
  ▼
Safety Evaluation
  │
 ┌┴──────────────────────┐
 │                       │
 ▼                       ▼
Normal                 Fault / Unsafe
 │                       │
 ▼                       ▼
Normal Control       Safety Response

Safety processing is performed onboard so that the system does not depend entirely on the availability of the GCS.

18. Sensor-Fault Handling

A sensor fault can compromise temperature-based control.

The firmware therefore separates valid sensor data from invalid or unavailable data.

Temperature Reading
        │
        ▼
Validation
        │
   ┌────┴────┐
   │         │
 Valid     Invalid
   │         │
   ▼         ▼
Thermal    Sensor
Control    Fault
             │
             ▼
        Safety Logic
             │
             ▼
       Safe Response

The specific safe response depends on the implemented hardware and firmware configuration.

19. Communication Monitoring

The firmware monitors the state of the LoRa communication path.

The communication state is used for:

telemetry status
GCS status indication
communication-loss detection
restoration of normal telemetry after link recovery
LoRa Link
   │
   ▼
Communication Monitor
   │
 ┌─┴──────────────┐
 │                │
 ▼                ▼
Available         Lost
 │                │
 ▼                ▼
Normal          Fault State
Telemetry          │
                   ▼
          Local Autonomous
          Thermal Control
20. Communication-Loss Operation

Communication loss does not terminate the onboard thermal-control loop.

The intended operation is:

Normal Operation
      │
      ▼
LoRa Communication
      │
      ├──────────────► Available
      │                    │
      │                    ▼
      │               Normal Telemetry
      │
      └──────────────► Lost
                           │
                           ▼
                    Communication Fault
                           │
                           ▼
                    Onboard Control
                           │
                           ▼
                     Safety Logic
                           │
                           ▼
                  Wait for Link Recovery
                           │
                           ▼
                   Resume Telemetry

This provides an autonomous operating path when ground communication is temporarily unavailable.

21. LoRa Telemetry

The LilyGO T3-S3 LoRa platform is used as the communication interface between the onboard system and GCS.

The firmware prepares telemetry information and passes it to the communication layer.

A conceptual telemetry packet contains:

HIFLY TELEMETRY
├── Battery Temperature
├── Ambient Temperature
├── Voltage
├── Current
├── Heater Status
├── Thermal Status
├── Operating Mode
├── Safety Status
└── Communication Status

The exact packet encoding depends on the implemented communication software.

22. Command Processing

Incoming commands are processed by the onboard firmware before affecting the physical system.

Incoming LoRa Packet
        │
        ▼
Packet Reception
        │
        ▼
Command Identification
        │
        ▼
Command Validation
        │
        ▼
Safety Evaluation
        │
        ▼
Control Action

This prevents the communication interface from becoming an uncontrolled direct path to the heater.

23. Telemetry State Representation

The firmware maintains software states that can be transmitted to the GCS.

A representative state set is:

THERMAL STATUS
├── NORMAL
├── HEATING
└── SAFETY / FAULT

OPERATING MODE
├── AUTO
└── MANUAL

COMMUNICATION STATUS
├── LINK AVAILABLE
└── LINK LOST

HEATER STATUS
├── ON
└── OFF

SAFETY STATUS
├── NORMAL
└── FAULT

The displayed terminology can be aligned with the final GCS implementation.

24. Firmware Main Loop

The overall firmware cycle is represented below:

┌──────────────────────────────────────────┐
│                STARTUP                   │
└───────────────────┬──────────────────────┘
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
          Set Safe Initial State
                    │
                    ▼
┌──────────────────────────────────────────┐
│                 LOOP                     │
│                                          │
│  1. Read Sensors                         │
│                                          │
│  2. Validate Sensor Data                 │
│                                          │
│  3. Evaluate Temperature                 │
│                                          │
│  4. Evaluate Thermal State               │
│                                          │
│  5. Process Operating Mode               │
│                                          │
│  6. Apply Heater Control                 │
│                                          │
│  7. Monitor Voltage and Current          │
│                                          │
│  8. Evaluate Safety State                │
│                                          │
│  9. Check Communication                  │
│                                          │
│ 10. Receive Commands                     │
│                                          │
│ 11. Prepare Telemetry                    │
│                                          │
│ 12. Transmit Telemetry                   │
│                                          │
│ 13. Repeat                               │
└──────────────────────────────────────────┘
25. Firmware-to-Hardware Interface

The firmware interfaces with the physical HIFLY hardware through defined functional paths.

                     HIFLY HARDWARE

       ┌─────────────────────────────────┐
       │        Temperature Sensors      │
       └───────────────┬─────────────────┘
                       │
       ┌───────────────▼─────────────────┐
       │             MCU                 │
       │                                 │
       │        HIFLY FIRMWARE           │
       └──────┬───────────────┬──────────┘
              │               │
              │               │
              ▼               ▼
       ┌─────────────┐  ┌──────────────┐
       │    MOSFET   │  │ LilyGO T3-S3 │
       │    Stage    │  │    LoRa      │
       └──────┬──────┘  └──────┬───────┘
              │                │
              ▼                ▼
       Heating Element        GCS
              │
              ▼
        Thermal System

Voltage / Current Sensors
              │
              ▼
             MCU
26. Firmware and Battery Thermal Management

The firmware forms the active control layer of the battery thermal-management system.

The complete relationship is:

                 BATTERY SYSTEM
                       │
             ┌─────────┴─────────┐
             │                   │
             ▼                   ▼
       Thermal Insulation    Temperature Sensor
             │                   │
             │                   ▼
             │                Firmware
             │                   │
             │            Thermal Evaluation
             │                   │
             │                   ▼
             │              Heater Command
             │                   │
             │                   ▼
             │              MOSFET Stage
             │                   │
             └──────────────────►▼
                              Heater
                                 │
                                 ▼
                            Battery Thermal
                              Condition
                                 │
                                 └──────► Feedback

The software therefore complements the passive and active hardware elements rather than functioning as an isolated software feature.

27. Firmware and Protection Architecture

The software also participates in the wider protection architecture.

High-Altitude Environment
          │
          ▼
Physical Protection
          │
          ├── Insulation
          ├── Protection Chamber
          ├── Flexible Silicone
          ├── Lightweight Protective Layer
          └── Antenna Protection
                    │
                    ▼
              Sensor Layer
                    │
                    ▼
             HIFLY Firmware
                    │
          ┌─────────┼─────────┐
          ▼         ▼         ▼
       Thermal    Safety   Communication
       Control     Logic      Logic

The firmware does not replace the physical environmental protection. It supervises the active electrical and thermal functions within the integrated system.

28. Software Data Path to GCS

The complete telemetry path is:

Physical Condition
       │
       ▼
Sensors
       │
       ▼
Firmware
       │
       ├── Temperature
       ├── Voltage
       ├── Current
       ├── Heater State
       ├── Thermal State
       └── Safety State
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
GCS
       │
       ├── Numerical Display
       ├── Temperature Graph
       ├── Alerts
       └── Control Interface
29. Software Reliability Concept

The HIFLY firmware is structured around local decision-making for functions that are directly related to thermal safety and system operation.

The design separates:

CRITICAL LOCAL FUNCTIONS
├── Sensor Reading
├── Thermal Evaluation
├── Heater Control
└── Safety Logic

COMMUNICATION FUNCTIONS
├── Telemetry
├── Command Reception
└── Link Monitoring

GROUND FUNCTIONS
├── Visualization
├── Graphing
├── Alerts
└── Supervised Control

This separation provides a clear boundary between local autonomous operation and remote supervision.

30. Firmware Development Structure

The firmware can be represented as the following logical modules:

firmware/
│
├── initialization
│   └── hardware and communication startup
│
├── sensors
│   ├── temperature acquisition
│   ├── voltage acquisition
│   └── current acquisition
│
├── thermal_control
│   ├── thermal-state evaluation
│   ├── automatic control
│   └── heater command
│
├── safety
│   ├── sensor validation
│   ├── fault handling
│   └── safe-state logic
│
├── power
│   ├── voltage monitoring
│   ├── current monitoring
│   └── power calculation
│
├── communication
│   ├── LoRa transmit
│   ├── LoRa receive
│   └── link monitoring
│
└── telemetry
    ├── data packaging
    └── GCS status

This logical separation makes the firmware easier to connect with the corresponding hardware and GCS components.

31. Relationship with Ground Control Station

The firmware and GCS form a two-level control and monitoring architecture.

                 HIFLY ONBOARD SYSTEM
                         │
              ┌──────────┴──────────┐
              │                     │
              ▼                     ▼
        Local Control          Telemetry
              │                     │
              ▼                     ▼
        Thermal / Safety           LoRa
              │                     │
              │                     ▼
              │                    GCS
              │                     │
              │              ┌──────┼──────┐
              │              ▼      ▼      ▼
              │           Display  Graphs Alerts
              │
              └───────────────┐
                              │
                        Remote Commands
                              │
                              ▼
                           LoRa
                              │
                              ▼
                         Firmware

The local firmware remains responsible for essential onboard control while the GCS provides the operator-facing interface.

32. Firmware Status

The firmware architecture covers the required software functions for the HIFLY prototype:

Area	Function
Sensor acquisition	Temperature, voltage and current
Thermal control	Temperature-based heating
Heater interface	MOSFET control
Power monitoring	Voltage, current and derived power
Safety	Fault and unsafe-state handling
Communication	LilyGO T3-S3 LoRa
Telemetry	System-state transmission
Command handling	AUTO, PRE-HEAT and HEATER OFF
GCS integration	Monitoring, graphing, alerts and control

The repository separates firmware architecture from experimental validation. A software function is not treated as experimentally validated solely because the corresponding logic exists in the design.

33. Overall Firmware Flow
                         HIFLY FIRMWARE
                              │
                              ▼
                     ┌─────────────────┐
                     │   INITIALIZE    │
                     └────────┬────────┘
                              │
                              ▼
                     ┌─────────────────┐
                     │ READ SENSORS    │
                     └────────┬────────┘
                              │
                              ▼
                     ┌─────────────────┐
                     │ VALIDATE DATA   │
                     └────────┬────────┘
                              │
                 ┌────────────┼────────────┐
                 │            │            │
                 ▼            ▼            ▼
             THERMAL       SAFETY       POWER
             CONTROL        LOGIC       MONITOR
                 │            │            │
                 └────────────┼────────────┘
                              │
                              ▼
                     ┌─────────────────┐
                     │ PROCESS COMMAND │
                     └────────┬────────┘
                              │
                              ▼
                     ┌─────────────────┐
                     │ BUILD TELEMETRY │
                     └────────┬────────┘
                              │
                              ▼
                     ┌─────────────────┐
                     │      LoRa       │
                     └────────┬────────┘
                              │
                              ▼
                             GCS
                              │
                              ▼
                       Operator View
                              │
                              ▼
                        Commands
                              │
                              └──────────────► Firmware

The firmware therefore forms the central software layer connecting HIFLY's sensors, thermal-management hardware, electrical monitoring, autonomous safety behavior, LoRa communication, and Ground Control Station.
