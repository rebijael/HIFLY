# HIFLY — Simulation Models

## 1. Purpose

The `models/` directory contains the simulation-model definitions used to represent the thermal, electrical, battery, protection, and environmental aspects of the HIFLY system.

The purpose of these models is to establish a consistent engineering representation of the system before simulation cases are executed.

The models provide the connection between:

- HIFLY system architecture
- CAD geometry
- material properties
- thermal loads
- electrical loads
- battery behaviour
- thermal-management elements
- environmental conditions
- simulation cases
- experimental measurements

The models are intended to support engineering comparison and design verification without treating simulation results as experimental validation.

---

## 2. Model Structure

The HIFLY simulation model can be represented as a set of coupled physical domains.

```text
                         HIFLY SIMULATION MODEL
                                  │
             ┌────────────────────┼────────────────────┐
             │                    │                    │
             ▼                    ▼                    ▼
      THERMAL MODEL         ELECTRICAL MODEL     ENVIRONMENT MODEL
             │                    │                    │
             │                    │                    │
             ▼                    ▼                    ▼
       Heat sources          Voltage/current      High-altitude
       conduction            consumption          conditions
       convection            heater load          pressure effects
       radiation             battery load         thermal boundary
             │                    │                    │
             └────────────────────┼────────────────────┘
                                  │
                                  ▼
                       BATTERY THERMAL MODEL
                                  │
                                  ▼
                     THERMAL MANAGEMENT MODEL
                                  │
             ┌────────────────────┼────────────────────┐
             │                    │                    │
             ▼                    ▼                    ▼
             PHP              Heater            Insulation
             │                    │                    │
             └────────────────────┼────────────────────┘
                                  │
                                  ▼
                       SYSTEM THERMAL RESPONSE
                                  │
                                  ▼
                    SIMULATION CASE COMPARISON
                                  │
                                  ▼
                    EXPERIMENTAL CORRELATION
```
3. Thermal Model

The thermal model represents heat generation, heat transfer, and temperature distribution throughout the HIFLY system.

The model may include:

processor or electronics heat generation
battery heat generation
heater heat generation
conduction through structural components
conduction through thermal interfaces
heat transfer through insulation
heat spreading through conductive components
radiation where appropriate
convection where the environmental model supports it
heat-transfer behaviour associated with the Pulsating Heat Pipe (PHP)

The thermal model is the primary simulation domain for evaluating the effectiveness of the proposed thermal-management architecture.

3.1 Thermal Nodes

The system can be represented using thermal regions or nodes corresponding to physically meaningful components.

      ELECTRONICS
           │
           │ heat generation
           ▼
   ┌────────────────┐
   │ Thermal Region │
   └───────┬────────┘
           │
           │ conduction
           ▼
   ┌────────────────┐
   │ Heat Spreader  │
   └───────┬────────┘
           │
      ┌────┴─────┐
      │          │
      ▼          ▼
     PHP      Structure
      │          │
      │          │
      ▼          ▼
 Condenser   Enclosure
      │          │
      └────┬─────┘
           │
           ▼
      Environment

The exact number of thermal regions depends on the CAD configuration and the level of detail required by the simulation case.

4. Heat Sources

Heat sources are assigned to components according to their expected operating conditions.

Potential heat sources in the HIFLY model include:

Component	Thermal representation
Processor / electronics	Volumetric or surface heat generation
Battery pack	Internal heat generation
Heating element	Controlled heat generation
Power electronics	Electrical-to-thermal loss
Other electronics	Component-level thermal load where applicable

Actual heat-generation values should be taken from measured electrical behaviour, component specifications, manufacturer information, or explicitly defined simulation assumptions.

No fixed heat-load value is assumed by this repository unless it is associated with a specific documented simulation case.

5. Battery Thermal Model

The battery is treated as both an electrical energy source and a thermal component.

The battery model can include:

electrical load
internal heat generation
heat transfer to surrounding structure
insulation around the battery
temperature monitoring location
thermal-management interaction
heater interaction
environmental heat loss

The conceptual relationship is:

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
   Electrical output    Heat generation
                              │
                              ▼
                       Battery temperature
                              │
                  ┌───────────┴───────────┐
                  │                       │
                  ▼                       ▼
             Heat transfer          Heater response
                  │                       │
                  └───────────┬───────────┘
                              ▼
                       Thermal condition

The battery thermal model should remain linked to the electrical operating condition used in the corresponding simulation case.

6. Pulsating Heat Pipe Model

The Pulsating Heat Pipe is represented as a thermal-management element within the system.

