# HIFLY — Hardware Overview

## 1. Hardware System Overview

HIFLY is built around a combination of thermal-management hardware, battery hardware, environmental-protection structures, sensing electronics, control electronics, switching hardware, and communication hardware.

The hardware architecture provides the physical implementation for the seven high-altitude reliability challenges addressed by the HIFLY system.

The major hardware groups are:

| Hardware Group | Major Elements | Primary Function |
|---|---|---|
| Thermal Management | Pulsating Heat Pipe, thermal insulation, heating element | Thermal transport and temperature control |
| Battery System | Li-ion battery pack, battery protection and thermal elements | Electrical energy storage and battery thermal management |
| Sensing | Temperature, voltage, current measurement | System-state measurement |
| Control | ESP32-based controller | Processing and control |
| Switching | MOSFET switching stage | Heater power control |
| Environmental Protection | Protection chamber, enclosure, flexible silicone, protective layer | Environmental and mechanical protection |
| Communication | LilyGO T3-S3, LoRa, antenna arrangement | Wireless telemetry |
| Ground Interface | GCS communication hardware | System monitoring and operator interaction |

The overall hardware relationship is:

```text
                    HIGH-ALTITUDE ENVIRONMENT
                              │
                              ↓
                 ┌─────────────────────────┐
                 │ ENVIRONMENTAL PROTECTION│
                 │                         │
                 │ Enclosure               │
                 │ Insulation              │
                 │ Protection Chamber      │
                 │ Flexible Silicone       │
                 │ Protective Layer        │
                 └────────────┬────────────┘
                              │
                              ↓
                 ┌─────────────────────────┐
                 │ THERMAL MANAGEMENT      │
                 │                         │
                 │ PHP                     │
                 │ Heating Element         │
                 │ Thermal Insulation      │
                 └────────────┬────────────┘
                              │
                              ↓
                 ┌─────────────────────────┐
                 │ BATTERY SYSTEM           │
                 │                         │
                 │ Li-ion Battery          │
                 │ Thermal Management      │
                 │ Electrical Monitoring   │
                 └────────────┬────────────┘
                              │
                              ↓
                 ┌─────────────────────────┐
                 │ SENSOR SYSTEM            │
                 │                         │
                 │ Temperature             │
                 │ Voltage                 │
                 │ Current                 │
                 └────────────┬────────────┘
                              │
                              ↓
                 ┌─────────────────────────┐
                 │ ESP32 CONTROL SYSTEM     │
                 │                         │
                 │ Thermal Control         │
                 │ Power Monitoring        │
                 │ Safety Logic             │
                 └────────────┬────────────┘
                              │
                    ┌─────────┴─────────┐
                    ↓                   ↓
             MOSFET / HEATER       LilyGO T3-S3
             CONTROL SYSTEM             │
                                        ↓
                                      LoRa
                                        │
                                        ↓
                                       GCS
```
2. Thermal Management Hardware
2.1 Pulsating Heat Pipe

The Pulsating Heat Pipe is a central component of the passive thermal-management architecture.

The PHP provides a thermal pathway between the heat-source region and the heat-rejection region.

Its functional arrangement is:

                    HEAT SOURCE
                         │
                         ↓
                ┌────────────────┐
                │ PHP EVAPORATOR │
                └───────┬────────┘
                        │
                        │ Heat Transport
                        ↓
                ┌────────────────┐
                │ PHP CONDENSER  │
                └───────┬────────┘
                        │
                        ↓
                  HEAT REJECTION

The PHP is integrated with the thermal structure rather than functioning as an isolated component.

The PHP hardware works together with the enclosure, insulation, heat-source interface, and thermal-management geometry.

2.2 Heating Element

The heating element provides active thermal correction.

The heater is controlled through an electronic switching stage connected to the ESP32 controller.

ESP32
  │
  ↓
Control Signal
  │
  ↓
MOSFET Switching Stage
  │
  ↓
Heating Element
  │
  ↓
Protected Thermal Region

The heating element forms the active part of the thermal-management system.

Its operation is controlled using temperature information from the sensing system.

2.3 Thermal Insulation

Thermal insulation surrounds the appropriate protected regions of the system.

