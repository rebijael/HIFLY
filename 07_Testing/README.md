# HIFLY — Testing and Validation

## 1. Purpose

The `07_Testing/` directory defines the physical testing and validation framework for the HIFLY Integrated High-Altitude UAV Reliability System.

Testing connects the HIFLY design and simulation work with physical evidence obtained from:

- prototype hardware
- thermal experiments
- battery measurements
- electrical measurements
- communication tests
- environmental testing
- control-system testing
- protection-system testing

The testing framework is designed to distinguish clearly between:

- design
- simulation
- prototype implementation
- physical testing
- validation

No simulation result is treated as a physical test result, and no physical test is treated as model validation unless an appropriate simulation-to-test comparison has been performed.

---

# 2. Testing Objectives

The HIFLY testing programme focuses on the seven high-altitude reliability challenges addressed by the system.

| Challenge | Testing focus |
|---|---|
| Reduced cooling efficiency | PHP-assisted thermal transfer |
| Insulation breakdown and electrical arcing | Protection chamber and insulation behaviour |
| Battery degradation | Battery thermal management |
| Thermal cycling damage | Flexible silicone protection and repeated thermal exposure |
| Increased radiation exposure | Protective-layer concept and material evaluation |
| Communication effects | Antenna protection and LoRa communication |
| Mission energy and endurance | Electrical power and energy monitoring |

The testing programme is intended to establish evidence for the behaviour of the implemented system under defined test conditions.

---

# 3. Testing Architecture

```text
                         HIFLY DESIGN
                              │
                              ▼
                       Simulation Model
                              │
                              ▼
                       Physical Prototype
                              │
              ┌───────────────┼────────────────┐
              │               │                │
              ▼               ▼                ▼
          Thermal         Electrical      Communication
           Tests             Tests             Tests
              │               │                │
              └───────────────┼────────────────┘
                              │
                              ▼
                       Control-System Tests
                              │
                              ▼
                       Protection Tests
                              │
                              ▼
                        Test Data
                              │
                              ▼
                     Result Evaluation
                              │
                              ▼
                    Simulation Correlation
                              │
                              ▼
                         Validation
```
4. Testing Philosophy

HIFLY testing follows a progressive approach.

Concept
   │
   ▼
Design
   │
   ▼
Simulation
   │
   ▼
Prototype
   │
   ▼
Component Testing
   │
   ▼
Subsystem Testing
   │
   ▼
Integrated Testing
   │
   ▼
Environmental Testing
   │
   ▼
System Validation

Testing should progress from controlled component-level experiments toward integrated system evaluation.

This allows individual thermal, electrical, communication, and control functions to be evaluated before the complete system is tested.

5. Evidence Classification

The project uses the following evidence states:

State	Meaning
Concept	Proposed engineering concept
Design	Architecture or component design defined
Simulated	Behaviour evaluated through simulation
Prototype	Physical implementation available
Tested	Physical test performed
Validated	Model or requirement supported by appropriate evidence

The classification prevents simulated behaviour from being presented as experimentally verified performance.

6. Test Levels

Testing can be divided into four primary levels.

6.1 Component Testing

Individual components are evaluated independently.

Examples include:

temperature sensors
voltage sensors
current sensors
heater
MOSFET switching stage
LoRa module
antenna
battery
insulation material
PHP
6.2 Subsystem Testing

Multiple components are combined into a functional subsystem.

Examples include:

battery thermal-management subsystem
thermal-control subsystem
LoRa communication subsystem
protection chamber
power-monitoring subsystem
6.3 Integrated System Testing

The complete HIFLY architecture is operated as a combined system.

Battery
   │
   ├──► Power Monitoring
   │
   ├──► Heater
   │
   └──► Electronics
          │
          ├──► Temperature Monitoring
          │
          ├──► Thermal Control
          │
          └──► LoRa Communication
                       │
                       ▼
                    GCS
6.4 Environmental Testing

Environmental testing evaluates system behaviour under conditions intended to represent relevant high-altitude effects.