Its purpose in the HIFLY architecture is to provide a passive heat-transfer path between a heat-source region and a heat-rejection region.

Conceptually:

              HEAT SOURCE
                  │
                  ▼
          ┌───────────────┐
          │  EVAPORATOR   │
          └───────┬───────┘
                  │
                  │ heat transport
                  ▼
        ┌─────────────────────┐
        │ Pulsating Heat Pipe │
        └──────────┬──────────┘
                   │
                   ▼
            ┌─────────────┐
            │ CONDENSER   │
            └──────┬──────┘
                   │
                   ▼
             Heat rejection

The PHP may be represented at different levels of modelling fidelity.

6.1 Detailed PHP Representation

A high-fidelity model may include:

pipe geometry
working-fluid properties
fluid behaviour
phase-change effects
evaporator region
condenser region
thermal interfaces
transient behaviour

Such a model requires appropriate two-phase-flow and phase-change modelling.

6.2 System-Level PHP Representation

For system-level thermal FEA, the PHP can instead be represented as an equivalent thermal path or effective thermal-conductance region.

This approach allows the overall HIFLY thermal architecture to be evaluated without claiming that a conventional steady-state thermal solver reproduces the complete pulsating two-phase flow inside the PHP.

7. PHP Comparison Model

The PHP-assisted design should be compared with an appropriate reference configuration.

A conceptual comparison is:

CASE A
Without PHP

Heat Source
    │
    ▼
Structure
    │
    ▼
Enclosure
    │
    ▼
Environment


CASE B
With PHP

Heat Source
    │
    ▼
PHP Evaporator
    │
    ▼
PHP Thermal Path
    │
    ▼
PHP Condenser
    │
    ▼
Heat Rejection Region

The comparison may examine:

maximum temperature
temperature distribution
thermal gradients
heat flux
heat-transfer path
thermal concentration around the heat source

The comparison must use equivalent boundary conditions and equivalent heat-source definitions wherever the purpose is to isolate the effect of the PHP architecture.

8. Heater Model

The heating element represents the active thermal-management component used to support operation when the system becomes too cold.

The heater can be represented as a controlled heat source.

                 Temperature Sensor
                        │
                        ▼
                 ┌──────────────┐
                 │ Control Logic│
                 └──────┬───────┘
                        │
                 Heater command
                        │
                        ▼
                 ┌──────────────┐
                 │    Heater    │
                 └──────┬───────┘
                        │
                        ▼
                Battery / thermal
                     region

The simulation representation may include:

heater location
heating surface or volume
heat-generation input
surrounding insulation
thermal contact
control-state assumptions

The heater model should be linked to the control logic used by the corresponding simulation case.

9. Insulation Model

Thermal insulation is represented as a material region surrounding or separating selected components.

Its purpose is to control unwanted heat transfer between:

battery and environment
electronics and environment
heater and surrounding structure
internal thermal regions
external cold environment

The insulation model should include the relevant thermal properties of the selected material.

Where temperature-dependent material properties are available, they may be used instead of a single constant property.

10. Flexible Silicone Protection Model

Flexible silicone is considered both as a protective material and as a thermal-interface region where applicable.

The model may account for:

geometric coverage
thermal conductivity
thickness
contact with protected components
thermal expansion considerations
environmental exposure

The simulation should distinguish between the mechanical protection function and the thermal insulation function.

Silicone should not automatically be treated as a primary heat-transfer path unless the actual design uses it in that role.

11. Lightweight Protective Layer

The lightweight protective layer represents the environmental protection element included in the HIFLY architecture.

Depending on the final design, the layer may provide protection against environmental exposure while contributing to the thermal path of the system.

The model should therefore distinguish:

Mechanical / environmental function
                │
                ├──────────────► Protective layer
                │
Thermal function
                │
                └──────────────► Thermal boundary/interface

The exact thermal effect depends on the material, thickness, geometry, and placement used in the final design.

12. Enclosure Model

The enclosure represents the external structural boundary of the HIFLY thermal system.

The enclosure model may include:

enclosure geometry
wall thickness
material properties
internal contact regions
external surface conditions
interfaces with insulation
antenna or communication-system interfaces where relevant

The enclosure provides the boundary through which internal thermal energy interacts with the external environment.

13. Environmental Model

High-altitude operation changes the thermal environment compared with conventional ground-level operation.

The environmental model can represent relevant conditions such as:

reduced ambient pressure
reduced convective heat transfer
low ambient temperature
radiative heat exchange
external surface conditions

