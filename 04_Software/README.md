# HIFLY — Wiring and Electrical Interconnection

## 1. Overview

The HIFLY wiring architecture connects the battery, sensing system, ESP32 controller, heater switching stage, heating element, LilyGO T3-S3 communication system, and supporting electrical hardware into one integrated system.

The wiring architecture follows the functional structure of the HIFLY control system:

```text
                    LI-ION BATTERY
                           │
                           ↓
                  POWER DISTRIBUTION
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ↓                ↓                ↓
       ESP32             Sensors          Heater
          │                │                │
          │                │                ↓
          │                │              MOSFET
          │                │                │
          │                │                ↓
          │                │          Heating Element
          │                │
          ├────────────────┤
          │                │
          ↓                ↓
      LilyGO T3-S3    Voltage / Current
          │             Monitoring
          ↓
        LoRa
          │
          ↓
         GCS
```
The wiring system provides the electrical connection required for temperature monitoring, power monitoring, thermal control, and telemetry.

2. Main Electrical Architecture

The main electrical relationship is:

                         LI-ION BATTERY
                                │
                                ↓
                       POWER DISTRIBUTION
                                │
             ┌──────────────────┼──────────────────┐
             │                  │                  │
             ↓                  ↓                  ↓
           ESP32             Sensors            MOSFET
             │                  │                  │
             │                  │                  ↓
             │                  │              HEATER
             │                  │
             │                  │
             ↓                  ↓
       LilyGO T3-S3       Voltage / Current
             │              Monitoring
             ↓
           LoRa
             │
             ↓
            GCS

The battery is the primary electrical source, while the ESP32 acts as the central control element.

3. Battery Connection

The Li-ion battery provides the electrical supply for the HIFLY onboard system.

The battery power path is:

LI-ION BATTERY
      │
      ↓
POWER DISTRIBUTION
      │
      ├──────────────→ ESP32
      │
      ├──────────────→ Sensors
      │
      ├──────────────→ LilyGO T3-S3
      │
      └──────────────→ Heater Power

The heater is treated as a power load controlled through the MOSFET switching stage.

4. ESP32 Controller Connections

The ESP32 is the central electrical and control interface.

The controller receives measurement signals and produces control and communication outputs.
```

                ┌─────────────────────┐
                │        ESP32        │
                │     CONTROLLER      │
                └──────────┬──────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ↓                  ↓                  ↓
 Temperature          Voltage / Current    MOSFET
   Inputs                Inputs            Output
        │                  │                  │
        ↓                  ↓                  ↓
 Temperature          Power Monitoring      Heater
   Sensors
                           │
                           ↓
                     LilyGO T3-S3
                           │
                           ↓
                         LoRa

```

The ESP32 performs the central processing required for thermal control and system monitoring.

5. Temperature Sensor Wiring

Temperature sensors provide the feedback required for the thermal-control loop.

The basic connection is:

TEMPERATURE SENSOR
        │
        │ Sensor Signal
        ↓
      ESP32
        │
        ↓
Thermal Evaluation
        │
        ↓
Heater Control

The temperature sensor is electrically connected to the controller through the appropriate sensor interface.

The measured temperature is used for:

Thermal-state monitoring
Heater control
Thermal-status reporting
GCS telemetry
6. Battery Temperature Wiring

Battery temperature is monitored separately as part of the Battery Thermal Management System.

LI-ION BATTERY
      │
      ↓
BATTERY TEMPERATURE SENSOR
      │
      ↓
ESP32
      │
      ↓
Battery Thermal Evaluation
      │
      ↓
Heater Control

The battery thermal measurement becomes part of the overall thermal-control data.

7. Voltage Measurement Wiring

Voltage monitoring provides the controller with the electrical supply condition.

BATTERY
   │
   ↓
VOLTAGE MEASUREMENT
   │
   ↓
ESP32
   │
   ├────────→ Power Status
   │
   └────────→ GCS Telemetry

The measured voltage is associated with the electrical state of the system.

8. Current Measurement Wiring

Current monitoring provides information about electrical load.

BATTERY / LOAD PATH
       │
       ↓
CURRENT MEASUREMENT
       │
       ↓
ESP32
       │
       ├────────→ Load Monitoring
       │
       ├────────→ Power Monitoring
       │
       └────────→ GCS Telemetry

Current information is particularly relevant to heater operation because the heating element introduces an active electrical load.

9. Heater Switching Wiring

The heating element is controlled through a MOSFET switching stage.

