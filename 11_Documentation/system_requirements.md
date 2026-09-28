# HIFLY — System Requirements

## 1. Purpose

This document defines the system-level requirements for **HIFLY — Integrated High-Altitude UAV Reliability System**.

The requirements are derived from the seven high-altitude reliability challenges addressed by the project and establish the functional relationship between:

- environmental conditions,
- reliability risks,
- system functions,
- hardware,
- software,
- monitoring,
- protection,
- communication,
- and verification.

The requirements describe **what the system is intended to provide**. They do not by themselves constitute evidence that a requirement has been experimentally validated.

---

# 2. System Requirement Structure

The HIFLY requirement structure follows:

```text
High-Altitude Environment
          │
          ▼
Reliability Challenge
          │
          ▼
System Requirement
          │
          ▼
Engineering Function
          │
          ▼
Implementation
          │
          ▼
Verification Method

The principal requirement groups are:

                    HIFLY REQUIREMENTS
                           │
       ┌───────────────────┼───────────────────┐
       │                   │                   │
       ▼                   ▼                   ▼
    Thermal            Electrical        Communication
       │                   │                   │
       ▼                   ▼                   ▼
   Battery              Safety             Telemetry
       │                   │                   │
       └───────────────────┼───────────────────┘
                           ▼
                    System Integration
```
3. Seven Reliability Challenges

HIFLY addresses the following seven high-altitude reliability challenges:

ID	Reliability Challenge	Primary Engineering Response
CH-01	Reduced cooling efficiency	Pulsating Heat Pipe
CH-02	Insulation breakdown and electrical arcing	Low-pressure protection chamber
CH-03	Battery degradation	Smart Battery Thermal Management System
CH-04	Thermal cycling damage	Flexible silicone protection
CH-05	Increased radiation exposure	Lightweight protective layer
CH-06	Communication-system effects	Hydrophobic antenna protection and LoRa
CH-07	Mission energy and endurance	Energy and thermal management with power/energy monitoring
4. Requirement Classification

Requirements are organized into the following categories.

Category	Description
SYS	System-level requirements
THM	Thermal requirements
BAT	Battery requirements
ELE	Electrical requirements
ENV	Environmental-protection requirements
COM	Communication requirements
SW	Software requirements
GCS	Ground Control Station requirements
SAF	Safety requirements
DAT	Data and telemetry requirements
CAD	Mechanical/CAD integration requirements
TST	Verification and testing requirements
5. System-Level Requirements
SYS-01 — Integrated Reliability System

HIFLY shall provide an integrated architecture addressing the identified high-altitude reliability challenges through coordinated thermal, electrical, environmental-protection, communication, sensing, control, and monitoring functions.

Primary evidence: System architecture, hardware architecture, software architecture, CAD, testing.

SYS-02 — Subsystem Integration

The thermal, battery, electrical, communication, sensing, control, and monitoring subsystems shall be capable of operating as parts of a common HIFLY architecture.

        THERMAL
           │
           ▼
       SENSING
           │
           ▼
      CONTROL MCU
           │
    ┌──────┼──────┐
    │      │      │
    ▼      ▼      ▼
 BATTERY  HEATER  LoRa
    │      │      │
    └──────┼──────┘
           ▼
          GCS

Primary evidence: Integrated architecture and subsystem testing.

SYS-03 — System Monitoring

The system shall provide monitoring of relevant thermal and electrical operating parameters.

The principal monitored quantities are:

temperature,
voltage,
current.

Primary evidence: Firmware, telemetry, data records, GCS.

SYS-04 — System State Awareness

The system shall maintain an internal representation of relevant operating states, including thermal, electrical, heater, communication, and safety states.

Primary evidence: Firmware and telemetry implementation.

6. Thermal Requirements
THM-01 — Thermal Monitoring

The system shall monitor temperature at relevant thermal locations.

Temperature Sensor
        │
        ▼
