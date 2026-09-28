# HIFLY — Data and Measurement Records

## 1. Purpose

The `08_Data/` directory contains measurement data associated with the HIFLY Integrated High-Altitude UAV Reliability System.

The data layer connects physical measurements with:

- electrical operation
- thermal behaviour
- battery behaviour
- heater operation
- sensor monitoring
- communication status
- simulation correlation
- testing and validation

The data repository distinguishes measured values from calculated values, simulation outputs, and engineering interpretation.

---

# 2. Data Architecture

```text
                    HIFLY HARDWARE
                          │
          ┌───────────────┼────────────────┐
          │               │                │
          ▼               ▼                ▼
     Temperature       Voltage          Current
       Sensors         Sensor           Sensor
          │               │                │
          └───────────────┼────────────────┘
                          ▼
                        ESP32
                          │
             ┌────────────┼────────────┐
             │            │            │
             ▼            ▼            ▼
        Local Data       LoRa       Control State
             │            │            │
             └────────────┼────────────┘
                          ▼
                    Data Records
                          │
          ┌───────────────┼────────────────┐
          │               │                │
          ▼               ▼                ▼
       Analysis       Simulation       Validation
```
3. Data Categories

HIFLY data can be divided into several categories.

Data category	Examples
Thermal	Temperature measurements
Electrical	Voltage and current
Power	Calculated instantaneous power
Control	Heater and operating state
Communication	LoRa link state and telemetry
Environmental	Ambient and test conditions
Simulation	Numerical model outputs
Validation	Simulation-to-measurement comparisons

Each category should retain its original source and context.

4. Measurement Data

Measurement data represents values obtained from physical hardware or experimental equipment.

Examples include:

temperature
voltage
current
heater state
communication state

Measured values should not be silently modified during analysis.

Any filtering, averaging, interpolation, or transformation should be identifiable as a derived data operation.

5. Existing HIFLY Measurement Dataset

The current project contains the following measured values:

Reading	Current	Voltage	Temperature
1	0	0	16
2	0.6	5.3	17
3	0.8	7.6	21
4	1.26	10.1	25
5	1.54	12	28
6	1.23	10.1	33
7	0.86	7.1	34
8	0.6	5.3	35
9	0	0	36

The reading number represents the order of the available measurements.

The dataset does not contain an explicit timestamp or sampling interval.

Therefore, the reading index should not be interpreted as elapsed time.

6. Measurement Variables
6.1 Current

Current represents the measured electrical load at the corresponding operating point.

Unit:

$$ \text{A} $$
6.2 Voltage

Voltage represents the measured electrical potential at the corresponding operating point.

Unit:

$$ \text{V} $$
6.3 Temperature

Temperature represents the measured thermal condition associated with the measurement.

The dataset records temperature values without an explicit unit declaration in the available measurement record. The original sensor configuration should therefore remain associated with the dataset when the values are used for formal reporting.

7. Data Integrity

Measurement data should preserve:

original numerical values
measurement order
measurement units where known
sensor identity where available
test configuration
operating condition
environmental condition
acquisition method

The original dataset should remain distinguishable from processed or derived datasets.

8. Raw Data and Derived Data

The repository distinguishes between raw measurements and derived quantities.

                 RAW MEASUREMENTS
                        │
          ┌─────────────┼─────────────┐
          │             │             │
          ▼             ▼             ▼
       Current        Voltage     Temperature
          │             │             │
          └─────────────┼─────────────┘
                        ▼
                DATA PROCESSING
                        │
             ┌──────────┼──────────┐
             │                     │
             ▼                     ▼
       Calculated Power       Statistical
                              Analysis
             │                     │
             └──────────┬──────────┘
                        ▼
                  Derived Data

Derived values should not replace the original measurements.

9. Electrical Power

Instantaneous electrical power can be calculated from voltage and current:

$$ P = VI $$

For the measured point:

$$ V = 12.0\text{ V} $$ $$ I = 1.54\text{ A} $$

therefore:

$$ P = 12.0 \times 1.54 $$ $$ P = 18.48\text{ W} $$

This value is a calculated instantaneous electrical power value.

It is not equivalent to total energy consumption.

10. Energy Calculation

Electrical energy requires a time component.

For a time-varying electrical load:

$$ E = \int P(t)\,dt $$

and:

$$ P(t) = V(t)I(t) $$

Therefore, the existing dataset cannot by itself establish total electrical energy because it does not contain a time axis.