Potential environmental factors include:

low temperature
reduced pressure
thermal cycling
environmental exposure
communication effects
thermal gradients

The actual environmental test conditions depend on the available test equipment and defined test case.

7. Thermal Testing

Thermal testing is a major part of the HIFLY validation programme.

The thermal tests evaluate:

component temperature
battery temperature
heater response
thermal insulation
PHP-assisted heat transfer
enclosure thermal behaviour
thermal gradients
response to changing operating conditions

A conceptual thermal test arrangement is:

             THERMAL TEST SETUP
                     │
        ┌────────────┼────────────┐
        │            │            │
        ▼            ▼            ▼
   Temperature    Voltage       Current
     Sensors       Sensor        Sensor
        │            │            │
        └────────────┼────────────┘
                     │
                     ▼
               Data Acquisition
                     │
                     ▼
                 Test Data
8. Temperature Measurement

Temperature measurements provide direct physical evidence of thermal behaviour.

Measurement locations may include:

battery surface
electronics region
processor region
heater region
PHP evaporator region
PHP condenser region
enclosure
external environment

Sensor locations should correspond to physically meaningful regions of the HIFLY thermal architecture.

9. Electrical Testing

Electrical testing measures the operating condition of the HIFLY power system.

Primary electrical variables include:

voltage
current
instantaneous power
heater state
electrical load

Instantaneous electrical power is calculated as:

$$ P = V I $$

where:

\(P\) is electrical power
\(V\) is voltage
\(I\) is current

Electrical measurements should be recorded together with the corresponding thermal condition whenever thermal behaviour is being evaluated.

10. Existing Measurement Dataset

The project contains the following measured current, voltage, and temperature values:

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

This dataset provides an existing experimental reference for electrical and temperature behaviour.

The dataset does not contain a time column. Therefore, the measurements are treated as sequential readings rather than a time-history unless the original experiment establishes the sampling interval.

11. Existing Power Calculation

For the recorded operating point:

Voltage = 12.0 V
Current = 1.54 A

the corresponding instantaneous electrical power is:

$$ P = VI $$ $$ P = 12.0 \times 1.54 $$ $$ P = 18.48\text{ W} $$

This is an instantaneous electrical power calculation.

It is not a measurement of:

total energy consumption
mission endurance
battery capacity
average power
thermal power deposited into a particular component

without additional measurements or assumptions.

12. Battery Thermal Testing

Battery thermal testing evaluates the temperature behaviour of the battery under defined electrical operating conditions.

The test may examine:

battery temperature
current
voltage
heater state
surrounding temperature
temperature change
thermal response to load

Conceptually:

                Electrical Load
                      │
                      ▼
               ┌─────────────┐
               │   Battery   │
               └──────┬──────┘
                      │
             ┌────────┴────────┐
             │                 │
             ▼                 ▼
       Electrical data    Thermal data
             │                 │
             └────────┬────────┘
                      ▼
                 Test record
13. Heater Testing

The heater is tested as an active thermal-management element.

Testing can evaluate:

heater switching
heater electrical operation
temperature response
thermal distribution
control-system response
heater interaction with insulation
heater interaction with the battery

The heater control path is:

Temperature Sensor
       │
       ▼
Control Logic
       │
       ▼
MOSFET Driver
       │
       ▼
Heater
       │
       ▼
Thermal Response
       │
       ▼
Temperature Sensor

This creates a closed thermal-control loop.

14. Thermal-Control Testing

The thermal-control system should be evaluated under defined temperature conditions.

The control system includes:

temperature acquisition
decision logic
heater control
safety logic
system-state reporting

A simplified test sequence is:

Temperature Measurement
          │
          ▼
     Control Logic
          │
    ┌─────┴─────┐
    │           │
    ▼           ▼
Heating       No heating
required      required
    │           │
    ▼           ▼
Heater ON    Heater OFF
    │           │
    └─────┬─────┘
          ▼
   Updated Temperature
          │
          ▼
   Next Measurement

The actual control thresholds are determined by the implemented control specification.