Sensor Acquisition
        │
        ▼
Controller
        │
        ├────► Thermal Control
        │
        ├────► Safety Logic
        │
        └────► Telemetry

Primary evidence: Temperature sensor implementation and recorded measurements.

THM-02 — Thermal Management

The system shall provide a thermal-management architecture addressing reduced cooling efficiency and low-temperature operating conditions.

Primary engineering responses:

Pulsating Heat Pipe,
thermal insulation,
heating element,
temperature monitoring,
thermal control logic.
THM-03 — PHP Integration

The thermal architecture shall incorporate a Pulsating Heat Pipe as a passive thermal-management element.

Primary evidence: PHP CAD, thermal simulation, prototype, thermal testing.

THM-04 — PHP Thermal Evaluation

The PHP-assisted architecture shall be capable of being evaluated against an appropriate baseline thermal configuration.

Baseline Configuration
          │
          ▼
     Thermal Model
          │
          ▼
      Results A


PHP-Assisted Configuration
          │
          ▼
     Thermal Model
          │
          ▼
      Results B
          │
          └──────────────┐
                         ▼
                  Comparative Study

The comparison shall distinguish modeled thermal behaviour from experimentally measured behaviour.

THM-05 — Active Heating

The system shall provide an electrically controlled heating element for active thermal management.

Primary evidence: Heater hardware, MOSFET switching stage, firmware, test data.

THM-06 — Heater Control

The heating element shall be controllable by the embedded control system.

Temperature
     │
     ▼
Control Logic
     │
     ▼
Control Signal
     │
     ▼
MOSFET
     │
     ▼
Heating Element
THM-07 — Heater Status

The system shall maintain and report the operating state of the heater.

The heater state shall be available to the monitoring/telemetry architecture.

THM-08 — Thermal Control Continuity

Thermal control shall remain capable of operating through onboard logic when communication with the Ground Control Station is unavailable.

Primary evidence: Firmware implementation and communication-loss testing.

7. Battery Requirements
BAT-01 — Battery Thermal Monitoring

The system shall monitor battery temperature as part of the Smart Battery Thermal Management System.

Primary evidence: Battery temperature sensing and telemetry.

BAT-02 — Battery Electrical Monitoring

The system shall monitor electrical quantities associated with the battery and power system.

The principal quantities are:

voltage,
current.
BAT-03 — Battery Thermal Management

The battery subsystem shall be integrated with the HIFLY thermal-management architecture.

Battery
   │
   ├────► Temperature
   │
   ├────► Voltage
   │
   └────► Current
             │
             ▼
       Battery Monitoring
             │
             ▼
       Thermal Management
BAT-04 — Battery State Visibility

Relevant battery thermal and electrical information shall be available to the system monitoring and telemetry architecture.

8. Electrical Requirements
ELE-01 — Voltage Monitoring

The system shall provide voltage monitoring for the relevant power system.

ELE-02 — Current Monitoring

The system shall provide current monitoring for the relevant power system.

ELE-03 — Electrical Power Estimation

The system shall support instantaneous power calculation from measured voltage and current.

$$ P = V \times I $$

Power calculations shall use measured quantities rather than assumed values.

ELE-04 — Energy Monitoring

The system architecture shall support energy estimation when measurement data contains the required time information.

$$ E = \int P(t)\,dt $$

A sequence of voltage/current readings without timestamps shall not be interpreted as a time-based energy measurement.

ELE-05 — Heater Switching

The heater shall be controlled through an appropriate electronic switching stage.

The HIFLY architecture uses a MOSFET-based switching concept.

9. Environmental Protection Requirements
ENV-01 — Low-Pressure Protection

The system shall include a low-pressure protection architecture for relevant electrical and electronic components.

The protection concept uses a dedicated chamber/enclosure.

        High-Altitude Environment
                  │
                  ▼
        ┌──────────────────┐
        │ Protection       │
        │ Chamber          │
        │                  │
        │ Electronics      │
        │ Sensors          │
        │ Electrical Paths │
        └──────────────────┘