Its purpose is to reduce unwanted heat transfer between the protected electronics and the external environment.

       EXTERNAL ENVIRONMENT
                │
                ↓
        ┌───────────────┐
        │ Insulation    │
        │ Layer         │
        └───────┬───────┘
                │
                ↓
       Protected Electronics

The insulation works together with the heater and PHP.

The combined thermal architecture is:
```

                 THERMAL SYSTEM
                       │
          ┌────────────┴────────────┐
          ↓                         ↓
     Passive Control           Active Control
          │                         │
     ┌────┴────┐                    │
     ↓         ↓                    ↓
 Insulation   PHP                Heater
     │         │                    │
     └─────────┴──────────┬─────────┘
                          ↓
                   Thermal Condition
```
3. Battery Hardware
3.1 Li-ion Battery Pack

The Li-ion battery pack provides the electrical energy required by the HIFLY system.

The battery supplies power to:

ESP32 controller
Temperature sensors
Voltage and current monitoring
Heating element
LoRa communication hardware
Other connected electrical loads

The battery is also part of the thermal-management system because its operating condition is affected by temperature.

3.2 Battery Thermal Management

The battery thermal subsystem combines physical thermal protection with active monitoring and heating.

                    LI-ION BATTERY
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ↓                ↓                ↓
     Temperature        Voltage          Current
       Sensor           Monitor           Monitor
          │                │                │
          └────────────────┼────────────────┘
                           ↓
                    ESP32 CONTROLLER
                           │
                           ↓
                    THERMAL CONTROL
                           │
                           ↓
                     HEATER CONTROL
                           │
                           ↓
                  BATTERY THERMAL STATE

This arrangement allows the battery to be evaluated using thermal and electrical measurements.

4. Sensing Hardware
4.1 Temperature Sensing

Temperature sensors provide the primary thermal feedback for HIFLY.

Temperature information is used by the ESP32 controller to determine the thermal condition of the protected system.

The measurement path is:

Thermal Region
      │
      ↓
Temperature Sensor
      │
      ↓
ESP32
      │
      ↓
Thermal Control Logic
      │
      ↓
Heater

Temperature measurements are also transmitted through the telemetry system for GCS monitoring.

4.2 Voltage Monitoring

Voltage monitoring provides information about the electrical supply condition.

Battery
  │
  ↓
Voltage Measurement
  │
  ↓
ESP32
  │
  ├── Power Monitoring
  └── Telemetry

Voltage information is available to the control and monitoring architecture.

4.3 Current Monitoring

Current monitoring provides information about electrical load.

Battery / Load
      │
      ↓
Current Measurement
      │
      ↓
ESP32
      │
      ├── Load Monitoring
      ├── Power Monitoring
      └── Telemetry

Current measurement is particularly relevant when the heater is operating because active thermal control introduces an electrical load.

4.4 Combined Sensor Architecture

The three primary measurements are integrated into the central controller.

Temperature ────────┐
                    │
Voltage ────────────┤
                    │
Current ────────────┤
                    ↓
             ESP32 CONTROLLER
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
     Thermal      Power       Safety
     Control    Monitoring     Logic
        │           │           │
        └───────────┼───────────┘
                    ↓
              LoRa Telemetry
5. Control Hardware
5.1 ESP32 Controller

The ESP32-based controller is the central processing unit of the HIFLY hardware architecture.

It receives:

Temperature measurements
Voltage measurements
Current measurements
Operating commands
Communication information

It provides:

Thermal-control decisions
Heater-control signals
Power-state information
Safety-state information
Telemetry data

The controller architecture is:
```

                 SENSOR INPUTS
                      │
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
   Temperature     Voltage       Current
        │             │             │
        └─────────────┼─────────────┘
                      ↓
                ESP32 CONTROLLER
                      │
          ┌───────────┼───────────┐
          ↓           ↓           ↓
       Thermal       Power       Safety
       Control     Monitoring     Logic
          │           │           │
          └───────────┼───────────┘
                      ↓
             Communication System
```
5.2 MOSFET Switching Stage

The MOSFET switching stage provides the electrical interface between the low-power controller and the heating element.

ESP32 GPIO
    │
    ↓
Control Signal
    │
    ↓
MOSFET
    │
    ↓
Heating Element
    │
    ↓
Battery Power

The MOSFET stage allows the ESP32 to control the heater without directly supplying the heater load from a microcontroller output.

6. Environmental Protection Hardware
6.1 Protection Chamber

