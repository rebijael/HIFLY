# HIFLY — Wiring & Electrical Connections

## 1. Purpose

This folder documents the electrical connections between the HIFLY battery, sensors, heater, MOSFET switching stage, LilyGO T3-S3 LoRa controller and other electrical subsystems.

The wiring documentation should match the actual prototype hardware.

---

## 2. Main Electrical Architecture

```text
                    Li-ion Battery
                         │
              ┌──────────┼──────────┐
              │          │          │
              ↓          ↓          ↓
          Controller   Sensors    Heater
              │          │          │
              │          │          ↓
              │          │       MOSFET
              │          │          │
              │          │          ↓
              │          │    Heating Element
              │          │
              └────┬─────┘
                   │
                   ↓
            LilyGO T3-S3 LoRa
                   │
                   ↓
              LoRa Link
                   │
                   ↓
                  GCS
```

---

## 3. Battery Connections

The battery provides electrical power to the onboard system according to the implemented power-distribution design.

The final wiring should document:

- Battery positive connection
- Battery negative connection
- Controller power input
- Sensor power connections
- Heater power path
- Common ground connections
- Protection elements, where implemented

---

## 4. Temperature Sensor Connection

The temperature sensor is connected to the onboard controller for battery temperature measurement.

```text
Temperature Sensor
       │
       ├── Power
       ├── Ground
       └── Signal
              │
              ↓
       LilyGO T3-S3
```

The exact GPIO/pin assignment must be updated according to the actual firmware and hardware configuration.

---

## 5. Voltage Monitoring Connection

The voltage-monitoring circuit provides battery voltage information to the controller.

```text
Battery
   │
   ↓
Voltage Monitoring Circuit
   │
   ↓
LilyGO T3-S3
```

The final circuit must ensure that the controller input remains within its permitted electrical range.

The exact circuit values and pin assignment should be documented using the implemented hardware.

---

## 6. Current Monitoring Connection

The current-monitoring circuit measures the electrical current associated with the monitored system.

```text
Battery / Load Path
        │
        ↓
Current Sensor
        │
        ↓
LilyGO T3-S3
```

The exact sensor arrangement depends on whether the measurement is intended to represent total system current, heater current or another defined load.

The implemented measurement configuration must be documented here.

---

## 7. Heater and MOSFET Connection

The heater is controlled using a MOSFET switching stage.

```text
                 Battery Supply
                       │
                       │
                       ↓
                 Heating Element
                       │
                       ↓
                    MOSFET
                       │
                       ↓
                     GND


LilyGO T3-S3
      │
      ↓
 MOSFET Gate
      │
      ↓
 Heater ON / OFF
```

The controller provides the MOSFET control signal.

The MOSFET switches the electrical power delivered to the heating element.

---

## 8. Controller Connections

The LilyGO T3-S3 LoRa acts as the central controller.

The controller interfaces with:

- Temperature sensor
- Voltage monitoring
- Current monitoring
- MOSFET/heater control
- LoRa communication

```text
                 LilyGO T3-S3
                      │
       ┌──────────────┼──────────────┐
       │              │              │
       ↓              ↓              ↓
 Temperature      Voltage        Current
   Sensor         Monitor         Sensor
       │
       │
       ↓
 Thermal Control
       │
       ↓
     MOSFET
       │
       ↓
     Heater
```

---

## 9. LoRa Communication

The LilyGO T3-S3 provides the onboard LoRa communication interface.

```text
Onboard Controller
        │
        ↓
       LoRa
        │
        ↓
Ground Control Station
```

The communication layer is intended to transmit relevant system measurements and status information to the GCS.

---

## 10. Ground Control Station Data

The onboard system can provide the following information to the GCS:

- Battery temperature
- Ambient temperature, where implemented
- Voltage
- Current
- Heater status
- Thermal status
- LoRa link status
- Operating mode
- Safety status
- Alerts

---

## 11. Fail-Safe Electrical Control

The onboard thermal-control function should not depend entirely on the availability of the LoRa/GCS link.

```text
                Sensors
                   │
                   ↓
            LilyGO T3-S3
                   │
                   ↓
          Thermal Control Logic
                   │
                   ↓
                MOSFET
                   │
                   ↓
                Heater
```

During communication loss, the onboard controller can continue the implemented autonomous thermal-control behaviour.

The exact thresholds and control behaviour must match the actual firmware.

---

## 12. Wiring Documentation Requirements

The final wiring documentation should include:

1. Complete circuit diagram
2. Controller pin assignments
3. Sensor connections
4. Heater connections
5. MOSFET connections
6. Power-distribution path
7. Ground connections
8. Communication connections
9. Protection components
10. Photographs of the physical wiring

---

## 13. Pin Assignment Table

The exact pin numbers must be filled using the actual prototype configuration.

| Function | Device | LilyGO T3-S3 Pin | Status |
|---|---|---|---|
| Battery Temperature | Temperature Sensor | TBD | To be confirmed |
| Battery Voltage | Voltage Monitor | TBD | To be confirmed |
| Battery Current | Current Sensor | TBD | To be confirmed |
| Heater Control | MOSFET | TBD | To be confirmed |
| LoRa | LilyGO T3-S3 | Integrated | Implemented |
| GCS Communication | LoRa | Integrated | Development |

> Do not guess GPIO assignments. Update this table from the actual wiring and firmware.

---

## 14. Physical Wiring Evidence

Physical wiring photographs should be stored in:

```text
10_Media/prototype_photos/
```

Recommended photographs include:

- Complete electronics assembly
- Battery connections
- Sensor connections
- Heater/MOSFET wiring
- Controller connections
- LoRa antenna connection
- Final integrated wiring

---

## 15. Safety Considerations

Before powering the prototype:

- Verify battery polarity.
- Verify common-ground connections.
- Check for short circuits.
- Verify heater wiring.
- Verify MOSFET connections.
- Confirm controller input limits.
- Confirm sensor wiring.
- Confirm voltage-monitoring scaling.
- Confirm current-sensor connections.
- Inspect exposed conductors and insulation.

All electrical connections should be checked against the actual circuit before operation.

---

## 16. Wiring Status

The wiring documentation is considered complete only when the documented circuit matches the physical prototype.

Current documentation status:

**Development / To be updated with final prototype wiring.**

---

## 17. Related Documentation

Hardware overview:

```text
03_Hardware/hardware_overview.md
```

Bill of materials:

```text
03_Hardware/bill_of_materials.md
```

Software:

```text
04_Software/
```

Testing:

```text
07_Testing/
```

Data:

```text
08_Data/
```