ENV-02 — Electrical Protection

The system shall address the reliability risk associated with insulation degradation and electrical arcing under low-pressure conditions.

The design response includes:

electrical insulation,
protected electrical paths,
low-pressure protection architecture,
monitoring and safety logic.
ENV-03 — Thermal Cycling Protection

The system shall incorporate flexible silicone protection for relevant interfaces exposed to thermal cycling.

ENV-04 — Lightweight Protective Layer

The system shall incorporate a lightweight protective layer as part of the environmental protection architecture.

The effectiveness of the layer against radiation exposure shall be established through appropriate evidence before a quantitative protection claim is made.

ENV-05 — Antenna Environmental Protection

The communication antenna shall include hydrophobic environmental protection.

10. Communication Requirements
COM-01 — LoRa Communication

The system shall provide a LoRa-based communication path between the HIFLY airborne system and the Ground Control Station.

The hardware architecture uses the LilyGO T3-S3 LoRa platform.

COM-02 — Telemetry Transmission

The communication system shall transmit relevant system telemetry.

Telemetry may include:

battery temperature,
ambient temperature,
voltage,
current,
heater status,
thermal status,
safety status,
communication status.
COM-03 — Communication Status

The system shall provide an indication of the communication-link state.

The communication state shall distinguish between an available link and a communication-loss condition.

COM-04 — Communication-Loss Handling

The onboard system shall continue local thermal and safety control when communication with the GCS is unavailable.

              LoRa Link
                 │
          ┌──────┴──────┐
          │             │
         OK            LOST
          │             │
          ▼             ▼
      Telemetry      Local Control
          │             │
          │             ▼
          │       Thermal Safety
          │             │
          └──────┬──────┘
                 ▼
          Normal Operation
COM-05 — Communication Restoration

When communication is restored, the system shall be capable of resuming telemetry exchange without requiring the local thermal-control architecture to have been disabled during the communication-loss period.

11. Software Requirements
SW-01 — Sensor Acquisition

The firmware shall acquire data from the relevant temperature, voltage, and current sensors.

SW-02 — Sensor Validation

The firmware shall include sensor-validity handling so that invalid sensor information can be distinguished from valid measurements.

SW-03 — Thermal State Evaluation

The firmware shall evaluate measured temperature information for thermal control and monitoring.

SW-04 — Heater Control

The firmware shall generate the control required to operate the heater switching stage.

SW-05 — Safety Logic

The firmware shall include safety logic associated with abnormal thermal, electrical, sensor, or communication states.

SW-06 — Telemetry Generation

The firmware shall package relevant system information into telemetry data for transmission.

SW-07 — Autonomous Operation

The onboard controller shall maintain thermal-control functionality independently of continuous GCS communication.

SW-08 — Operating Modes

The control architecture shall support the defined operating modes.

The principal modes are:

AUTO,
PRE-HEAT,
HEATER OFF,
manual control where supported.
12. Ground Control Station Requirements
GCS-01 — System Overview

The GCS shall provide an operator-facing view of the HIFLY system state.

GCS-02 — Battery Information

The GCS shall display battery temperature.

GCS-03 — Environmental Temperature

The GCS shall display the relevant ambient temperature measurement.

GCS-04 — Electrical Information

The GCS shall display:

voltage,
current.
GCS-05 — Thermal Information

The GCS shall display:

heater status,
thermal status.
GCS-06 — Communication Information

The GCS shall provide visibility of the LoRa communication status.

GCS-07 — Operating Mode

The GCS shall indicate the active operating mode.

The intended modes include:

AUTO,
MANUAL.
GCS-08 — Safety Status

The GCS shall display relevant system safety status.

GCS-09 — Temperature Trend

The GCS shall provide a temperature trend visualization.

