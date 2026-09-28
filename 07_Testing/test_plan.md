# HIFLY — Test Plan

## 1. Purpose

This test plan defines the physical evaluation approach for the HIFLY Integrated High-Altitude UAV Reliability System.

The plan connects the HIFLY architecture with measurable physical evidence across:

- thermal management
- battery thermal behaviour
- heater operation
- electrical monitoring
- insulation
- thermal cycling
- protection systems
- LoRa communication
- ground-control monitoring
- autonomous fail-safe behaviour
- simulation correlation

The test plan is structured so that component-level behaviour can be established before integrated system behaviour is evaluated.

---

# 2. Test Philosophy

HIFLY testing follows a progressive verification path.

```text
                    HIFLY DESIGN
                         │
                         ▼
                   COMPONENT TEST
                         │
                         ▼
                  SUBSYSTEM TEST
                         │
                         ▼
                INTEGRATED SYSTEM TEST
                         │
                         ▼
                ENVIRONMENTAL TEST
                         │
                         ▼
                  DATA ANALYSIS
                         │
                         ▼
               SIMULATION CORRELATION
                         │
                         ▼
                     VALIDATION
``` 
The test plan does not assume that successful operation of one subsystem establishes complete system validation.

3. Test Objectives

The primary objectives are to evaluate whether the implemented HIFLY architecture behaves as intended under defined test conditions.

The testing programme addresses:

thermal sensing
electrical monitoring
heater operation
battery thermal behaviour
PHP-assisted heat transfer
insulation behaviour
thermal cycling
low-pressure protection
communication operation
communication-loss response
GCS monitoring and control
integrated system operation
simulation-to-test comparison
4. Test Levels

Testing is divided into four levels.

Level	Description
Component	Individual hardware or material evaluation
Subsystem	Multiple components operating together
Integrated	Complete HIFLY system operation
Environmental	Evaluation under selected environmental conditions

The progression allows faults or unexpected behaviour to be isolated before integrated testing.

5. Test Matrix
Test ID	Test Area	Primary Evidence
T-01	Temperature sensing	Sensor readings
T-02	Voltage measurement	Measured voltage
T-03	Current measurement	Measured current
T-04	Heater switching	Heater electrical and thermal response
T-05	Thermal control	Control response
T-06	Battery thermal behaviour	Battery temperature and electrical condition
T-07	PHP thermal behaviour	Thermal measurements
T-08	Insulation	Temperature comparison
T-09	Thermal cycling	Repeated thermal exposure response
T-10	Flexible silicone protection	Physical and thermal condition
T-11	Low-pressure protection	System behaviour under defined pressure condition
T-12	LoRa communication	Telemetry and link behaviour
T-13	Communication loss	Autonomous onboard response
T-14	GCS	Monitoring and command behaviour
T-15	Integrated system	Combined subsystem operation
T-16	Simulation correlation	Simulation versus measurement
6. T-01 — Temperature Sensor Test
Objective

Verify that the temperature-monitoring system produces usable measurements at the intended monitoring locations.

Test Configuration
Temperature Source / Test Environment
                │
                ▼
          Temperature Sensor
                │
                ▼
             ESP32
                │
                ▼
        Recorded Temperature
Measurements

The test records:

sensor reading
measurement location
corresponding test condition
controller state
Evidence

The result establishes sensor operation under the tested condition.

It does not establish the accuracy of the complete thermal model.

7. T-02 — Voltage Measurement Test
Objective

Verify the voltage-monitoring path used by the HIFLY power-monitoring system.

Test Configuration
Electrical Source
       │
       ▼
Voltage Measurement
       │
       ▼
     ESP32
       │
       ▼
 Recorded Voltage
Measurements

The measured voltage is compared with the reference electrical measurement used by the test setup.

Result

The test establishes whether the voltage-monitoring subsystem operates consistently under the tested electrical condition.

8. T-03 — Current Measurement Test
Objective

Verify the current-monitoring path.

Test Configuration
Battery / Power Source
          │
          ▼
       Load
          │
          ▼
 Current Measurement
          │
          ▼
        ESP32
          │
          ▼
   Recorded Current
Measurements

The test records:

current
voltage
load condition
system state

The measured current can subsequently be used with voltage to calculate instantaneous electrical power.

9. T-04 — Heater Switching Test
Objective

Verify electrical switching of the heater through the control electronics.

Test Configuration
ESP32
  │
  ▼
Control Signal
  │
  ▼
MOSFET Driver
  │
  ▼
