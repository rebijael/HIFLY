# HIFLY Thermal Simulation

## 1. Overview

The HIFLY thermal simulation study evaluates the thermal behaviour of the integrated high-altitude thermal-management architecture.

The thermal model is based on the HIFLY CAD system and focuses on the interaction between:

- heat-generating electronics
- battery thermal region
- Pulsating Heat Pipe (PHP)
- heating element
- thermal insulation
- enclosure
- thermal interfaces
- surrounding structure

The purpose of the thermal simulation is to provide a computational reference that can be compared with physical prototype measurements.

The simulation results are treated separately from experimental data.

---

## 2. Thermal Simulation Objective

The primary objective is to evaluate how heat is distributed through the HIFLY architecture and how the proposed thermal-management elements affect that distribution.

The study can be used to examine:

- maximum temperature
- temperature distribution
- thermal gradients
- heat-transfer paths
- PHP-assisted thermal behaviour
- heater-assisted thermal behaviour
- insulation effects
- thermal interfaces
- interaction between thermal components

```text
                     HIFLY CAD
                         │
                         ▼
                Thermal Model
                         │
        ┌────────────────┼────────────────┐
        │                │                │
        ▼                ▼                ▼
     Battery         Electronics         PHP
        │                │                │
        └────────────────┼────────────────┘
                         │
                         ▼
                     Insulation
                         │
                         ▼
                      Heater
                         │
                         ▼
                     Enclosure
                         │
                         ▼
                  Thermal Analysis
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
     Temperature      Heat Flux      Gradients
```
3. Simulation Model Scope

The thermal model represents the physical geometry relevant to the selected study.

Depending on the simulation case, the model may include:

Model Component	Thermal Role
Electronics / processor region	Heat source
Battery	Thermally sensitive subsystem
PHP	Passive thermal-transfer pathway
Heater	Controlled thermal input
Insulation	Thermal isolation
Enclosure	Structural and thermal boundary
Thermal interfaces	Heat-transfer connections
Surrounding structure	Heat-transfer and boundary region

The exact components included depend on the specific simulation configuration.

4. Thermal Simulation Workflow
             HIFLY CAD
                 │
                 ▼
        Geometry Preparation
                 │
                 ▼
       Material Assignment
                 │
                 ▼
        Thermal Interfaces
                 │
                 ▼
       Boundary Conditions
                 │
                 ▼
          Mesh Generation
                 │
                 ▼
          Thermal Solver
                 │
                 ▼
       Post-Processing
                 │
        ┌────────┼────────┐
        ▼        ▼        ▼
   Temperature Heat Flux Thermal
   Distribution          Gradient
        │        │        │
        └────────┼────────┘
                 ▼
          Engineering Review
5. CAD Geometry

The thermal simulation uses the HIFLY CAD architecture as the geometric reference.

Relevant CAD subsystems include:

05_CAD/
│
├── assembly/
├── battery/
├── php/
├── enclosure/
└── thermal_management/

These subsystems provide the physical relationships between the battery, PHP, enclosure, heater, insulation, and surrounding structure.

The CAD model is the reference for physical geometry. Unsupported dimensions are not introduced into the simulation documentation.

6. Thermal Heat Sources

Heat generation is represented at the appropriate component or thermal region.

For electronic components:

              ELECTRONICS
                   │
                   ▼
             Heat Generation
                   │
                   ▼
          Thermal Interface
                   │
                   ▼
                 PHP
                   │
                   ▼
          Thermal Output Path

The processor or electronics heat input should be based on the actual operating condition represented by the simulation.

An arbitrary heat-generation value should not be used as evidence of actual HIFLY operation.

7. Battery Thermal Region

The battery is included where its thermal behaviour is relevant to the study.

                BATTERY
                   │
                   ▼
          Thermal Properties
                   │
                   ▼
            Thermal Model
                   │
                   ▼
         Temperature Field
                   │
                   ▼
          Thermal Evaluation

The simulation does not establish battery electrochemical behaviour unless a separate electrochemical model is used.

The thermal study is limited to the thermal behaviour represented by the selected model.

8. PHP Representation

The PHP is represented as part of the thermal-management architecture.

             HEAT SOURCE
                  │
                  ▼
          PHP EVAPORATOR
                  │
                  ▼
              PHP PATH
                  │
                  ▼
          PHP CONDENSER
                  │
                  ▼
            HEAT OUTPUT

A conventional steady-state thermal simulation can evaluate the thermal behaviour of the PHP-assisted architecture.

It does not automatically simulate:

vapour plug movement
liquid slug movement
pulsation frequency
phase-change dynamics
detailed internal two-phase flow

A dedicated multiphase simulation would be required for those phenomena.

9. PHP-Assisted Thermal Model