A simplified thermal representation is:

             HIFLY INTERNAL SYSTEM
                     │
          ┌──────────┼──────────┐
          │          │          │
          ▼          ▼          ▼
      Conduction  Radiation  Convection
          │          │          │
          └──────────┼──────────┘
                     │
                     ▼
             External Environment

The actual boundary conditions must correspond to the altitude and operating condition represented by the simulation case.

14. Low-Pressure Protection Chamber

The low-pressure protection chamber is represented as a separate system boundary where appropriate.

Its purpose is associated with protection of electrical and electronic components under reduced-pressure conditions.

The model may include:

chamber geometry
internal components
protective enclosure
thermal interfaces
internal pressure assumptions
external environmental boundary

The thermal model should distinguish the chamber from the surrounding external environment if the chamber is intended to provide a controlled internal condition.

15. Electrical Model

The electrical model connects system loads with the battery and power-management architecture.

Conceptually:

                 BATTERY
                    │
                    ▼
             Power Distribution
                    │
        ┌───────────┼────────────┐
        │           │            │
        ▼           ▼            ▼
   Electronics    Heater      Sensors
        │           │            │
        └───────────┼────────────┘
                    │
                    ▼
              Electrical load

Electrical variables relevant to the model include:

voltage
current
electrical power
heater state
battery operating condition

For a measured operating point, electrical power may be calculated as:

$$ P = V I $$

For example, the available measurement dataset contains a point of:

$$ V = 12.0\text{ V} $$ $$ I = 1.54\text{ A} $$

which corresponds to an instantaneous electrical power of:

$$ P = 12.0 \times 1.54 = 18.48\text{ W} $$

This value represents the electrical power calculated from that measured operating point. It is not, by itself, an energy-consumption result.

16. Power-to-Heat Representation

Electrical power consumed by components may be converted into thermal loads where the electrical energy is ultimately dissipated as heat.

A simplified relationship is:

Electrical power
      │
      ▼
Component operation
      │
      ▼
Electrical losses
      │
      ▼
Thermal load
      │
      ▼
Temperature response

The conversion should reflect the physical behaviour of the specific component.

Not every electrical power path should automatically be treated as a thermal source in the same location.

17. Sensor Model

The HIFLY model includes measurement locations corresponding to the physical monitoring system.

Potential monitored variables include:

Variable	Model relevance
Temperature	Thermal-state monitoring
Voltage	Battery/electrical-state monitoring
Current	Electrical-load monitoring
Heater state	Active thermal-management state
Communication state	System-monitoring state

Sensor placement should correspond to the actual physical design where possible.

The simulation model may produce temperatures at sensor locations so that simulation values can later be compared with experimental measurements.

18. Thermal Control Model

The thermal-control model connects temperature measurements to heater operation.

A conceptual control flow is:

                 Temperature
                      │
                      ▼
              Sensor acquisition
                      │
                      ▼
              Control evaluation
                      │
             ┌────────┴────────┐
             │                 │
        Heating required    Heating not
             │               required
             ▼                 │
          HEATER ON             ▼
             │              HEATER OFF
             └────────┬────────┘
                      │
                      ▼
              Updated thermal state

The control model should represent the same control logic implemented by the HIFLY firmware when simulation-to-hardware comparison is required.

Actual temperature thresholds are design parameters and should only be included in a simulation model when they are defined by the corresponding control specification.

19. Fail-Safe Model

The HIFLY architecture includes autonomous safety behaviour for loss of communication.

The conceptual model is:

              Communication Link
                      │
                      ▼
                Link available?
                  /       \
                YES        NO
                 │          │
                 ▼          ▼
          Normal GCS     Autonomous
           operation     control mode
                 │          │
                 └────┬─────┘
                      ▼
               Safety logic
                      │
                      ▼
              Thermal control
                      │
                      ▼
               System state

The model separates communication availability from local thermal-control capability.

Loss of the LoRa/GCS link should not automatically imply loss of onboard thermal-control operation.

20. LoRa Communication Model

The communication subsystem can be represented at system level as:

       HIFLY ONBOARD SYSTEM
                │
                ▼
          LilyGO T3-S3
                │
                │ LoRa
                ▼
          Ground Station
                │
                ▼
       Monitoring / Control

The simulation model may represent communication state as:

link available
link unavailable
data transmission state
control availability
communication-loss transition

A communication model does not by itself establish real-world radio range or link reliability.

Those properties require experimental testing.

21. Coupled Model

