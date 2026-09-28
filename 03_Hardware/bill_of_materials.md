# HIFLY — Bill of Materials

## 1. Overview

The HIFLY Bill of Materials represents the hardware required to implement the integrated thermal-management, battery-management, sensing, control, environmental-protection, and communication architecture.

The system is organized by functional subsystem rather than treating every component as an isolated part.

The major hardware groups are:

1. Thermal Management
2. Battery and Power
3. Sensing and Monitoring
4. Control and Switching
5. Environmental Protection
6. Communication and Telemetry
7. Mechanical Integration

---

## 2. Thermal Management Components

| Item | Component | Function | Subsystem |
|---|---|---|---|
| 1 | Pulsating Heat Pipe (PHP) | Passive heat-transfer pathway | Thermal Management |
| 2 | Thermal Insulation | Reduces unwanted heat transfer | Thermal Management |
| 3 | Heating Element | Active thermal correction | Thermal Management |
| 4 | Thermal Interface / Mounting Structure | Transfers heat between components and thermal hardware | Thermal Management |

The thermal subsystem combines passive and active thermal-management methods.

```text
                 THERMAL MANAGEMENT
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
         PHP        Insulation       Heater
          │              │              │
          └──────────────┼──────────────┘
                         ↓
                Protected Electronics
```
3. Battery and Power Components
Item	Component	Function	Subsystem
1	Li-ion Battery Pack	Primary electrical energy storage	Power
2	Battery Protection / Management Hardware	Electrical protection and battery management	Power
3	Power Distribution Wiring	Distributes battery power to system loads	Power
4	Voltage Monitoring Circuit	Measures electrical supply voltage	Power
5	Current Monitoring Circuit	Measures electrical load current	Power

The battery provides power to the controller, sensors, communication system, and heating system.

                    LI-ION BATTERY
                           │
                           ↓
                  POWER DISTRIBUTION
                           │
          ┌────────────────┼────────────────┐
          ↓                ↓                ↓
       ESP32            Sensors           Heater
          │                │                │
          ↓                ↓                ↓
      Control         Monitoring       Thermal Control
          │
          ↓
      LilyGO T3-S3
          │
          ↓
        LoRa
4. Sensing and Monitoring Components
Item	Component	Measurement / Function
1	Temperature Sensor	Measures thermal condition
2	Battery Temperature Sensor	Measures battery thermal condition
3	Voltage Measurement	Measures electrical supply condition
4	Current Measurement	Measures electrical load
5	Sensor Wiring	Connects measurement devices to the controller

The sensing system provides the data required for thermal control, power monitoring, safety logic, and telemetry.

Temperature ────────┐
                    │
Battery Temperature ┤
                    │
Voltage ────────────┤
                    │
Current ────────────┤
                    ↓
              ESP32 CONTROLLER
                    │
       ┌────────────┼────────────┐
       ↓            ↓            ↓
    Thermal       Power        Safety
    Control     Monitoring      Logic
5. Control and Switching Components
Item	Component	Function
1	ESP32 Controller	Central processing and control
2	MOSFET	Electronic heater switching
3	Gate / Driver Interface	Connects controller output to switching stage
4	Control Wiring	Electrical connection between controller and loads
5	PCB / Prototype Interconnect	Physical electrical integration

The controller receives sensor measurements and generates the control signal for the heater.

Temperature Sensor
        │
        ↓
   ESP32 Controller
        │
        ↓
    Control Signal
        │
        ↓
       MOSFET
        │
        ↓
 Heating Element
6. Environmental Protection Components
Item	Component	Function
1	Protective Enclosure	Physical protection of internal hardware
2	Low-Pressure Protection Chamber	Environmental and electrical protection
3	Thermal Insulation	Thermal separation from external environment
4	Flexible Silicone	Flexible protection around selected interfaces
5	Lightweight Protective Layer	Additional environmental protection
6	Mechanical Fasteners	Structural assembly
7	Sealing / Interface Materials	Protection of selected enclosure interfaces

The environmental-protection system surrounds the sensitive electronics and separates them from external environmental conditions.

              EXTERNAL ENVIRONMENT
                       │
                       ↓
             Protective Layer
                       │
                       ↓
              Enclosure Structure
                       │
                       ↓
                Insulation
                       │
                       ↓
            Protection Chamber
                       │
                       ↓
              Sensitive Electronics
7. Communication Components
Item	Component	Function
1	LilyGO T3-S3	LoRa communication and telemetry
2	LoRa Radio Interface	Wireless data transmission
3	Antenna	Wireless communication
4	Antenna Protection / Radome Structure	Physical protection of antenna
5	Communication Wiring	Electrical interconnection

The communication hardware connects the onboard system to the Ground Control System.

ESP32 Controller
       │
       ↓
LilyGO T3-S3
       │
       ↓
    Antenna
       │
       ↓
   LoRa Link
       │
       ↓
Ground Receiver
       │
       ↓
      GCS
8. Mechanical Integration Components
Item	Component	Function
1	Enclosure Structure	Houses protected electronics
2	PHP Mounting Structure	Positions PHP within thermal assembly
3	Battery Mounting Structure	Secures battery
4	Heater Mounting Structure	Positions heating element
5	Sensor Mounting Points	Maintains sensor placement
6	Fasteners	Mechanical assembly
7	Flexible Interfaces	Accommodates selected thermal/mechanical interfaces

The mechanical components provide the physical structure required to integrate the thermal, electrical, and communication hardware.