A sequence of voltage and current readings without known sampling intervals is insufficient for direct energy integration.

11. Temperature Data

Temperature data is central to the HIFLY thermal-management architecture.

Temperature measurements can be used to evaluate:

battery thermal condition
electronics temperature
heater response
PHP behaviour
insulation behaviour
environmental effects
thermal-control response

The meaning of each temperature measurement depends on the sensor location and test configuration.

12. Electrical-Thermal Relationship

The measured data can be examined as a relationship between electrical operating condition and temperature.

Current ─────┐
             │
Voltage ─────┼────► Electrical Condition
             │
             ▼
        Component Load
             │
             ▼
        Heat Generation
             │
             ▼
        Temperature

This relationship is useful for identifying operating regions for further testing.

It should not be interpreted as a complete physical model of battery or electronics thermal behaviour without additional evidence.

13. Data Acquisition

The HIFLY data-acquisition path is based on onboard sensing and controller processing.

Temperature Sensors
        │
        ├──────────────┐
        │              │
Voltage Sensor         │
        │              │
Current Sensor         │
        │              │
        └──────┬───────┘
               ▼
             ESP32
               │
       ┌───────┴────────┐
       │                │
       ▼                ▼
 Local Processing      LoRa
       │                │
       ▼                ▼
 Data Record           GCS

The acquisition architecture allows electrical and thermal measurements to be considered together.

14. Data Fields

A complete HIFLY test dataset may contain fields such as:

Field	Purpose
Timestamp	Time reference
Reading index	Sequential record identifier
Temperature	Thermal measurement
Voltage	Electrical voltage
Current	Electrical current
Power	Calculated instantaneous electrical power
Heater state	Active thermal-management state
Control state	AUTO/MANUAL/etc.
Communication state	LoRa link condition
Ambient condition	Environmental reference
Test ID	Test traceability

Not every dataset is required to contain every field.

The available historical dataset contains current, voltage, and temperature.

15. Timestamp Requirement

A timestamp is required when the objective is to evaluate:

temperature rise rate
cooling rate
heater response time
thermal settling time
energy consumption
communication latency
transient behaviour

Without time information, the existing measurement sequence should not be converted into a time-history.

16. Data Quality

Data quality depends on:

sensor condition
calibration
sensor placement
electrical connection
sampling method
acquisition resolution
environmental stability
hardware configuration

Data should be reviewed for obvious acquisition faults before engineering interpretation.

A suspicious value should remain traceable to the original record rather than being silently removed.

17. Sensor Fault Data

Sensor faults are part of the HIFLY safety architecture.

A conceptual data state is:

Sensor Measurement
        │
        ▼
Validity Check
        │
   ┌────┴────┐
   │         │
   ▼         ▼
 Valid      Fault
   │         │
   ▼         ▼
Normal     Safety
Control    Handling

A sensor-fault state should be distinguishable from a genuine physical measurement.

18. Communication Data

LoRa communication can carry selected onboard data to the GCS.

Potential telemetry fields include:

temperature
voltage
current
heater status
thermal status
safety status
control mode
communication state

The telemetry dataset provides a remote representation of onboard system state.

19. GCS Data Flow
              ONBOARD SYSTEM
                    │
       ┌────────────┼────────────┐
       │            │            │
       ▼            ▼            ▼
 Temperature     Voltage       Current
       │            │            │
       └────────────┼────────────┘
                    ▼
                  ESP32
                    │
                    ▼
                  LoRa
                    │
                    ▼
                   GCS
                    │
        ┌───────────┼───────────┐
        │           │           │
        ▼           ▼           ▼
     Display      Graphs       Alerts

The GCS should preserve the distinction between live telemetry and stored test data.

20. Thermal-Control Data

Thermal-control data connects measured temperature to heater operation.

A control record may conceptually contain:

Temperature
     │
     ▼
Control State
     │
     ├────► Heater ON
     │
     └────► Heater OFF

The relationship between the temperature reading and control action is important for evaluating autonomous thermal management.

21. Communication-Loss Data

Communication state is relevant to the fail-safe architecture.

A communication event can be represented as:

Link Available
      │
      ▼
Normal Telemetry
      │
      ▼
Link Lost
      │
      ▼
Autonomous Onboard Operation
      │
      ▼
Link Restored
      │
      ▼
Normal Telemetry

A communication-loss test dataset should preserve the transition between states where those measurements are available.

22. Simulation Data