The overall HIFLY simulation can be treated as a coupled thermal-electrical-control model.

                         ┌─────────────────┐
                         │   Environment   │
                         └────────┬────────┘
                                  │
                                  ▼
                         ┌─────────────────┐
                         │ Thermal System  │
                         └───────┬─────────┘
                                 │
               ┌─────────────────┼─────────────────┐
               │                 │                 │
               ▼                 ▼                 ▼
           Electronics        Battery            PHP
               │                 │                 │
               └─────────────────┼─────────────────┘
                                 │
                                 ▼
                          Temperature State
                                 │
                                 ▼
                         Sensor Measurement
                                 │
                                 ▼
                         Control Algorithm
                                 │
                    ┌────────────┴────────────┐
                    │                         │
                    ▼                         ▼
                 Heater                   System State
                    │                         │
                    └────────────┬────────────┘
                                 │
                                 ▼
                         Updated Thermal State

This structure allows thermal-management behaviour to be evaluated as part of the complete system rather than as an isolated component.

22. Model Inputs

Model inputs may include:

CAD geometry
material properties
component heat-generation values
electrical operating conditions
battery characteristics
heater characteristics
environmental conditions
thermal boundary conditions
contact definitions
insulation properties
PHP equivalent thermal properties where applicable
control states

The source of each input should remain traceable to one of:

component documentation
measured data
CAD definition
established material data
explicitly defined simulation assumption
23. Model Outputs

Typical model outputs include:

Thermal
maximum temperature
minimum temperature
temperature distribution
temperature at sensor locations
thermal gradients
heat flux
heat-transfer paths
Electrical
voltage
current
instantaneous electrical power
electrical load distribution
Control
heater state
control state
thermal-control response
fail-safe state
System
thermal condition
battery thermal condition
communication state
interaction between active and passive thermal-management elements
24. Existing Measurement Dataset

The project contains a measurement dataset with the following recorded values:

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

The dataset provides measured electrical and temperature points that can be used for comparison with corresponding simulation operating conditions.

The dataset does not contain an explicit time column. Therefore, the rows should be treated as sequential measurements rather than a time-history unless the original experiment documentation establishes the sampling interval.

25. Model Verification

Model verification concerns whether the simulation model has been constructed and implemented correctly.

Verification activities may include:

checking geometry
checking material assignment
checking heat-source definition
checking boundary conditions
checking mesh quality
checking solver convergence
checking units
checking thermal interfaces
checking control-state definitions
checking electrical input consistency

Verification does not establish that the physical HIFLY system behaves exactly as the model predicts.

26. Model Validation

Model validation concerns comparison between simulation predictions and physical measurements.

A simplified validation flow is:

             Physical System
                   │
                   ▼
             Experimental Data
                   │
                   │
                   ▼
Simulation Model ────────► Comparison
                   │
                   ▼
             Agreement Analysis

Validation should use comparable:

operating conditions
heat loads
ambient conditions
sensor locations
component configurations
boundary conditions

The available project measurement dataset provides an experimental reference, but it does not by itself validate every part of the complete HIFLY model.

27. PHP Modelling Limitation

A conventional steady-state thermal FEA model should not be interpreted as a direct simulation of the internal pulsating two-phase flow of a PHP.

The following distinction is maintained:

STEADY-STATE THERMAL FEA
        │
        ├── evaluates temperature field
        ├── evaluates heat-transfer path
        ├── compares thermal architecture
        └── evaluates effective PHP representation


FULL PHP PHYSICAL MODEL
        │
        ├── fluid motion
        ├── phase change
        ├── vapor/liquid interaction
        ├── pulsation
        └── transient two-phase behaviour

A higher-fidelity PHP study would require an appropriate multiphase and phase-change modelling approach.

28. Model Fidelity Levels

The HIFLY simulation architecture can use different levels of model fidelity.

Level	Representation
Conceptual	System blocks and heat-transfer paths
System-level	Equivalent thermal/electrical components
CAD-based	Actual component geometry
Thermal FEA	Temperature and heat-flow evaluation
High-fidelity	Detailed physical or multiphysics behaviour

The selected fidelity should match the engineering question being investigated.

A higher-fidelity model is not automatically necessary when the objective is system-level comparison.

29. Simulation Case Traceability

Each simulation case should identify the model configuration used.

Simulation Case
      │
      ├── Geometry version
      ├── Material set
      ├── Heat-source definition
      ├── Boundary conditions
      ├── Environmental condition
      ├── PHP representation
      ├── Heater state
      ├── Electrical operating point
      └── Control state

This allows results from different simulation cases to be compared without losing the configuration that produced them.

30. Relationship With CAD

The simulation model should remain traceable to the CAD model.

CAD Geometry
     │
     ▼