The PHP-assisted configuration can be represented as:

              HEAT SOURCE
                   │
                   ▼
           ┌───────────────┐
           │ PHP Interface │
           └───────┬───────┘
                   │
                   ▼
              PHP Path
                   │
                   ▼
          Thermal Output Region

The thermal simulation evaluates the represented heat-transfer architecture rather than claiming to reproduce the internal PHP fluid dynamics.

10. Baseline Thermal Model

A baseline model provides a reference configuration for comparison.

              HEAT SOURCE
                   │
                   ▼
          Thermal Structure
                   │
                   ▼
             Heat Transfer
                   │
                   ▼
           External Boundary

Where a baseline model is compared with a PHP-assisted model, the relevant inputs and boundary conditions should remain consistent so that the architectural difference can be meaningfully evaluated.

11. PHP Comparison

The principal thermal comparison can be structured as:

                     HIFLY THERMAL STUDY
                              │
                 ┌────────────┴────────────┐
                 │                         │
                 ▼                         ▼
           Without PHP                 With PHP
                 │                         │
                 ▼                         ▼
          Thermal Solve              Thermal Solve
                 │                         │
                 ▼                         ▼
             Result A                  Result B
                 │                         │
                 └────────────┬────────────┘
                              ▼
                         Comparison

Relevant comparison outputs include:

Output	Comparison Purpose
Maximum temperature	Compare peak thermal exposure
Temperature distribution	Compare thermal spreading
Heat flux	Compare represented heat-transfer paths
Thermal gradient	Compare spatial temperature variation
Interface temperature	Examine important thermal contacts

No numerical improvement is claimed without corresponding simulation results.

12. Heater Representation

The heating element is represented as a thermal input when the selected simulation case includes active heating.

                 HEATER
                    │
                    ▼
               Heat Input
                    │
                    ▼
             Thermal Region
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
       Battery               PHP

The simulated heater input must correspond to a documented or measured operating condition.

The CAD model alone does not establish heater power.

13. Insulation Representation

Thermal insulation can be included in the model to represent the intended battery and system packaging.

       External Environment
                │
                ▼
       ┌─────────────────┐
       │ Insulation      │
       ├─────────────────┤
       │ Thermal System  │
       └─────────────────┘

The insulation can influence the calculated thermal distribution and heat-transfer path.

Material properties used in the simulation should correspond to the selected insulation material or documented source data.

14. Enclosure Representation

The enclosure can be included where its thermal properties and geometry affect the study.

        ┌────────────────────────┐
        │       Enclosure        │
        │                        │
        │  ┌──────────────────┐  │
        │  │ Thermal System   │  │
        │  │                  │  │
        │  │ Battery / PHP   │  │
        │  │ Electronics     │  │
        │  └──────────────────┘  │
        │                        │
        └────────────────────────┘

The enclosure can act as a conduction path and as part of the thermal boundary.

The exact treatment depends on the simulation objective.

15. Thermal Interfaces

Thermal interfaces are important because heat must pass between physically connected components.

Examples include:

electronics-to-thermal structure
battery-to-thermal structure
heater-to-battery region
PHP-to-thermal structure
enclosure-to-internal structure
 Component A
      │
      ▼
┌──────────────┐
│ Thermal      │
│ Interface    │
└──────┬───────┘
       │
       ▼
 Component B

The thermal model should represent the appropriate interface assumptions for the selected study.

16. Material Properties

The thermal simulation requires material properties for the represented geometry.

Relevant properties include:

Property	Role
Thermal conductivity	Governs conductive heat transfer
Density	Relevant to transient thermal behaviour
Specific heat	Relevant to transient thermal behaviour
Emissivity	Relevant where radiative heat transfer is modelled

The actual values should originate from the selected material or documented component data.

This README does not assign unsupported numerical material properties.

17. Boundary Conditions

The thermal model requires defined boundary conditions.

Potential inputs include:

heat generation
heater input
ambient temperature
convection condition
fixed temperature
thermal contact
insulation boundary
radiation condition where applicable
             THERMAL MODEL
                   │
       ┌───────────┼───────────┐
       │           │           │
       ▼           ▼           ▼
   Heat Input   Ambient      Thermal
                Condition    Interface
       │           │           │
       └───────────┼───────────┘
                   ▼
              Thermal Solve

Every numerical boundary condition should be recorded with the corresponding simulation case.

18. Mesh

The CAD geometry is converted into a computational mesh before solving.

             CAD MODEL
                 │
                 ▼
             Mesh Setup
                 │
       ┌─────────┼─────────┐
       │         │         │
       ▼         ▼         ▼
     Global    Local     Interface
     Mesh      Refinement Refinement
       │         │         │
       └─────────┼─────────┘
                 ▼
           Thermal Solver

Mesh refinement is particularly relevant around:

heat-source regions
PHP interfaces
battery interfaces
heater interfaces
geometry transitions

Mesh quality and convergence should be evaluated for the individual simulation case.