Simulation results are stored separately from experimental measurements.

Simulation data may include:

temperature fields
maximum temperature
minimum temperature
heat flux
thermal gradients
thermal distribution
transient temperature
case comparison

The distinction is:

EXPERIMENTAL DATA
       │
       └──► Measured from physical system


SIMULATION DATA
       │
       └──► Generated by numerical model

The two datasets can be compared but should not be merged without preserving their origin.

23. Simulation Correlation Dataset

A simulation-correlation dataset can contain paired values:

Location / Condition	Simulation	Measurement
Defined test point	Simulated value	Measured value

The exact values depend on the corresponding simulation case and physical test.

The comparison is meaningful only when the simulation and measurement represent comparable conditions.

24. Data Comparison

A general comparison flow is:

                 SIMULATION DATA
                       │
                       ▼
                Matching Conditions
                       │
                       │
                       ▼
                    COMPARE
                       ▲
                       │
                       │
                Matching Conditions
                       │
                       ▲
                EXPERIMENTAL DATA

The comparison should consider:

geometry
heat input
environmental condition
measurement location
material configuration
operating state
25. Data Visualization

Useful HIFLY data visualizations include:

temperature versus reading index
temperature versus time when timestamps exist
current versus voltage
power versus time when timestamps exist
temperature versus electrical power
simulation temperature versus measured temperature
thermal contour plots
heat-flux plots

A graph must use an axis appropriate to the available data.

For the current dataset, reading index is available but time is not.

26. Existing Dataset Interpretation

The existing dataset shows temperature values associated with a sequence of electrical measurements.

The sequence is:

Current / Voltage
       │
       ▼
0 / 0
       │
       ▼
0.6 / 5.3
       │
       ▼
0.8 / 7.6
       │
       ▼
1.26 / 10.1
       │
       ▼
1.54 / 12
       │
       ▼
1.23 / 10.1
       │
       ▼
0.86 / 7.1
       │
       ▼
0.6 / 5.3
       │
       ▼
0 / 0

The associated temperature values are:

16 → 17 → 21 → 25 → 28 → 33 → 34 → 35 → 36

Because there is no time information, the sequence alone does not establish a heating rate or cooling rate.

27. Data Storage Principles

HIFLY data should preserve:

original measurements
measurement context
units
test identification
hardware configuration
processing history
derived quantities
simulation or experimental origin

This provides traceability from raw measurement to final engineering conclusion.

28. Data Processing

A typical processing path is:

Raw Measurement
       │
       ▼
Data Integrity Check
       │
       ▼
Unit Verification
       │
       ▼
Data Structuring
       │
       ▼
Derived Quantities
       │
       ▼
Visualization
       │
       ▼
Engineering Analysis

Processing should not alter the original raw measurement record.

29. Derived Quantities

Derived quantities may include:

Instantaneous electrical power
$$ P = VI $$
Electrical energy
$$ E = \int P(t)\,dt $$

when time-resolved measurements are available.

Temperature difference
$$ \Delta T = T_1 - T_2 $$

where two physically defined measurement locations are available.

Derived quantities should retain the source measurements from which they were calculated.

30. Data and Testing Relationship
                  TEST PLAN
                      │
                      ▼
                 TEST SETUP
                      │
                      ▼
                DATA ACQUISITION
                      │
                      ▼
                 RAW DATA
                      │
                      ▼
                DATA ANALYSIS
                      │
                      ▼
                 TEST RESULT
                      │
                      ▼
             VALIDATION EVIDENCE

The data layer therefore provides the numerical evidence underlying the testing framework.

31. Data and Seven Subproblems

The data structure supports the seven HIFLY reliability challenges.

Subproblem	Relevant data
Reduced cooling efficiency	Thermal measurements, PHP temperatures
Insulation breakdown / arcing	Electrical and environmental observations
Battery degradation	Battery temperature, voltage, current
Thermal cycling damage	Temperature cycle data
Radiation exposure	Environmental test observations
Communication effects	LoRa telemetry and link state
Energy and endurance	Voltage, current, and time-resolved power data

The available dataset currently provides direct current, voltage, and temperature measurements.

32. Data Limitations

The currently available measurement dataset does not establish:

elapsed time
energy consumption
mission endurance
battery capacity
battery ageing
complete high-altitude thermal behaviour
PHP performance under all operating conditions
communication range
radiation tolerance

Additional measurements would be required to establish those quantities.