Temperature
     │
     │              ╱
     │          ╱───
     │      ╱──
     │  ╱──
     └──────────────────── Time
GCS-10 — Alerts

The GCS shall support visibility of relevant alerts, including:

Low Temperature,
Over-temperature,
Sensor Fault,
Communication Lost.
GCS-11 — Thermal Controls

The GCS shall provide the defined operator controls where supported by the implemented system.

These include:

AUTO,
PRE-HEAT,
HEATER OFF,
manual heater override.
13. Safety Requirements
SAF-01 — Sensor Fault Awareness

The system shall detect or identify invalid sensor conditions where supported by the sensor implementation.

SAF-02 — Thermal Safety

The system shall include thermal safety logic associated with abnormal temperature conditions.

Exact threshold values are implementation-specific and shall be established by the applicable control design and test documentation.

SAF-03 — Communication Safety

Loss of communication shall not automatically disable onboard thermal safety logic.

SAF-04 — Heater Safety

The heater control architecture shall include a defined means of switching the heater to an appropriate safe state.

SAF-05 — Safety Status Reporting

Relevant safety states shall be available to the telemetry and GCS architecture.

14. Data Requirements
DAT-01 — Measurement Recording

Relevant measured parameters shall be recordable for analysis.

DAT-02 — Measurement Identity

Recorded data shall identify the measured quantities.

Examples include:

current,
voltage,
temperature.
DAT-03 — Data Context

Where applicable, data records shall preserve the context necessary to interpret the measurement.

This includes:

sensor identity,
measurement quantity,
operating condition,
test context,
timestamp where available.
DAT-04 — Derived Quantities

Derived quantities shall be distinguishable from directly measured quantities.

For example:

Measured:
Voltage
Current

Derived:
Power = Voltage × Current
DAT-05 — Time-Based Analysis

Time-dependent quantities shall only be derived when a valid time reference is available.

A sequential list of readings without timestamps shall not be treated as a time series.

15. CAD Requirements
CAD-01 — System Assembly

The CAD architecture shall represent the physical integration of the principal HIFLY subsystems.

CAD-02 — Battery Integration

The battery shall be represented as part of the system mechanical architecture.

CAD-03 — PHP Integration

The PHP shall have a defined physical integration within the thermal architecture.

CAD-04 — Enclosure Integration

The protection chamber/enclosure shall be represented in the physical design.

CAD-05 — Thermal Management Integration

The CAD architecture shall represent the relationship between:

heat sources,
thermal interfaces,
PHP,
insulation,
heater,
protected components.
16. Simulation Requirements
SIM-01 — Baseline Thermal Model

The thermal architecture shall support a baseline thermal evaluation without the PHP-assisted thermal path.

SIM-02 — PHP-Assisted Thermal Model

The thermal architecture shall support an evaluation of the PHP-assisted configuration.

SIM-03 — Thermal Comparison

The baseline and PHP-assisted configurations shall be capable of comparative analysis.

Potential comparison quantities include:

maximum temperature,
temperature distribution,
heat flux,
thermal gradients.
SIM-04 — Model Transparency

Simulation conclusions shall be interpreted according to the assumptions and limitations of the model.

A conventional steady-state thermal model shall not be presented as a direct simulation of the internal pulsating two-phase flow of a real PHP.

17. Testing Requirements
TST-01 — Component Testing

Relevant components shall be capable of being tested independently before integrated system testing.

TST-02 — Thermal Testing

The thermal subsystem shall be capable of experimental evaluation.

Relevant test quantities may include:

temperature,
heater state,
thermal response,
PHP-assisted thermal behaviour.
TST-03 — Electrical Testing

The electrical subsystem shall be capable of evaluation using:

voltage measurements,
current measurements,
heater switching behaviour.
TST-04 — Communication Testing

The LoRa communication subsystem shall be capable of testing for:

telemetry transmission,
communication status,
communication loss,
communication restoration.
TST-05 — Integrated Testing

The integrated HIFLY system shall be capable of testing the interaction between:

sensing,
thermal control,
battery monitoring,
heater control,
LoRa communication,
telemetry,
GCS.
TST-06 — Evidence Recording

Test observations and measurements shall be recorded in a form that allows them to be associated with the corresponding test condition.

18. Requirement Traceability Matrix
Requirement	Challenge / Function	Primary Subsystem	Verification Evidence
SYS-01	Integrated reliability	Complete HIFLY	Architecture / integration test
SYS-02	Subsystem integration	Complete HIFLY	Integration test
SYS-03	System monitoring	Sensors / MCU	Firmware / telemetry
THM-01	Thermal monitoring	Temperature sensors	Measurement data
THM-02	Thermal management	PHP / heater / insulation	Simulation / testing
THM-03	Reduced cooling efficiency	PHP	CAD / simulation / testing
THM-04	PHP evaluation	Thermal subsystem	Comparative simulation / test
THM-05	Low-temperature control	Heater	Hardware / test
THM-06	Heater control	MCU / MOSFET	Firmware / test
THM-07	Heater visibility	MCU / GCS	Telemetry / GCS
THM-08	Communication-loss thermal control	MCU	Firmware / test
BAT-01	Battery thermal behaviour	BTMS	Temperature data
BAT-02	Battery electrical monitoring	Power monitoring	Voltage/current data
BAT-03	Battery thermal management	BTMS	Design / test
BAT-04	Battery visibility	Telemetry / GCS	GCS / telemetry
ELE-01	Electrical monitoring	Voltage sensor	Electrical test
ELE-02	Electrical monitoring	Current sensor	Electrical test
ELE-03	Power estimation	MCU / telemetry	Calculation / data
ELE-04	Energy monitoring	Data system	Time-based data
ELE-05	Heater switching	MOSFET / MCU	Electrical test
ENV-01	Low-pressure reliability	Protection chamber	Environmental test
ENV-02	Electrical protection	Insulation / chamber	Electrical/environmental test
ENV-03	Thermal cycling	Silicone protection	Thermal-cycle test
ENV-04	Radiation protection	Protective layer	Material/environmental evidence
ENV-05	Antenna protection	Antenna / coating	Communication/environmental test
COM-01	Communication	LoRa	Communication test
COM-02	Telemetry	LoRa / MCU	Telemetry data
COM-03	Link status	LoRa / firmware	Communication test
COM-04	Communication loss	MCU / safety logic	Failure-mode test
COM-05	Communication recovery	LoRa / firmware	Recovery test
SW-01	Data acquisition	Firmware	Software test
SW-02	Sensor validity	Firmware	Software/fault test
SW-03	Thermal evaluation	Firmware	Software test
SW-04	Heater control	Firmware	Functional test
SW-05	Safety logic	Firmware	Fault-injection test
SW-06	Telemetry	Firmware	Communication test
SW-07	Autonomous control	Firmware	Communication-loss test
SW-08	Operating modes	Firmware / GCS	Functional test
GCS-01	System monitoring	GCS	Interface test
GCS-02	Battery temperature	GCS	Telemetry test
GCS-03	Ambient temperature	GCS	Telemetry test
GCS-04	Electrical display	GCS	Telemetry test
GCS-05	Thermal display	GCS	Telemetry test
GCS-06	Communication display	GCS	Communication test
GCS-07	Mode display	GCS	Functional test
GCS-08	Safety display	GCS	Functional test
GCS-09	Temperature trend	GCS	Interface/data test
GCS-10	Alerts	GCS	Functional/fault test
GCS-11	Thermal controls	GCS	Functional test
DAT-01	Measurement recording	Data system	Data review
DAT-02	Measurement identity	Data system	Data review
DAT-03	Data context	Data system	Data review
DAT-04	Derived values	Data system	Calculation review
DAT-05	Time-based analysis	Data system	Timestamp verification
CAD-01	System assembly	CAD	Design review
CAD-02	Battery integration	CAD	Design review
CAD-03	PHP integration	CAD	Design review
CAD-04	Enclosure	CAD	Design review
CAD-05	Thermal integration	CAD	Design review
SIM-01	Baseline thermal model	Simulation	Simulation result
SIM-02	PHP-assisted model	Simulation	Simulation result
SIM-03	Thermal comparison	Simulation	Comparative result
SIM-04	Model validity	Simulation	Model review
TST-01	Component verification	Test system	Component tests
TST-02	Thermal verification	Thermal subsystem	Thermal tests
TST-03	Electrical verification	Electrical subsystem	Electrical tests
TST-04	Communication verification	LoRa / GCS	Communication tests
TST-05	Integration verification	Complete HIFLY	Integrated test
TST-06	Evidence traceability	Test system	Test records
19. Seven-Challenge Traceability
CH-01 — Reduced Cooling Efficiency
Reduced Cooling Efficiency
          │
          ▼
     THM-02 / THM-03
          │
          ▼
     PHP Architecture
          │
          ▼
 Thermal Simulation / Testing