19. Steady-State Thermal Analysis

A steady-state thermal analysis evaluates the calculated thermal condition under constant model inputs and boundary conditions.

        Defined Thermal Inputs
                  │
                  ▼
          Thermal Solver
                  │
                  ▼
         Converged Solution
                  │
                  ▼
       Temperature Distribution
                  │
        ┌─────────┼─────────┐
        ▼         ▼         ▼
     Maximum    Heat Flux  Gradient
     Temp.

The resulting temperature field describes the simulated steady-state condition and should not be interpreted as a transient temperature history.

20. Transient Thermal Analysis

A transient analysis can be used where time-dependent thermal behaviour is required.

Potential uses include:

heater activation
heater deactivation
temperature response
thermal cycling
thermal settling
changing operating conditions
          Initial Condition
                  │
                  ▼
             Time = t₀
                  │
                  ▼
          Thermal Evolution
                  │
        ┌─────────┼─────────┐
        ▼         ▼         ▼
       t₁         t₂        t₃
        │         │         │
        └─────────┼─────────┘
                  ▼
          Temperature History

A transient study requires an appropriate time scale and time-dependent input conditions.

No time-dependent result is claimed unless the simulation has actually been performed.

21. Thermal Contours

Thermal contours provide a visual representation of the calculated temperature field.

              THERMAL CONTOUR

       ┌───────────────────────────┐
       │                           │
       │       Heat Source         │
       │          ███              │
       │        ███████            │
       │       █████████           │
       │            │              │
       │            ▼              │
       │         PHP Path          │
       │            │              │
       │            ▼              │
       │      Thermal Output       │
       │                           │
       └───────────────────────────┘

The actual temperature scale must be taken from the simulation output.

22. Maximum Temperature

Maximum temperature is one of the primary thermal outputs.

It identifies the highest calculated temperature within the selected model or component.

             Simulation Result
                    │
                    ▼
          Temperature Field
                    │
                    ▼
             Maximum Value
                    │
                    ▼
          Thermal Assessment

A maximum temperature should always be reported with:

simulation case
geometry version
heat input
boundary conditions
relevant component or location
23. Temperature Distribution

Temperature distribution provides more information than a single maximum value.

           Temperature Field
                  │
       ┌──────────┼──────────┐
       │          │          │
       ▼          ▼          ▼
     Battery     PHP      Electronics
       │          │          │
       └──────────┼──────────┘
                  ▼
             Distribution

This allows thermal gradients and local hot regions to be examined.

24. Heat Flux

Heat flux indicates the calculated direction and magnitude of heat transfer through the model.

             Heat Source
                  │
                  ▼
             Heat Flux
                  │
                  ▼
              PHP Path
                  │
                  ▼
          Thermal Output

Heat-flux results are useful for examining whether the represented thermal architecture produces the intended heat-transfer pathway.

25. Simulation Outputs

The thermal study can report:

Result	Description
Maximum temperature	Highest calculated temperature
Minimum temperature	Lowest calculated temperature
Temperature distribution	Spatial temperature field
Thermal gradient	Temperature variation across the model
Heat flux	Heat-transfer distribution
Interface temperature	Temperature at selected interfaces
Thermal contour	Visual representation of the temperature field

The actual numerical results belong to the corresponding simulation result files.

26. Simulation Case Documentation

Each completed simulation case should retain a clear record of its inputs and outputs.

Simulation Case
│
├── Geometry
├── Materials
├── Heat Inputs
├── Boundary Conditions
├── Mesh
├── Solver Settings
├── Results
└── Interpretation

This structure provides traceability between the model and its result.

27. Experimental Correlation

Thermal simulation becomes more useful when its predictions can be compared with physical measurements.

                SIMULATION
                    │
                    ▼
             Predicted Temp.
                    │
                    │
                    ▼
                Comparison
                    ▲
                    │
             Measured Temp.
                    │
                    ▲
                TEST DATA

Correlation should use physically comparable conditions.

Relevant factors include:

heat input
ambient conditions
component geometry
sensor location
thermal interface
insulation configuration
heater state
28. Existing HIFLY Measurement Dataset

The currently available recorded dataset contains current, voltage, and temperature values.

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

The available records do not establish timestamps.

Therefore, the reading number is an index and must not be interpreted as elapsed time.

29. Electrical Operating Point for Thermal Correlation

The recorded dataset contains an operating point of:

Voltage: 12 V
Current: 1.54 A
Temperature: 28

The corresponding instantaneous electrical power is:

$$ P = V \times I $$ $$ P = 12 \times 1.54 $$ $$ P = 18.48\ W $$

This value represents instantaneous electrical power at that recorded operating point.

It is not an energy measurement and does not establish system endurance.

             12 V
              │
              ×
              ▲
              │
            1.54 A
              │
              ▼
        18.48 W Instantaneous
        Electrical Power