33. Data Validation

Before using data for validation, the following relationships should be established:

Measurement
    │
    ▼
Known Sensor
    │
    ▼
Known Location
    │
    ▼
Known Test Condition
    │
    ▼
Known Hardware Configuration
    │
    ▼
Comparable Simulation / Requirement
    │
    ▼
Validation Evidence

Without this traceability, a numerical value may remain useful as an observation but may not support a broader validation claim.

34. Data Evidence Levels
Evidence level	Description
Raw	Original recorded measurement
Processed	Measurement after documented processing
Derived	Calculated quantity
Simulated	Numerical model output
Correlated	Simulation and measurement comparison
Validated	Evidence supporting a defined model or requirement

This classification helps prevent different types of evidence from being presented as equivalent.

35. Data-to-Repository Traceability
03_Hardware/
      │
      ▼
04_Software/
      │
      ▼
06_Simulation/
      │
      ▼
07_Testing/
      │
      ▼
08_Data/
      │
      ├──► Analysis
      │
      ├──► GCS
      │
      └──► Documentation

Hardware configuration and firmware behaviour influence the data that is produced during testing.

36. Data and Firmware

Firmware determines how sensor measurements are:

acquired
processed
interpreted
transmitted
used for control

Therefore, firmware version is relevant when interpreting recorded data.

A change in firmware can change:

sampling behaviour
scaling
filtering
control response
telemetry content

Data records should therefore remain associated with the firmware configuration used to acquire them.

37. Data and GCS

The GCS provides a human-readable representation of selected onboard measurements.

The data path is:

Physical Condition
       │
       ▼
Sensor
       │
       ▼
ESP32
       │
       ▼
Firmware Processing
       │
       ▼
LoRa Telemetry
       │
       ▼
GCS
       │
       ▼
Displayed Information

The displayed value should remain distinguishable from the original sensor measurement where processing or conversion occurs.

38. Data and Safety

Safety-related data includes:

temperature state
heater state
sensor fault state
communication state
control state
system state

These values support the evaluation of the HIFLY autonomous safety architecture.

A communication failure should not be confused with a thermal failure.

A sensor fault should not be confused with a valid physical temperature.

39. Data Reporting

Engineering reports should identify whether a statement is based on:

direct measurement
calculated value
simulation
comparison
engineering interpretation

For example:

Measured:
Voltage = 12.0 V
Current = 1.54 A

Calculated:
Power = 18.48 W

Interpretation:
Electrical operating point associated with the measurement.

This separation maintains evidence clarity.

40. Data Reproducibility

A useful HIFLY dataset should allow the analysis to be repeated from the original measurements.

The reproducibility chain is:

Raw Data
   │
   ▼
Processing Method
   │
   ▼
Derived Data
   │
   ▼
Analysis
   │
   ▼
Result

The raw data should remain unchanged when derived datasets are generated.

41. Data Quality and Experimental Evidence

Experimental evidence becomes more useful when the test context is preserved.

Important contextual information includes:

hardware revision
test ID
sensor location
environmental condition
electrical operating point
heater state
control mode
communication state

A numerical value without context has limited engineering meaning.

42. Overall HIFLY Data Architecture
                         HIFLY SYSTEM
                              │
                              ▼
                        Physical State
                              │
          ┌───────────────────┼───────────────────┐
          │                   │                   │
          ▼                   ▼                   ▼
      Thermal             Electrical        Communication
      Sensors              Sensors              State
          │                   │                   │
          └───────────────────┼───────────────────┘
                              ▼
                            ESP32
                              │
                 ┌────────────┼────────────┐
                 │                         │
                 ▼                         ▼
            Local Data                   LoRa
                 │                         │
                 ▼                         ▼
             Data File                    GCS
                 │
                 └──────────────┬──────────┘
                                ▼
                           Data Analysis
                                │
                 ┌──────────────┼──────────────┐
                 │              │              │
                 ▼              ▼              ▼
             Thermal        Electrical      Control
             Analysis        Analysis       Analysis
                 │              │              │
                 └──────────────┼──────────────┘
                                ▼
                         Simulation Comparison
                                │
                                ▼
                           Validation
43. Status

Data Framework Status: Defined / Experimental Data Available

The HIFLY project contains an existing current-voltage-temperature measurement dataset.

The dataset provides experimental electrical and thermal observations, while additional time-resolved and subsystem-specific data can support deeper thermal, energy, communication, and validation analysis.