Primary engineering element:

Pulsating Heat Pipe

CH-02 — Insulation Breakdown and Electrical Arcing
Reduced-Pressure Electrical Risk
             │
             ▼
          ENV-01
             │
             ▼
    Protection Chamber
             │
             ▼
      Electrical Protection
             │
             ▼
          Testing

Primary engineering elements:

low-pressure protection chamber,
electrical insulation,
protected electrical paths.
CH-03 — Battery Degradation
Battery Thermal Risk
        │
        ▼
     BAT-01
        │
        ▼
Battery Temperature Monitoring
        │
        ▼
     BAT-03
        │
        ▼
Battery Thermal Management

Primary engineering element:

Smart Battery Thermal Management System

CH-04 — Thermal Cycling Damage
Thermal Cycling
      │
      ▼
   ENV-03
      │
      ▼
Flexible Silicone Protection
      │
      ▼
 Thermal-Cycle Testing

Primary engineering element:

Flexible silicone protection

CH-05 — Increased Radiation Exposure
Radiation Exposure
       │
       ▼
    ENV-04
       │
       ▼
Lightweight Protective Layer
       │
       ▼
Material / Environmental Evidence

Primary engineering element:

Lightweight protective layer

CH-06 — Communication-System Effects
Communication Environment
          │
          ▼
       COM-01
          │
          ▼
       LoRa Link
          │
          ▼
Hydrophobic Antenna Protection
          │
          ▼
       Telemetry
          │
          ▼
          GCS

Primary engineering elements:

LilyGO T3-S3 LoRa,
hydrophobic antenna protection,
telemetry,
GCS.
CH-07 — Mission Energy and Endurance
Mission Energy
      │
      ▼
Voltage / Current Monitoring
      │
      ▼
Power Estimation
      │
      ▼
Energy Monitoring
      │
      ▼
Thermal-Energy Management

Primary engineering elements:

voltage monitoring,
current monitoring,
power monitoring,
thermal management,
energy monitoring.
20. Requirement Verification Status

Requirement status is interpreted using the following categories:

Status	Meaning
Concept	Requirement or response exists at conceptual level
Design	Architecture or implementation is defined
Simulated	Requirement-related behaviour has been evaluated through simulation
Prototype	Physical implementation exists
Tested	A corresponding test has been performed
Validated	Evidence demonstrates satisfaction of the applicable requirement
Planned	Verification activity is identified but not yet completed

A requirement shall not be marked Validated merely because the corresponding hardware or software has been designed.

21. Verification Methods