30. Measurement Limitations

The available dataset does not contain sufficient information to establish:

elapsed time
energy consumption
battery capacity
runtime
thermal time constant
heating rate
cooling rate
high-altitude operating condition
pressure condition
ambient temperature for each reading
heater state for each reading

These quantities should not be inferred from the nine recorded readings alone.

31. Simulation and PHP Interpretation

The PHP-assisted simulation should be interpreted carefully.

A calculated lower temperature in a PHP-assisted model can demonstrate a difference between the modelled configurations under the specified assumptions.

It does not by itself prove that the physical PHP will produce the same result.

          PHP Simulation
                │
                ▼
       Calculated Behaviour
                │
                ▼
        Prototype PHP
                │
                ▼
       Measured Behaviour
                │
                ▼
            Validation

Physical PHP effectiveness requires corresponding experimental evidence.

32. High-Altitude Boundary Considerations

High-altitude thermal behaviour depends on the environmental boundary conditions represented in the simulation.

Relevant factors include:

ambient temperature
pressure
convection
radiation
external heat transfer
enclosure geometry

A simulation performed with ordinary laboratory boundary conditions should not automatically be labelled as a high-altitude qualification simulation.

        Environmental Model
                 │
       ┌─────────┼─────────┐
       │         │         │
       ▼         ▼         ▼
    Pressure  Temperature Convection
       │         │         │
       └─────────┼─────────┘
                 ▼
           Thermal Model
33. Simulation Limitations

The thermal model is an engineering representation of the physical system.

Possible differences between simulation and physical behaviour can arise from:

simplified geometry
uncertain material properties
thermal-contact assumptions
actual component heat generation
environmental variation
sensor placement
manufacturing variation
PHP internal dynamics
external airflow
battery electrochemical effects

These limitations should be considered when comparing simulation and prototype results.

34. Evidence Classification
Evidence Type	Meaning
CAD	Physical geometry
Thermal simulation	Computed thermal behaviour
Prototype	Physical implementation
Measurement	Recorded physical behaviour
Validation	Comparison of simulation and measurement

The thermal simulation should remain clearly identified as computational evidence.

35. Simulation Status

Simulation subsystem: HIFLY thermal analysis.

Primary analysis: Thermal behaviour of the integrated HIFLY architecture.

Primary thermal components: Battery, electronics, PHP, heater, insulation, enclosure, and thermal interfaces.

Available measurement reference: Recorded current, voltage, and temperature data.

Important limitation: The available measurement data have no documented time axis.

PHP modelling boundary: Conventional thermal FEA represents the PHP-assisted thermal architecture but does not automatically represent detailed pulsating two-phase flow.

36. Traceability to HIFLY Challenges
HIFLY Challenge	Thermal Simulation Contribution
Reduced cooling efficiency	Evaluates the PHP-assisted thermal architecture
Battery degradation	Evaluates battery-region thermal behaviour
Thermal cycling damage	Provides temperature-distribution information
Mission energy management	Allows heater and electrical operating conditions to be considered
Electronics thermal management	Evaluates represented heat-transfer paths
System integration	Evaluates the combined thermal architecture
37. Simulation-to-Testing Workflow
                    HIFLY CAD
                        │
                        ▼
                Thermal Simulation
                        │
                        ▼
                  Simulated Data
                        │
                        ▼
               Physical Prototype
                        │
                        ▼
                Thermal Testing
                        │
                        ▼
                 Measured Data
                        │
                        ▼
                    Comparison
                        │
                        ▼
              Engineering Evaluation
                        │
                        ▼
                  Design Iteration

This workflow provides a traceable path from digital design to computational analysis and physical evidence.

38. Overall Thermal Simulation Architecture
                 ┌───────────────────────┐
                 │       HIFLY CAD       │
                 └───────────┬───────────┘
                             │
                             ▼
                    Thermal Simulation
                             │
       ┌─────────────────────┼─────────────────────┐
       │                     │                     │
       ▼                     ▼                     ▼
    Battery                 PHP                  Heater
       │                     │                     │
       └─────────────────────┼─────────────────────┘
                             │
                             ▼
                         Insulation
                             │
                             ▼
                          Enclosure
                             │
                             ▼
                      Thermal Solution
                             │
          ┌──────────────────┼──────────────────┐
          │                  │                  │
          ▼                  ▼                  ▼
     Temperature          Heat Flux        Temperature
     Distribution                           Gradient
          │                  │                  │
          └──────────────────┼──────────────────┘
                             ▼
                       Prototype Data
                             │
                             ▼
                         Comparison
                             │
                             ▼
                       HIFLY Validation

The HIFLY thermal simulation subsystem provides the computational layer connecting the CAD architecture with experimental thermal evaluation while maintaining a clear distinction between simulated results and measured physical evidence.