15. PHP Testing

The Pulsating Heat Pipe is tested as the passive thermal-management component of HIFLY.

Testing can compare thermal behaviour between:

REFERENCE CONFIGURATION
        │
        ▼
Heat source
        │
        ▼
Conventional thermal path
        │
        ▼
Heat rejection


PHP-ASSISTED CONFIGURATION
        │
        ▼
Heat source
        │
        ▼
PHP evaporator
        │
        ▼
PHP thermal path
        │
        ▼
PHP condenser
        │
        ▼
Heat rejection

Relevant measurements include:

source-region temperature
PHP evaporator temperature
PHP condenser temperature
surrounding temperature
electrical operating condition

A PHP test should compare equivalent operating conditions wherever the objective is to evaluate the effect of the PHP.

16. PHP Test Interpretation

A physical PHP test provides experimental evidence of the thermal behaviour of the implemented PHP.

The test does not automatically validate a simulation model.

Simulation correlation requires comparison between:

simulated temperature
measured temperature
equivalent geometry
equivalent heat input
comparable boundary conditions
comparable sensor locations
17. Thermal Cycling Testing

Thermal cycling evaluates the response of the system to repeated changes between defined thermal conditions.

The test can examine:

temperature cycling
enclosure response
flexible silicone protection
insulation behaviour
thermal-interface stability
repeated heater operation
sensor behaviour

Conceptually:

Temperature
    │
    │       ┌───────┐       ┌───────┐
    │       │       │       │       │
    │───────┘       └───────┘       └────
    │
    └──────────────────────────────────── Time

The actual temperature range and cycle duration must be associated with the defined test configuration.

18. Flexible Silicone Testing

Flexible silicone is evaluated as a protective element under thermal and environmental conditions relevant to the HIFLY design.

Testing may examine:

physical integrity
attachment
flexibility
thermal exposure
interface behaviour
protection of enclosed components

The test should distinguish material protection behaviour from any separate thermal-insulation claim.

19. Insulation Testing

Insulation testing evaluates the effect of the selected insulation arrangement on thermal transfer.

Measurements may include:

protected-region temperature
external temperature
internal temperature
heater response
thermal gradient

The insulation comparison should use equivalent operating conditions where possible.

WITHOUT INSULATION
Heat source
     │
     ▼
Structure
     │
     ▼
Environment


WITH INSULATION
Heat source
     │
     ▼
Insulation
     │
     ▼
Structure / enclosure
     │
     ▼
Environment
20. Low-Pressure Protection Testing

The low-pressure protection chamber is associated with protection against reduced-pressure environmental effects.

Testing may examine:

enclosure integrity
component operation
electrical behaviour
thermal behaviour
insulation behaviour
system operation under the selected pressure condition

The test should distinguish between a chamber-level pressure test and a complete high-altitude environmental qualification.

21. Electrical Protection Testing

Electrical protection testing can evaluate:

insulation behaviour
electrical isolation
power-system operation
heater switching
sensor operation
behaviour of protected electronics

The testing should be conducted using appropriate laboratory safety procedures and equipment suitable for the voltage and energy levels involved.

22. Communication Testing

The HIFLY communication subsystem uses LoRa communication through the LilyGO T3-S3 platform.

Communication testing can evaluate:

telemetry transmission
command reception
link continuity
communication-loss detection
recovery after link restoration
antenna protection effects

A simplified test architecture is:

        HIFLY UNIT
            │
            ▼
       LilyGO T3-S3
            │
            │ LoRa
            ▼
       Ground Station
            │
            ▼
        Telemetry
            │
            ▼
       Operator View
23. Communication-Loss Test

Communication-loss testing verifies that loss of the ground link does not unnecessarily terminate onboard thermal-control capability.

The intended behaviour is:

Normal communication
        │
        ▼
     GCS control
        │
        ▼
Communication lost
        │
        ▼
Onboard autonomous control
        │
        ▼
Safety logic
        │
        ▼
Thermal management continues
        │
        ▼