HIFLY uses multiple verification methods.

                 REQUIREMENT
                      │
        ┌─────────────┼─────────────┐
        │             │             │
        ▼             ▼             ▼
     Analysis      Inspection      Test
        │             │             │
        └─────────────┼─────────────┘
                      ▼
                 Evidence
                      │
                      ▼
              Verification Status
Analysis

Used for calculations, engineering relationships, and simulation interpretation.

Inspection

Used for reviewing:

CAD,
schematics,
software architecture,
documentation,
physical assembly.
Test

Used for measured system behaviour under defined conditions.

22. Requirement Boundaries

The requirements intentionally avoid assigning unsupported numerical values.

The following are not specified here without corresponding engineering evidence:

maximum operating altitude,
minimum operating temperature,
maximum operating temperature,
heater power,
battery capacity,
system mass,
LoRa range,
thermal resistance,
PHP heat-transfer rate,
radiation attenuation,
endurance,
pressure rating.

Such values should be associated with the appropriate component specifications, design calculations, simulations, or experimental results when established.

23. Requirements and Evidence Relationship

The final relationship between requirements and technical evidence is:

                 REQUIREMENTS
                      │
                      ▼
                    DESIGN
                      │
       ┌──────────────┼──────────────┐
       │              │              │
       ▼              ▼              ▼
    Hardware       Software          CAD
       │              │              │
       └──────────────┼──────────────┘
                      ▼
                  Simulation
                      │
                      ▼
                   Testing
                      │
                      ▼
                     Data
                      │
                      ▼
                   Evidence
                      │
                      ▼
                 Verification

This structure ensures that a documented requirement can be followed through the engineering lifecycle.

24. System Requirement Summary

HIFLY system requirements can be summarized as:

┌──────────────────────────────────────────────────────┐
│                  HIFLY REQUIREMENTS                  │
├──────────────────────────────────────────────────────┤
│ 1. Thermal Monitoring                                │
│ 2. PHP-Based Thermal Management                      │
│ 3. Active Heater Control                             │
│ 4. Battery Thermal Management                        │
│ 5. Voltage and Current Monitoring                    │
│ 6. Low-Pressure Protection                           │
│ 7. Thermal-Cycling Protection                        │
│ 8. Lightweight Environmental Protection              │
│ 9. Hydrophobic Antenna Protection                    │
│10. LoRa Communication                                │
│11. Autonomous Thermal Control                        │
│12. Communication-Loss Safety                         │
│13. Telemetry                                         │
│14. Ground Control Station Monitoring                 │
│15. Safety and Fault Handling                         │
│16. Data Recording and Traceability                   │
│17. Simulation-Based Thermal Evaluation               │
│18. Component and Integrated Testing                  │
└──────────────────────────────────────────────────────┘
25. Overall Requirement Architecture
                         HIFLY
                           │
                           ▼
                SYSTEM REQUIREMENTS
                           │
      ┌────────────────────┼────────────────────┐
      │                    │                    │
      ▼                    ▼                    ▼
   THERMAL             ELECTRICAL         COMMUNICATION
      │                    │                    │
      ▼                    ▼                    ▼
 PHP / Heater        Battery / V / I       LoRa / Antenna
      │                    │                    │
      └────────────────────┼────────────────────┘
                           ▼
                       SOFTWARE
                           │
                           ▼
                     SAFETY LOGIC
                           │
                           ▼
                         GCS
                           │
                           ▼
                       TESTING
                           │
                           ▼
                       EVIDENCE
                           │
                           ▼
                    VERIFICATION
Requirements Status

System Requirements: Defined
Thermal Requirements: Defined
Battery Requirements: Defined
Electrical Requirements: Defined
Environmental Protection Requirements: Defined
Communication Requirements: Defined
Software Requirements: Defined
GCS Requirements: Defined
Safety Requirements: Defined
Data Requirements: Defined
CAD Requirements: Defined
Simulation Requirements: Defined
Testing Requirements: Defined
Traceability Matrix: Defined

Overall Requirements Status: Defined