The protection chamber provides a physical enclosure for sensitive electrical and electronic components.

The chamber forms part of the low-pressure and environmental-protection architecture.

             EXTERNAL ENVIRONMENT
                      │
                      ↓
              Protective Structure
                      │
                      ↓
             ┌──────────────────┐
             │ Protected Chamber│
             │                  │
             │ ESP32            │
             │ Sensors          │
             │ Power Electronics│
             │ Communication    │
             └──────────────────┘

The protected chamber is integrated with thermal insulation and other enclosure elements.

6.2 Flexible Silicone Protection

Flexible silicone protection is incorporated around selected interfaces.

It provides a compliant layer between rigid components and sensitive interfaces.

Rigid Structure
      │
      ↓
Flexible Silicone
      │
      ↓
Protected Interface
      │
      ↓
Electronic Assembly

This element forms part of the protection strategy against repeated thermal expansion and contraction.

6.3 Lightweight Protective Layer

A lightweight protective layer forms part of the environmental protection surrounding sensitive electronics.

External Environment
        │
        ↓
Lightweight Protective Layer
        │
        ↓
Enclosure / Structural Layer
        │
        ↓
Thermal Protection
        │
        ↓
Sensitive Electronics

The protective layer is integrated into the physical architecture while remaining compatible with the requirements of an airborne platform.

No unsupported quantitative radiation-shielding performance is assigned to the material.

7. Communication Hardware
7.1 LilyGO T3-S3

The LilyGO T3-S3 provides the LoRa-based communication interface used by HIFLY.

The module connects the onboard controller to the wireless telemetry system.

ESP32 / Onboard Controller
          │
          ↓
     LilyGO T3-S3
          │
          ↓
       LoRa Link
          │
          ↓
    Ground Receiver
          │
          ↓
         GCS

The communication subsystem provides telemetry for system monitoring.

7.2 Antenna and Protection

The communication hardware includes an antenna arrangement integrated with the physical protection architecture.

The antenna system is positioned to maintain the required wireless communication path while remaining compatible with the enclosure and environmental-protection structure.

The relationship is:

LilyGO T3-S3
      │
      ↓
Antenna Connection
      │
      ↓
Protected Antenna Arrangement
      │
      ↓
Wireless LoRa Link
8. Power Distribution

The battery provides the primary electrical source for the onboard hardware.

The power architecture can be represented as:
```

                       LI-ION BATTERY
                              │
                              ↓
                    POWER DISTRIBUTION
                              │
       ┌──────────────────────┼──────────────────────┐
       │                      │                      │
       ↓                      ↓                      ↓
    ESP32                 Sensors               Heater
       │                      │                      │
       │                      │                      │
       ↓                      ↓                      ↓
  Control System       Measurements          Thermal Control
       │
       ↓
  LilyGO T3-S3
       │
       ↓
   LoRa System
```

The current and voltage monitoring system provides electrical-state information from the power architecture.

9. Hardware Interconnection

The major hardware components are connected through the following system structure:

                         LI-ION BATTERY
                                │
                ┌───────────────┼───────────────┐
                │               │               │
                ↓               ↓               ↓
             ESP32          Heater Power     Sensors
                │               │               │
                │               ↓               │
                │          MOSFET Stage        │
                │               │               │
                │               ↓               │
                │          Heating Element      │
                │                               │
                └───────────────┬───────────────┘
                                │
                                ↓
                         Thermal System
                                │
                         ┌──────┴──────┐
                         ↓             ↓
                        PHP       Insulation
                         │             │
                         └──────┬──────┘
                                │
                                ↓
                      Protected Electronics
                                │
                                ↓
                         LilyGO T3-S3
                                │
                                ↓
                             LoRa
                                │
                                ↓
                              GCS
10. Thermal Hardware Arrangement

The main thermal hardware is organized around the protected electronics.

                 EXTERNAL ENVIRONMENT
                          │
                          ↓
                 ┌─────────────────┐
                 │ Protective Layer│
                 └────────┬────────┘
                          │
                          ↓
                 ┌─────────────────┐
                 │ Enclosure       │
                 └────────┬────────┘
                          │
                          ↓
                 ┌─────────────────┐
                 │ Insulation      │
                 └────────┬────────┘
                          │
             ┌────────────┴────────────┐
             │                         │
             ↓                         ↓
      Protected Electronics           PHP
             │                         │
             │                         ↓
             │                   Thermal Path
             │
             ↓
       Temperature Sensor
             │
             ↓
            ESP32
             │
             ↓
       MOSFET Switching
             │
             ↓
        Heating Element