Heater
  │
  ▼
Thermal Response
Test States

The heater should be evaluated in its applicable control states, including:

OFF
ON
controlled operation

The exact control thresholds are defined by the implemented firmware and control design.

Evidence

The test records:

heater command
heater electrical state
temperature response
controller state
10. T-05 — Thermal-Control Test
Objective

Verify that the onboard thermal-control logic responds to temperature measurements.

Control Flow
Temperature Sensor
        │
        ▼
   Sensor Reading
        │
        ▼
   Control Decision
        │
   ┌────┴────┐
   │         │
   ▼         ▼
 Heater ON  Heater OFF
   │         │
   └────┬────┘
        ▼
 Temperature Change
        │
        ▼
 Next Control Cycle
Test Evidence

The test should establish the relationship between:

measured temperature
control decision
heater state
resulting temperature response
11. T-06 — Battery Thermal Test
Objective

Evaluate the thermal behaviour of the battery under a defined electrical operating condition.

Test Configuration
             Battery
                │
       ┌────────┼────────┐
       │        │        │
       ▼        ▼        ▼
   Voltage    Current  Temperature
       │        │        │
       └────────┼────────┘
                ▼
           Data Record
Measurements

The test may record:

battery temperature
voltage
current
ambient temperature
heater state
control state
Existing Reference Data

The project contains measured values:

Current	Voltage	Temperature
0	0	16
0.6	5.3	17
0.8	7.6	21
1.26	10.1	25
1.54	12	28
1.23	10.1	33
0.86	7.1	34
0.6	5.3	35
0	0	36

The dataset does not include an explicit time column, so it is treated as a sequence of measurements rather than a time-history.

12. T-07 — PHP Thermal Test
Objective

Evaluate the physical thermal behaviour of the Pulsating Heat Pipe used in the HIFLY thermal architecture.

Test Arrangement
                 Heat Source
                     │
                     ▼
              PHP Evaporator
                     │
                     ▼
             Pulsating Heat Pipe
                     │
                     ▼
              PHP Condenser
                     │
                     ▼
               Heat Rejection
Measurements

Relevant measurement locations include:

heat-source region
evaporator region
condenser region
surrounding environment
Comparison

Where applicable, the PHP-assisted configuration can be compared with a corresponding reference configuration.

The comparison should use equivalent electrical and environmental conditions.

13. T-08 — Insulation Test
Objective

Evaluate the thermal effect of the selected insulation arrangement.

Test Arrangement
REFERENCE
Heat Source
    │
    ▼
Structure
    │
    ▼
Environment


INSULATED
Heat Source
    │
    ▼
Insulation
    │
    ▼
Structure
    │
    ▼
Environment
Measurements

Measurements may include:

internal temperature
external temperature
ambient temperature
heater state
electrical operating condition

The result is interpreted as a comparison of the tested configurations rather than a universal insulation-performance claim.

14. T-09 — Thermal Cycling Test
Objective

Evaluate the response of the system and selected protective materials to repeated changes in thermal condition.

Test Arrangement
Temperature
    │
    │      ┌───────┐       ┌───────┐
    │      │       │       │       │
    │──────┘       └───────┘       └─────
    │
    └──────────────────────────────── Time
Evaluation Areas

The test can evaluate:

thermal response
repeated temperature exposure
flexible silicone condition
insulation condition
thermal interfaces
sensor behaviour
enclosure condition

The actual temperature range and cycle duration are defined by the selected test configuration.

15. T-10 — Flexible Silicone Protection Test
Objective

Evaluate the physical and thermal behaviour of flexible silicone used as a protective layer.

Evaluation Areas

The test may examine:

physical integrity
attachment
flexibility
thermal exposure
protection of enclosed components
interface condition

The protection function is evaluated separately from any claim that the silicone provides a specific thermal performance.

16. T-11 — Low-Pressure Protection Test
Objective

Evaluate the behaviour of the low-pressure protection architecture under a defined reduced-pressure condition.

Test Arrangement
            Test Chamber
        ┌──────────────────┐
        │                  │
        │   HIFLY System   │
        │                  │
        │  Electronics     │
        │  Protection      │
        │  Thermal System  │
        │                  │
        └────────┬─────────┘
                 │
                 ▼
          Pressure Control
Measurements

Depending on the test setup, measurements may include:

system operation
electrical behaviour
temperature
sensor operation
enclosure condition
protection-system behaviour

A chamber test under reduced pressure does not automatically constitute complete high-altitude qualification.