Communication restored
        │
        ▼
Normal communication

The exact transition behaviour depends on the implemented firmware.

24. Ground Control Station Testing

The GCS is tested for correct monitoring and control of the onboard system.

Relevant displayed information includes:

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
alerts

Potential control states include:

AUTO
PRE-HEAT
HEATER OFF
manual override

GCS testing should verify that displayed information corresponds to the onboard state.

25. Sensor Testing

Sensor testing evaluates:

sensor response
measurement continuity
electrical connection
communication with the controller
fault detection
consistency between sensors where applicable

A sensor-fault condition should be treated as a system state rather than silently ignored.

Conceptually:

Sensor
  │
  ▼
Measurement
  │
  ▼
Validity Check
  │
 ┌┴─────────────┐
 ▼              ▼
Valid          Fault
 │              │
 ▼              ▼
Control       Safety Logic
26. Power Monitoring Testing

The power-monitoring system measures electrical operating conditions.

The test verifies the relationship between:

actual electrical input
sensor reading
firmware interpretation
GCS display

The basic relationship is:

$$ P = VI $$

Power-monitoring tests should include multiple operating conditions where practical.

27. Data Acquisition

Test data should preserve the relationship between measurements.

A typical record structure is:

Parameter	Example role
Reading index	Identifies sequential measurement
Temperature	Thermal condition
Voltage	Electrical condition
Current	Electrical load
Heater state	Thermal-management state
Communication state	Link condition
Control state	AUTO/MANUAL/etc.

If a time source is available, timestamps should be used instead of relying only on reading order.

28. Test Data Flow
              HIFLY HARDWARE
                    │
          ┌─────────┼─────────┐
          │         │         │
          ▼         ▼         ▼
      Sensors    Electrical   LoRa
          │       Monitor      │
          │         │          │
          └─────────┼──────────┘
                    ▼
                ESP32
                    │
                    ▼
             Data Acquisition
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
       Local Data            GCS
          │                   │
          └─────────┬─────────┘
                    ▼
               Test Dataset
                    │
                    ▼
              Data Analysis
29. Test Repeatability

Repeated tests can be used to evaluate whether a measured response is consistent under similar conditions.

Repeatability depends on controlling or recording:

initial temperature
electrical operating condition
environmental condition
component configuration
test duration
sensor placement
heater state

Differences between repeated tests should be retained rather than removed without explanation.

30. Test Uncertainty

Physical measurements contain uncertainty.

Potential sources include:

sensor accuracy
sensor placement
electrical measurement accuracy
environmental variation
thermal-contact variation
component variation
data-acquisition resolution
test setup variation

Where uncertainty information is available, it should be retained with the corresponding measurement.

31. Simulation Correlation

Physical test results can be compared with simulation results when the conditions are comparable.

               SIMULATION
                   │
                   ▼
          Predicted temperature
                   │
                   │
                   ▼
              COMPARISON
                   ▲
                   │
                   │
           Measured temperature
                   │
                   ▲
                 TEST

Correlation should consider:

same heat input
same geometry
same thermal interfaces
same environmental conditions
same measurement location
same operating state
32. Validation

Validation is achieved through evidence-based comparison.

A validation statement should identify:

what was simulated or predicted
what was physically measured
under what conditions
where the measurement was taken
how the results compare
what limitations remain

Validation should not be claimed merely because a prototype was successfully powered or operated.

33. Test Safety

Testing involves electrical, thermal, battery, pressure, and environmental hazards.

Appropriate laboratory procedures should be followed for:

lithium-ion battery handling
electrical connections
heater operation
thermal exposure
low-pressure testing
high-current conditions
enclosure testing
component overheating

Testing should be stopped if the hardware enters an unsafe or uncontrolled state.

34. Test Record Structure

Each completed test should have an associated record containing the available information for that test.

A test record can follow the structure:

Test Identification
        │
        ├── Test objective
        ├── Hardware configuration
        ├── Test setup
        ├── Operating condition
        ├── Sensors used
        ├── Measurements
        ├── Observations
        ├── Result
        └── Evidence classification