This arrangement connects passive thermal management, active heating, sensing, and control.

11. Hardware Functional Mapping
Hardware Component	Function	Connected Subsystem
Li-ion Battery	Electrical energy storage	Power / BTMS
Pulsating Heat Pipe	Passive heat transfer	Thermal Management
Thermal Insulation	Reduces heat transfer	Thermal Management
Heating Element	Active heating	Thermal Management
Temperature Sensor	Thermal measurement	Sensing / Control
Voltage Monitor	Supply measurement	Power Monitoring
Current Monitor	Load measurement	Power Monitoring
MOSFET	Heater switching	Control / Power
ESP32	Central processing	Control
Protection Chamber	Environmental protection	Mechanical / Electrical
Flexible Silicone	Interface protection	Mechanical / Thermal
Lightweight Protective Layer	Environmental protection	Structural
LilyGO T3-S3	LoRa communication	Communication
Antenna	Wireless transmission	Communication
GCS Receiver	Telemetry reception	Ground System
12. Hardware Support for the Seven Subproblems
High-Altitude Subproblem	Hardware Response
Reduced cooling efficiency	Pulsating Heat Pipe
Insulation breakdown and electrical arcing	Protection chamber and protected electrical architecture
Battery degradation	Li-ion battery thermal-management hardware
Thermal cycling damage	Flexible silicone protection
Increased radiation exposure	Lightweight protective layer
Communication system effects	LilyGO T3-S3, LoRa, antenna protection
Mission energy and endurance	Battery, voltage/current monitoring, controlled heater

13. Hardware–Software Boundary

The hardware provides physical measurements and executes the commands generated by the control software.
```

                    HARDWARE
                       │
       ┌───────────────┼────────────────┐
       │               │                │
       ↓               ↓                ↓
   Sensors          Power          Communication
       │               │                │
       └───────────────┼────────────────┘
                       ↓
                  ESP32 SOFTWARE
                       │
       ┌───────────────┼────────────────┐
       ↓               ↓                ↓
 Thermal Control   Power Logic      Safety Logic
       │               │                │
       └───────────────┼────────────────┘
                       ↓
                    HARDWARE
                       │
          ┌────────────┴────────────┐
          ↓                         ↓
       Heater                   Telemetry
```

The hardware and software therefore form a closed control system.

14. Physical System Architecture

The complete physical arrangement can be represented as:
```

                         HIFLY HARDWARE
                              │
          ┌───────────────────┼───────────────────┐
          │                   │                   │
          ↓                   ↓                   ↓
     THERMAL SYSTEM       POWER SYSTEM      COMMUNICATION
          │                   │                   │
     ┌────┼────┐         ┌────┼────┐         ┌────┴────┐
     ↓    ↓    ↓         ↓    ↓    ↓         ↓         ↓
    PHP Ins. Heater    Battery Volt Current LilyGO   Antenna
                              │
                              ↓
                       ESP32 CONTROLLER
                              │
                    ┌─────────┼─────────┐
                    ↓         ↓         ↓
                 Sensors    Heater    LoRa
                              │         │
                              ↓         ↓
                         Thermal      GCS
                         Control
```
15. Hardware Architecture Summary

The HIFLY hardware system combines thermal, electrical, environmental, sensing, control, and communication components into one integrated platform.

The main hardware chain is:

ENVIRONMENT
    ↓
PROTECTION STRUCTURE
    ↓
THERMAL MANAGEMENT
    ↓
BATTERY / POWER SYSTEM
    ↓
SENSORS
    ↓
ESP32 CONTROLLER
    ↓
MOSFET / HEATER CONTROL
    ↓
LILYGO T3-S3 / LoRa
    ↓
GROUND CONTROL SYSTEM

The hardware architecture supports the complete HIFLY reliability concept by connecting passive thermal protection, active heating, battery management, environmental protection, system sensing, autonomous control, electrical monitoring, and communication.

The resulting hardware platform provides the physical foundation for the HIFLY software, CAD, simulation, testing, data, and GCS systems.