The electrical control path is:

                    ESP32
                      │
                      │ Control Signal
                      ↓
                  MOSFET GATE
                      │
                      ↓
                    MOSFET
                      │
             ┌────────┴────────┐
             │                 │
             ↓                 ↓
        Battery Power      Heating Element
             │                 │
             └────────┬────────┘
                      ↓
                 Thermal Load

The MOSFET provides the switching interface between the low-power controller and the heater load.

10. Heater Control Loop

The heater forms part of a closed-loop thermal system.

Protected Thermal Region
          │
          ↓
Temperature Sensor
          │
          ↓
ESP32 Controller
          │
          ↓
Thermal Control Logic
          │
          ↓
MOSFET
          │
          ↓
Heating Element
          │
          ↓
Protected Thermal Region

This loop allows measured thermal conditions to influence heater operation.

11. LilyGO T3-S3 Wiring

The LilyGO T3-S3 provides the LoRa communication interface.

The communication connection is:

ESP32 / Controller
       │
       ↓
LilyGO T3-S3
       │
       ↓
LoRa Radio
       │
       ↓
Antenna
       │
       ↓
Wireless Link
       │
       ↓
Ground Station

The LilyGO T3-S3 is integrated into the onboard communication architecture.

12. Telemetry Data Path

The wiring and control system provides the telemetry path from physical measurements to the ground station.

Temperature Sensors
       │
       ├─────────────┐
       │             │
Voltage Monitor     │
       │             │
       ├─────────────┤
       │             │
Current Monitor     │
       │             │
       └──────┬──────┘
              ↓
        ESP32 CONTROLLER
              │
              ↓
        TELEMETRY DATA
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
13. Ground Control Parameters

The onboard wiring supports transmission of the following system parameters:

Parameter	Source
Battery Temperature	Battery temperature sensor
Ambient Temperature	Temperature sensor
Voltage	Voltage monitoring circuit
Current	Current monitoring circuit
Heater Status	ESP32 / heater-control state
Thermal Status	ESP32 thermal logic
LoRa Link	Communication system
Operating Mode	Controller state
Safety Status	ESP32 safety logic

These parameters form the primary electrical and thermal telemetry set.

14. Power and Signal Separation

The HIFLY wiring architecture separates high-power loads from low-power control signals.

              LOW-POWER CONTROL
                     │
                     ↓
                   ESP32
                     │
             Control Signal
                     │
                     ↓
                   MOSFET
                     │
                     ↓
              HIGH-POWER LOAD
                     │
                     ↓
              Heating Element

The controller generates the switching command while the power circuit supplies the heater.

This prevents the heating load from being driven directly from a microcontroller output.

15. Common Ground and Electrical Reference

The sensing, controller, and communication circuits operate through a common electrical reference appropriate to the implemented power architecture.

The basic relationship is:

Battery Ground
     │
     ├────────→ ESP32 Ground
     │
     ├────────→ Sensor Ground
     │
     ├────────→ Monitoring Circuit Ground
     │
     ├────────→ MOSFET Control Ground
     │
     └────────→ Communication Ground

The actual electrical implementation follows the grounding arrangement of the selected hardware and power circuitry.

16. Wiring of the Thermal System

The thermal system contains both electrical and passive elements.

                    THERMAL SYSTEM
                          │
          ┌───────────────┼───────────────┐
          │                               │
          ↓                               ↓
         PHP                           Heater
          │                               │
          │                         MOSFET Control
          │                               │
          │                               ↑
          │                               │
          └──────────→ Thermal Region ←───┘
                              │
                              ↓
                     Temperature Sensor
                              │
                              ↓
                            ESP32

The PHP does not require an electrical control signal for its passive heat-transfer function.

The heater is electrically controlled.

17. Battery and Heater Power Relationship

The battery supplies the energy required by the active thermal system.

                     LI-ION BATTERY
                            │
                            ↓
                    POWER DISTRIBUTION
                            │
             ┌──────────────┴──────────────┐
             │                             │
             ↓                             ↓
        Electronics                     Heater
             │                             │
             ↓                             ↓
          ESP32                         MOSFET
             │                             │
             ↓                             ↓
      Sensors / LoRa                 Heating Element

Voltage and current monitoring provides visibility into the electrical condition of this system.

18. Communication Wiring

The communication subsystem follows the onboard controller to LoRa path.

ESP32
  │
  ↓
LilyGO T3-S3
  │
  ↓
LoRa Transceiver
  │
  ↓
Antenna
  │
  ↓