The record should preserve the distinction between measured observations and engineering interpretation.

35. Test Result Interpretation

A test result should be interpreted only within the conditions under which it was obtained.

For example:

Measured temperature
        │
        ▼
Specific hardware configuration
        │
        ▼
Specific electrical condition
        │
        ▼
Specific environmental condition
        │
        ▼
Test result

A result from one operating condition should not automatically be generalized to all mission conditions.

36. Integrated Testing

Integrated testing combines the major HIFLY subsystems.

                  ┌───────────────┐
                  │    Battery    │
                  └───────┬───────┘
                          │
              ┌───────────┼───────────┐
              │           │           │
              ▼           ▼           ▼
           Heater     Electronics   Sensors
              │           │           │
              └───────────┼───────────┘
                          │
                          ▼
                        ESP32
                          │
                 ┌────────┴────────┐
                 │                 │
                 ▼                 ▼
               LoRa              Safety
                 │                 │
                 ▼                 │
                GCS ◄──────────────┘

Integrated testing evaluates whether the subsystems operate together as intended.

37. End-to-End Test Flow
Power ON
   │
   ▼
Sensor Initialization
   │
   ▼
System Health Check
   │
   ▼
Thermal Monitoring
   │
   ▼
Power Monitoring
   │
   ▼
Thermal Control
   │
   ▼
LoRa Telemetry
   │
   ▼
GCS Monitoring
   │
   ▼
Communication-Loss Test
   │
   ▼
Autonomous Safety Behaviour
   │
   ▼
Communication Recovery
   │
   ▼
Test Data Review
38. Testing and the Seven Subproblems

The testing programme maps directly to the HIFLY problem architecture.

1. Reduced Cooling Efficiency
            │
            ▼
        PHP Testing

2. Insulation Breakdown / Arcing
            │
            ▼
 Protection and Insulation Testing

3. Battery Degradation
            │
            ▼
     Battery Thermal Testing

4. Thermal Cycling Damage
            │
            ▼
       Thermal Cycle Testing

5. Radiation Exposure
            │
            ▼
 Protective Layer Evaluation

6. Communication Effects
            │
            ▼
    LoRa / Antenna Testing

7. Energy and Endurance
            │
            ▼
 Power and Energy Monitoring
39. Testing-to-Requirement Traceability

Testing provides evidence for the corresponding system requirements.

Problem
   │
   ▼
Requirement
   │
   ▼
Design Feature
   │
   ▼
Simulation
   │
   ▼
Physical Test
   │
   ▼
Measured Evidence
   │
   ▼
Validation Status

This structure allows each major HIFLY design feature to be connected to measurable evidence.

40. Testing Status

The repository distinguishes testing status from design status.

Concept
   │
   ▼
Designed
   │
   ▼
Prototype
   │
   ▼
Tested
   │
   ▼
Validated

A component may therefore be:

designed but not tested
prototyped but not validated
tested but not correlated with simulation
simulated but not physically tested

These states should not be merged into a single performance claim.

41. Overall Testing Framework
                         HIFLY
                           │
                           ▼
                    System Requirements
                           │
                           ▼
                     Design Features
                           │
             ┌─────────────┼─────────────┐
             │             │             │
             ▼             ▼             ▼
          Thermal       Electrical    Communication
           System          System         System
             │             │             │
             └─────────────┼─────────────┘
                           │
                           ▼
                         Testing
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ▼                  ▼                  ▼
   Component           Subsystem          Integrated
      Tests              Tests               Tests
        │                  │                  │
        └──────────────────┼──────────────────┘
                           │
                           ▼
                    Environmental Tests
                           │
                           ▼
                      Test Data
                           │
                           ▼
                   Result Evaluation
                           │
                           ▼
                 Simulation Correlation
                           │
                           ▼
                       Validation
42. Status

Testing Framework Status: Defined

The testing framework establishes the structure for physical evaluation of the HIFLY thermal, electrical, communication, protection, and control systems.