Simulation Geometry
     │
     ├── Thermal regions
     ├── Interfaces
     ├── Heat-source locations
     ├── PHP location
     ├── Heater location
     └── Insulation regions

Changes to the CAD geometry can alter:

thermal path length
contact area
heat-transfer area
material volume
thermal mass
enclosure boundary
PHP placement
heater placement

Therefore, the simulation configuration should identify the corresponding geometry version.

31. Relationship With Hardware

The simulation model represents the hardware architecture rather than replacing it.

                 DESIGN
                   │
                   ▼
                  CAD
                   │
             ┌─────┴─────┐
             ▼           ▼
        Simulation     Hardware
             │           │
             │           ▼
             │       Prototype
             │           │
             └─────┬─────┘
                   ▼
              Comparison
                   │
                   ▼
              Refinement

This forms the simulation-to-prototype engineering loop.

32. Evidence Classification

Simulation information in the HIFLY repository should be interpreted according to its evidence state.

State	Meaning
Concept	Proposed model or architecture
Designed	Model configuration defined
Simulated	Solver-generated result exists
Prototype	Physical implementation exists
Tested	Physical test has been performed
Validated	Simulation has been compared against suitable experimental evidence

A simulation result should not be described as a physical test result.

A CAD design should not be described as a validated hardware implementation.

33. Model Limitations

The model may not fully represent:

complete atmospheric turbulence
all high-altitude radiation effects
detailed multiphase PHP dynamics
manufacturing tolerances
imperfect thermal contacts
sensor installation errors
all battery electrochemical behaviour
all transient flight conditions
mechanical vibration
every communication-path failure mode

These limitations are important when interpreting simulation results.

34. Engineering Use of the Models

The models support the following engineering activities:

comparison of thermal architectures
evaluation of heat-transfer paths
study of PHP-assisted thermal management
assessment of heater placement
evaluation of insulation arrangements
battery thermal analysis
investigation of thermal gradients
preparation of physical test cases
comparison with measured temperature and electrical data
refinement of the HIFLY design
35. Model-to-Test Workflow
             SYSTEM REQUIREMENTS
                     │
                     ▼
              HIFLY ARCHITECTURE
                     │
                     ▼
                  CAD MODEL
                     │
                     ▼
              SIMULATION MODEL
                     │
                     ▼
              Simulation Cases
                     │
                     ▼
                 Results
                     │
                     ▼
             Physical Prototype
                     │
                     ▼
                Test Setup
                     │
                     ▼
             Experimental Data
                     │
                     ▼
          Simulation/Test Comparison
                     │
                     ▼
             Model Refinement
                     │
                     ▼
             Design Refinement
36. Repository Traceability

The simulation-model definitions connect the following HIFLY repository areas:

01_Problem_Statement/
        │
        ▼
02_Solution/
        │
        ▼
03_Hardware/
        │
        ▼
05_CAD/
        │
        ▼
06_Simulation/
        │
        ▼
07_Testing/
        │
        ▼
08_Data/

This structure maintains a traceable path from the identified high-altitude reliability problems to the physical architecture, simulation model, experimental test, and measurement data.

37. Current Model Scope

The HIFLY model framework covers:

thermal behaviour
battery thermal behaviour
heater behaviour
insulation
PHP-assisted heat transfer
enclosure effects
environmental thermal conditions
electrical operating conditions
sensor measurements
thermal-control logic
communication state
fail-safe operation

The detailed fidelity of each model depends on the available geometry, component data, material data, experimental data, and simulation method.

38. Status

Simulation Model Status: Design / Simulation Framework

The model architecture defines the physical and logical domains required for HIFLY simulation.

Simulation outputs should be classified separately according to the actual cases executed and the evidence available for each case.

39. Overall Model Representation
                         ┌──────────────────────┐
                         │   HIFLY REQUIREMENTS │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ SYSTEM ARCHITECTURE  │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │      CAD MODEL       │
                         └──────────┬───────────┘
                                    │
                  ┌─────────────────┼─────────────────┐
                  │                 │                 │
                  ▼                 ▼                 ▼
            Thermal Model     Electrical Model   Environment
                  │                 │                 │
                  └─────────────────┼─────────────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Battery Thermal     │
                         │ Management          │
                         └──────────┬───────────┘
                                    │
                       ┌────────────┼────────────┐
                       │            │            │
                       ▼            ▼            ▼
                      PHP         Heater     Insulation
                       │            │            │
                       └────────────┼────────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Temperature / Power │
                         │ Response             │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Control + Fail-safe │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Simulation Results   │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Experimental Data   │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Comparison /         │
                         │ Engineering Review   │
                         └──────────────────────┘