9. Functional Bill of Materials
Functional Area	Main Hardware
Thermal Transfer	Pulsating Heat Pipe
Thermal Isolation	Thermal Insulation
Active Heating	Heating Element
Battery	Li-ion Battery Pack
Temperature Monitoring	Temperature Sensors
Voltage Monitoring	Voltage Measurement Circuit
Current Monitoring	Current Measurement Circuit
Main Control	ESP32
Heater Switching	MOSFET
Environmental Protection	Enclosure / Protection Chamber
Thermal-Cycle Protection	Flexible Silicone
Additional Environmental Protection	Lightweight Protective Layer
Telemetry	LilyGO T3-S3
Wireless Communication	LoRa
RF Interface	Antenna
Ground Monitoring	GCS Receiver / Interface
10. Electrical Hardware Relationship

The major electrical components form the following relationship:

                         LI-ION BATTERY
                                │
                                ↓
                       POWER DISTRIBUTION
                                │
             ┌──────────────────┼──────────────────┐
             │                  │                  │
             ↓                  ↓                  ↓
          ESP32              Sensors            MOSFET
             │                  │                  │
             │                  │                  ↓
             │                  │             Heater
             │                  │
             ↓                  ↓
        LilyGO T3-S3       Measurements
             │                  │
             ↓                  │
            LoRa                │
             │                  │
             └────────┬─────────┘
                      ↓
                     GCS
11. Thermal Hardware Relationship

The thermal components operate as one combined subsystem.

                    THERMAL ENVIRONMENT
                            │
                            ↓
                     Thermal Insulation
                            │
                            ↓
                   Protected Electronics
                            │
              ┌─────────────┴─────────────┐
              │                           │
              ↓                           ↓
             PHP                        Heater
              │                           │
              ↓                           ↑
        Heat Transfer                MOSFET Control
              │                           ↑
              └─────────────┬─────────────┘
                            │
                            ↑
                     ESP32 Controller
                            ↑
                            │
                    Temperature Sensor
12. Battery Hardware Relationship

The battery hardware is connected to both the electrical and thermal systems.

                       LI-ION BATTERY
                              │
               ┌──────────────┼──────────────┐
               │              │              │
               ↓              ↓              ↓
            Voltage        Current       Temperature
            Monitor        Monitor          Sensor
               │              │              │
               └──────────────┼──────────────┘
                              ↓
                       ESP32 CONTROLLER
                              │
                              ↓
                       Thermal Control
                              │
                              ↓
                           Heater
13. Communication Hardware Relationship
                SENSOR / CONTROL DATA
                         │
                         ↓
                  ESP32 CONTROLLER
                         │
                         ↓
                    LilyGO T3-S3
                         │
                         ↓
                       LoRa
                         │
                         ↓
                      Antenna
                         │
                         ↓
                 Wireless Telemetry
                         │
                         ↓
                  Ground Receiver
                         │
                         ↓
                        GCS
14. Hardware Supporting the Seven Subproblems
Subproblem	Hardware Components
Reduced Cooling Efficiency	Pulsating Heat Pipe, thermal structure
Insulation Breakdown and Electrical Arcing	Protection chamber, enclosure, insulation
Battery Degradation	Li-ion battery, temperature sensor, heater, insulation
Thermal Cycling Damage	Flexible silicone, mechanical structure
Increased Radiation Exposure	Lightweight protective layer, enclosure
Communication System Effects	LilyGO T3-S3, LoRa, antenna, antenna protection
Mission Energy and Endurance	Li-ion battery, voltage monitor, current monitor, controlled heater
15. Hardware Control Chain

The complete hardware control chain is:
```

                PHYSICAL CONDITION
                        │
                        ↓
                   SENSOR SYSTEM
                        │
                        ↓
                 ESP32 CONTROLLER
                        │
          ┌─────────────┼─────────────┐
          ↓             ↓             ↓
     Heater Control  Power State   Safety State
          │             │             │
          ↓             ↓             ↓
       MOSFET       Monitoring      Protection
          │
          ↓
       HEATER
          │
          ↓
   THERMAL CONDITION
          │
          └──────────────→ SENSOR
```

This forms the physical feedback loop used by HIFLY.

16. Hardware Integration With GCS

The hardware system provides the GCS with telemetry information through the communication subsystem.

The main parameters are:

Battery temperature
Ambient temperature
Voltage
Current
Heater status
Thermal status
LoRa link status
Operating mode
Safety status

The communication chain is:

Sensors
   │
   ↓
ESP32
   │
   ↓
LilyGO T3-S3
   │
   ↓
LoRa
   │
   ↓
Ground Receiver
   │
   ↓
GCS
17. Hardware Architecture Summary

The HIFLY Bill of Materials is organized around a single integrated hardware architecture.

                    HIFLY HARDWARE
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
        ↓                 ↓                 ↓
     THERMAL           POWER          COMMUNICATION
     SYSTEM            SYSTEM             SYSTEM
        │                 │                 │
     PHP / Heater     Battery /          LilyGO /
     Insulation       Monitoring         LoRa
        │                 │                 │
        └─────────────────┼─────────────────┘
                          │
                          ↓
                   SENSOR SYSTEM
                          │
                          ↓
                   ESP32 CONTROL
                          │
                          ↓
              ENVIRONMENTAL PROTECTION
                          │
                          ↓
                  PROTECTED SYSTEM

The hardware architecture provides the physical implementation of the HIFLY reliability concept.

The combination of passive thermal management, active heating, battery thermal management, sensing, ESP32 control, electrical switching, environmental protection, and LoRa communication forms the complete onboard hardware system.