17. T-12 — LoRa Communication Test
Objective

Evaluate the HIFLY communication path between the onboard system and the ground-control station.

Test Arrangement
        HIFLY ONBOARD UNIT
                │
                ▼
           LilyGO T3-S3
                │
                │ LoRa
                ▼
          Ground Station
                │
                ▼
             GCS Data
Evaluation Areas

The test may examine:

telemetry transmission
command reception
communication state
data continuity
communication-loss detection
recovery after communication restoration

The test does not automatically establish a specific communication range unless range testing is separately performed.

18. T-13 — Communication-Loss Test
Objective

Verify the fail-safe behaviour when the ground communication link becomes unavailable.

Test Sequence
Communication Available
          │
          ▼
      Normal Mode
          │
          ▼
 Communication Interrupted
          │
          ▼
 Communication-Loss Detection
          │
          ▼
 Autonomous Onboard Control
          │
          ▼
       Safety Logic
          │
          ▼
   Communication Restored
          │
          ▼
      Normal Operation
Evidence

The test evaluates whether onboard thermal-management logic remains available independently of the GCS link.

19. T-14 — GCS Test
Objective

Verify that the ground-control station correctly displays onboard system information and handles the supported control states.

Monitored Information

The GCS may display:

battery temperature
ambient temperature
voltage
current
heater status
thermal status
LoRa link status
AUTO/MANUAL state
safety status
temperature graph
Control States

The GCS may provide:

AUTO
PRE-HEAT
HEATER OFF
manual override

The test verifies consistency between onboard state and GCS representation.

20. T-15 — Integrated System Test
Objective

Evaluate the interaction of the major HIFLY subsystems.

Integrated Configuration
                     BATTERY
                        │
             ┌──────────┼──────────┐
             │          │          │
             ▼          ▼          ▼
          Heater     Electronics  Sensors
             │          │          │
             └──────────┼──────────┘
                        │
                        ▼
                      ESP32
                        │
             ┌──────────┼──────────┐
             │                     │
             ▼                     ▼
            LoRa                 Safety
             │
             ▼
            GCS
Evaluation Areas

The integrated test evaluates:

power operation
sensor acquisition
thermal control
heater control
telemetry
GCS monitoring
fail-safe behaviour
subsystem interaction
21. T-16 — Simulation Correlation Test
Objective

Compare physical measurements with corresponding simulation predictions.

Correlation Flow
                 SIMULATION
                     │
                     ▼
             Predicted Results
                     │
                     │
                     ▼
                 COMPARISON
                     ▲
                     │
                     │
              Measured Results
                     │
                     ▲
                   TEST
Required Comparability

The comparison should use equivalent:

geometry
heat input
electrical condition
environmental condition
material definition
measurement location
thermal boundary condition

Differences should be investigated before treating them as model error.

22. Test Data Recording

Each test should preserve the relationship between the measured parameters.

A typical dataset can contain:

Field	Description
Test ID	Identifies the test
Reading index	Sequential measurement identifier
Temperature	Measured thermal condition
Voltage	Measured electrical voltage
Current	Measured electrical current
Heater state	Heater operating condition
Control state	Thermal-control operating state
Communication state	LoRa link condition
Ambient condition	Relevant environmental condition
Observation	Physical test observation

If a reliable timestamp is available, it should be recorded with the measurement.

23. Instantaneous Power

Electrical power is calculated from measured voltage and current:

$$ P = VI $$

For the available measurement:

$$ V = 12.0\text{ V} $$ $$ I = 1.54\text{ A} $$

therefore:

$$ P = 18.48\text{ W} $$

This is an instantaneous electrical power value for that measurement point.

It is not an energy or endurance measurement.

24. Test Acceptance Philosophy

Test interpretation is based on the defined objective of each test.

A test result can establish:

sensor operation
electrical measurement behaviour
heater operation
thermal response
communication behaviour
fail-safe behaviour
physical condition

A result should not be extended beyond the tested condition without supporting evidence.

25. Failure Handling

Unexpected test behaviour should be treated as engineering evidence.

Potential failure categories include:

Test Failure
     │
     ├── Sensor fault
     │
     ├── Electrical fault
     │
     ├── Thermal fault
     │
     ├── Communication fault
     │
     ├── Control fault
     │
     └── Mechanical / protection fault

The test record should preserve the observed condition and the corresponding system state.

26. Safety During Testing

HIFLY testing can involve:

lithium-ion batteries
electrical loads
heaters
elevated temperatures
low-pressure equipment
electronic switching
high-current conditions

Testing should be performed using suitable laboratory safety procedures.

The system should not be intentionally operated outside the safe operating conditions of its components solely to generate a test result.

27. Test Configuration Control

A test result is associated with a specific hardware configuration.

Relevant configuration information includes:

hardware revision
PCB revision
firmware version
CAD revision
sensor configuration
battery configuration
heater configuration
PHP configuration
insulation configuration

This prevents results from one hardware configuration from being incorrectly assigned to another.

28. Test-to-Simulation Traceability
Requirement
    │
    ▼
Design Feature
    │
    ▼
CAD Configuration
    │
    ▼
Simulation Case
    │
    ▼
Predicted Result
    │
    ▼
Physical Test
    │
    ▼
Measured Result
    │
    ▼
Comparison
    │
    ▼
Validation Status

This traceability is central to the HIFLY engineering evidence structure.

29. Test Evidence Classification
Evidence	Classification
CAD model	Design evidence
Simulation contour	Simulation evidence
Sensor measurement	Experimental evidence
Electrical measurement	Experimental evidence
Communication log	Experimental evidence
Prototype inspection	Physical evidence
Simulation/test comparison	Correlation evidence
Agreement supported by defined comparison	Validation evidence

The classification depends on the actual evidence available for the specific case.

30. Existing Experimental Evidence

The current project contains an electrical and temperature dataset:

Current, Voltage, Temperature

0,    0,    16
0.6,  5.3,  17
0.8,  7.6,  21
1.26, 10.1, 25
1.54, 12,   28
1.23, 10.1, 33
0.86, 7.1,  34
0.6,  5.3,  35
0,    0,    36

This dataset can be used as experimental evidence for the corresponding measurement condition.

Because no time field is present, the dataset should not be presented as a time-dependent test curve without additional information.

31. Test Data and GCS

The GCS provides an operational interface for observing system state.

                  ONBOARD SENSORS
                        │
                        ▼
                     ESP32
                        │
             ┌──────────┴──────────┐
             │                     │
             ▼                     ▼
       Local Control          LoRa Telemetry
             │                     │
             │                     ▼
             │                    GCS
             │                     │
             └──────────┬──────────┘
                        ▼
                   Test Record

The onboard controller remains responsible for autonomous thermal control where required.

32. Integrated Thermal Test

An integrated thermal test combines:

battery
electronics
heater
thermal insulation
PHP
temperature sensors
power monitoring

The conceptual configuration is:

                  ENVIRONMENT
                       │
                       ▼
                ┌─────────────┐
                │  Enclosure  │
                └──────┬──────┘
                       │
             ┌─────────┼─────────┐
             │         │         │
             ▼         ▼         ▼
        Electronics  Battery     PHP
             │         │         │
             │       Heater       │
             │         │          │
             └─────────┼──────────┘
                       │
                       ▼
                Sensor Network
                       │
                       ▼
                     ESP32
                       │
                       ▼
                    LoRa/GCS

The purpose is to observe the interaction between passive and active thermal-management elements.

33. Test Reporting

A completed test report should preserve:

test identification
objective
configuration
setup
measured data
observations
result
limitations
evidence classification

The report should distinguish direct measurements from calculated quantities and engineering interpretation.

34. Test Limitations

Physical testing may not reproduce the complete high-altitude flight environment.

Limitations can include:

incomplete atmospheric simulation
limited pressure range
limited radiation representation
laboratory thermal boundary conditions
simplified flight conditions
limited vibration representation
limited communication-path variation
sensor uncertainty
prototype manufacturing variation

Therefore, each result should be interpreted within the conditions of the corresponding test.

35. Overall Validation Flow
                HIFLY REQUIREMENTS
                        │
                        ▼
                   DESIGN
                        │
                        ▼
                  SIMULATION
                        │
                        ▼
                   PROTOTYPE
                        │
                        ▼
               COMPONENT TESTS
                        │
                        ▼
                SUBSYSTEM TESTS
                        │
                        ▼
               INTEGRATED TEST
                        │
                        ▼
             ENVIRONMENTAL TESTS
                        │
                        ▼
                  TEST DATA
                        │
                        ▼
             SIMULATION CORRELATION
                        │
                        ▼
                  VALIDATION
36. Status

Test Plan Status: Defined

The test plan establishes the physical-test structure for HIFLY.

Individual tests become experimentally evidenced only when the corresponding hardware has been tested and measurement records are available.