Wireless Telemetry

The communication wiring is physically integrated with the protected electronics and antenna structure.

19. Safety-Related Wiring Architecture

Safety-relevant signals are processed onboard.
```

Temperature
     │
     ├──────────────┐
Voltage             │
     │              │
Current             │
     │              │
     └──────┬───────┘
            ↓
      ESP32 CONTROLLER
            │
            ↓
       SAFETY LOGIC
            │
       ┌────┴────┐
       ↓         ↓
  Heater State  System State
       │         │
       ↓         ↓
     MOSFET     Telemetry
```
The communication system provides system-state information, while the onboard controller remains responsible for local control logic.

20. Communication-Loss Electrical Behaviour

The wiring architecture keeps the thermal-control path onboard.

                 NORMAL STATE
                      │
                      ↓
                   ESP32
                      │
             ┌────────┴────────┐
             ↓                 ↓
          Heater            LilyGO
          Control              │
                               ↓
                              LoRa
                               │
                               ↓
                              GCS

If the communication path becomes unavailable:

                 LoRa LINK LOST
                      │
                      ↓
                   ESP32
                      │
          ┌───────────┴───────────┐
          ↓                       ↓
    Thermal Control          Safety Logic
          │                       │
          ↓                       ↓
       Heater                  System State

The local thermal-control wiring therefore remains independent of the wireless telemetry path.

21. Wiring Architecture for the Seven Subproblems
Subproblem	Wiring / Electrical Function
Reduced Cooling Efficiency	PHP integrated into thermal structure
Insulation Breakdown and Electrical Arcing	Protected electrical enclosure and wiring
Battery Degradation	Battery temperature sensing and heater control
Thermal Cycling Damage	Flexible protected interfaces
Increased Radiation Exposure	Protected electrical assembly
Communication System Effects	LilyGO T3-S3, LoRa, antenna
Mission Energy and Endurance	Voltage/current monitoring and controlled heater
22. Overall Wiring Architecture

The complete HIFLY wiring architecture can be represented as:

                              LI-ION BATTERY
                                     │
                                     ↓
                            POWER DISTRIBUTION
                                     │
              ┌──────────────────────┼──────────────────────┐
              │                      │                      │
              ↓                      ↓                      ↓
           ESP32                  Sensors                 MOSFET
              │                      │                      │
              │          ┌───────────┼───────────┐          │
              │          │           │           │          ↓
              │          ↓           ↓           ↓       HEATER
              │     Temperature    Voltage     Current       │
              │          │           │           │          │
              │          └───────────┼───────────┘          │
              │                      │                      │
              └──────────────────────┼──────────────────────┘
                                     │
                                     ↓
                              CONTROL SYSTEM
                                     │
                        ┌────────────┴────────────┐
                        │                         │
                        ↓                         ↓
                  Thermal Logic              Power Logic
                        │                         │
                        ↓                         ↓
                    Heater                  Energy State
                        │
                        ↓
                  Thermal System
                        │
                  ┌─────┴─────┐
                  ↓           ↓
                 PHP      Insulation
                  │           │
                  └─────┬─────┘
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
23. Electrical Data Flow

The HIFLY electrical system converts physical measurements into control actions and telemetry.

PHYSICAL CONDITION
       │
       ↓
SENSORS
       │
       ↓
ELECTRICAL SIGNALS
       │
       ↓
ESP32
       │
       ├────────────→ Thermal Control
       │
       ├────────────→ Power Monitoring
       │
       ├────────────→ Safety Logic
       │
       └────────────→ Telemetry
                              │
                              ↓
                         LilyGO T3-S3
                              │
                              ↓
                             LoRa
                              │
                              ↓
                             GCS
24. Wiring Architecture Summary

The HIFLY wiring system provides the electrical connection between the battery, controller, sensors, heater, switching stage, and communication hardware.

The complete control path is:
```
BATTERY
   ↓
POWER DISTRIBUTION
   ↓
SENSORS
   ↓
ESP32
   ↓
THERMAL / POWER / SAFETY LOGIC
   ↓
MOSFET
   ↓
HEATER

The telemetry path is:

SENSORS
   ↓
ESP32
   ↓
LILYGO T3-S3
   ↓
LoRa
   ↓
GCS
```
Together, these two paths create the electrical backbone of the HIFLY system.

The wiring architecture connects thermal control, battery monitoring, power monitoring, safety logic, and telemetry while maintaining the separation between low-power control signals and higher-power heater loads.
