# HIFLY — Main CAD Assembly

## 1. Purpose

This directory contains the documentation for the main HIFLY mechanical assembly.

The assembly combines the major physical elements of the thermal-management system into a single CAD representation.

The assembly is intended to show:

- Battery integration
- Heating-element placement
- Pulsating Heat Pipe (PHP) integration
- Thermal insulation
- Protection structure
- Sensor placement
- Electrical routing
- Antenna arrangement
- Mechanical supports

---

## 2. Main Assembly

The main assembly should represent the physical relationship between the major HIFLY components.

Conceptual structure:

```text
HIFLY MAIN ASSEMBLY
│
├── Battery Pack
│
├── Heating Element
│
├── Pulsating Heat Pipe
│
├── Thermal Insulation
│
├── Protection Chamber
│
├── Flexible Silicone Protection
│
├── Protective Layer
│
├── Temperature Sensors
│
├── Electrical Monitoring
│
├── Mechanical Supports
│
└── Antenna / Protection
```

The actual geometry should be taken from the current CAD model.

---

## 3. Assembly Layout

A simplified conceptual layout is:

```text
              ┌──────────────────────┐
              │ Protective Layer     │
              └──────────────────────┘
                        │
                        ↓
              ┌──────────────────────┐
              │ Thermal Insulation   │
              └──────────────────────┘
                        │
        ┌───────────────┴───────────────┐
        │                               │
        ↓                               ↓
 ┌───────────────┐               ┌───────────────┐
 │ Heating       │               │ PHP           │
 │ Element       │               │ Assembly      │
 └───────┬───────┘               └───────┬───────┘
         │                                 │
         └────────────┬────────────────────┘
                      ↓
             ┌───────────────────┐
             │   Battery Pack    │
             └───────────────────┘
                      │
                      ↓
             Temperature Sensor
```

This diagram is only a conceptual representation and is not a dimensioned drawing.

---

## 4. Assembly Components

### 4.1 Battery Pack

The battery pack is the primary thermal-management target.

The assembly should document:

- Battery position
- Orientation
- Mounting
- Available clearance
- Sensor contact points
- Heating-element interface
- PHP interface
- Insulation boundary

Actual battery dimensions should be taken from the physical battery or verified CAD model.

---

### 4.2 Heating Element

The heating element is positioned to provide thermal input to the battery system.

The assembly should show:

```text
Heating Element
      ↓
Thermal Contact
      ↓
Battery / Thermal Interface
```

The exact contact arrangement should match the fabricated prototype.

---

### 4.3 Pulsating Heat Pipe

The PHP forms part of the thermal-management architecture.

The assembly should identify:

- PHP location
- Thermal contact region
- Routing
- Support points
- Condenser region
- Clearance from neighbouring components

The CAD assembly documents physical integration.

It does not by itself prove PHP thermal performance.

---

### 4.4 Thermal Insulation

Thermal insulation surrounds or separates selected regions of the thermal-management system.

The assembly should show:

- Insulation boundary
- Thickness where finalized
- Interfaces with the battery
- Interfaces with the heater
- Interfaces with the PHP
- External boundary

The actual material and thickness should be documented separately.

---

### 4.5 Protection Chamber

The protection chamber provides a defined physical enclosure for selected sensitive components.

The assembly should document:

- Chamber geometry
- Component mounting
- Entry/exit points
- Cable routing
- Closure mechanism
- Service access

If the chamber is intended to operate under reduced pressure, the final design should document the actual pressure-control and sealing arrangement separately.

---

### 4.6 Flexible Silicone

Flexible silicone may be integrated around selected interfaces.

Potential functions include:

- Sealing
- Mechanical protection
- Vibration isolation
- Flexible support
- Environmental protection

The actual application must match the prototype.

---

### 4.7 Protective Layer

The outer protective layer is intended to reduce environmental exposure of the protected system.

The final design should identify:

- Material
- Thickness
- Mounting method
- Coverage
- Interfaces

No radiation-protection performance should be claimed without supporting analysis or test evidence.

---

## 5. Sensor Integration

The main assembly should provide sufficient space for temperature and electrical monitoring components.

Example:

```text
Battery
   │
   ├── Temperature Sensor
   │
   ├── Voltage Monitoring
   │
   └── Current Monitoring
```

Sensor placement should be chosen to obtain useful measurements without interfering with the mechanical assembly.

---

## 6. Electrical Routing

The CAD assembly should account for wiring paths.

Potential wiring includes:

```text
Battery
  │
  ├── Power Wiring
  │
  ├── Heater Wiring
  │
  ├── Temperature Sensor
  │
  ├── Voltage/Current Monitoring
  │
  └── Controller
```

The assembly should maintain adequate clearance for connectors and cable routing.

---

## 7. LoRa and Antenna Integration

The LilyGO T3-S3 / LoRa communication hardware requires an antenna arrangement.

The CAD model should identify:

- Controller location
- Antenna location
- Antenna support
- Cable routing where applicable
- Protective cover/radome where applicable

The CAD model should not be used as proof of communication performance.

Communication performance should be established through testing.

---

## 8. Mechanical Supports

The assembly may require supports for:

- Battery
- PHP
- Heating element
- Sensors
- Controller
- Protection chamber
- Antenna
- Insulation

Each support should be checked for:

- Fit
- Clearance
- Accessibility
- Mechanical stability
- Compatibility with the selected fabrication method

---

## 9. Exploded Assembly

An exploded CAD view should separate major components to make the assembly sequence understandable.

Conceptually:

```text
             Protective Layer
                    ↑
                    │
             Thermal Insulation
                    ↑
                    │
             Protection Structure
                    ↑
          ┌─────────┴─────────┐
          │                   │
       PHP Assembly       Heater
          ↑                   ↑
          └─────────┬─────────┘
                    ↑
               Battery Pack
                    ↑
             Mechanical Base
```

The actual exploded view should be generated from the CAD model.

---

## 10. Assembly Sequence

A possible assembly sequence is:

### Step 1 — Prepare Mechanical Structure

Verify the base/support structure.

### Step 2 — Install Battery

Position and secure the battery pack.

### Step 3 — Install Thermal Components

Install:

- Heating element
- PHP
- Thermal interface components

### Step 4 — Install Sensors

Attach temperature sensors and electrical monitoring connections.

### Step 5 — Install Insulation

Install the defined thermal insulation arrangement.

### Step 6 — Install Protection

Install:

- Protection chamber
- Flexible silicone
- Protective layer

### Step 7 — Install Electronics

Mount the controller and related electronics.

### Step 8 — Route Wiring

Complete:

- Sensor wiring
- Heater wiring
- Power wiring
- Communication wiring

### Step 9 — Install Antenna

Install and secure the antenna arrangement.

### Step 10 — Final Inspection

Check:

- Mechanical fit
- Wiring
- Sensor placement
- Insulation
- Thermal interfaces
- Enclosure closure
- Antenna clearance

---

## 11. Clearance Checks

The assembly should be checked for interference between components.

Important checks include:

```text
Battery ↔ Insulation
Battery ↔ Heater
Battery ↔ PHP
PHP ↔ Protection Structure
Heater ↔ Wiring
Sensor ↔ Insulation
Controller ↔ Enclosure
Antenna ↔ Protective Structure
Cable ↔ Mechanical Parts
```

Record any interference found during CAD review.

---

## 12. Assembly Dimensions

Do not enter dimensions based on assumptions.

The following should be populated from the actual CAD model:

| Parameter | Value |
|---|---|
| Overall assembly length | TBD |
| Overall assembly width | TBD |
| Overall assembly height | TBD |
| Battery dimensions | TBD |
| PHP dimensions | TBD |
| Heater dimensions | TBD |
| Insulation thickness | TBD |
| Protection-layer thickness | TBD |
| Enclosure dimensions | TBD |
| Assembly mass | TBD |

---

## 13. Assembly Mass

The total system mass should eventually be determined from actual component masses.

Recommended breakdown:

| Component | Mass |
|---|---:|
| Battery | TBD |
| PHP | TBD |
| Heating Element | TBD |
| Insulation | TBD |
| Protection Structure | TBD |
| Electronics | TBD |
| Sensors | TBD |
| Wiring | TBD |
| Antenna | TBD |
| Mechanical Supports | TBD |
| Other Components | TBD |
| **Total** | **TBD** |

Do not claim a final mass until the components have been measured or reliably documented.

---

## 14. CAD-to-Prototype Comparison

The assembly should be compared against the fabricated prototype.

Recommended evidence:

```text
CAD Assembly
     │
     ↓
Rendered View
     │
     ↓
Physical Prototype
     │
     ↓
Photograph
     │
     ↓
Inspection Notes
```

Any significant difference between the CAD design and prototype should be documented.

---

## 15. Assembly Revision History

Maintain a revision table.

| Revision | Date | Change | Status |
|---|---|---|---|
| A | TBD | Initial assembly | Design |
| B | TBD | Component arrangement update | Design |
| C | TBD | Prototype integration update | Prototype |

Replace these entries with the actual revision history.

---

## 16. CAD File Naming

Recommended assembly file names:

```text
HIFLY_Main_Assembly
HIFLY_Main_Assembly_REV_A
HIFLY_Main_Assembly_REV_B
HIFLY_Exploded_Assembly
HIFLY_Assembly_Drawing
```

Use the native CAD format and a neutral exchange format where appropriate.

For example:

```text
HIFLY_Main_Assembly.step
HIFLY_Main_Assembly.[native CAD format]
```

Only include formats actually generated by the project.

---

## 17. Assembly Evidence

The repository should eventually contain:

- Main assembly CAD file
- Neutral STEP/STP file where available
- Assembly screenshot
- Exploded-view screenshot
- Technical drawing where available
- Prototype photograph
- CAD-to-prototype comparison
- Revision history

Example repository structure:

```text
05_CAD/assembly/
├── README.md
├── cad/
├── drawings/
├── exploded/
├── screenshots/
└── prototype_comparison/
```

Create these directories when the corresponding evidence becomes available.

---

## 18. Thermal Simulation Interface

The final assembly may provide geometry for the thermal-analysis workflow.

```text
Main CAD Assembly
        ↓
Simplified Analysis Geometry
        ↓
Material Assignment
        ↓
Thermal Boundary Conditions
        ↓
Thermal Analysis
        ↓
Results
```

The simulation geometry may be simplified from the complete CAD model where small mechanical details do not affect the intended analysis.

---

## 19. Manufacturing Considerations

Before fabrication, review:

- Available manufacturing process
- Material availability
- Component tolerances
- Fastener availability
- Assembly sequence
- Cable routing
- Sensor access
- Inspection access
- Thermal-interface requirements

The selected manufacturing method should be recorded for fabricated components.

---

## 20. Assembly Inspection Checklist

### Mechanical

- [ ] Battery securely mounted
- [ ] PHP correctly positioned
- [ ] Heating element correctly positioned
- [ ] Insulation correctly installed
- [ ] Protection structure assembled
- [ ] Flexible silicone correctly applied
- [ ] Protective layer installed
- [ ] Mechanical supports secure

### Electrical

- [ ] Sensor wiring secure
- [ ] Heater wiring secure
- [ ] Power wiring secure
- [ ] Controller connections secure
- [ ] Antenna connection secure

### Clearance

- [ ] No unintended mechanical interference
- [ ] Cable routing clear
- [ ] Sensor locations accessible
- [ ] Enclosure can be closed
- [ ] Antenna has required clearance

---

## 21. Current Assembly Status

| Item | Status |
|---|---|
| Main Assembly Concept | Design |
| Battery Integration | Design / Prototype |
| PHP Integration | Design / Prototype |
| Heater Integration | Design / Prototype |
| Insulation Integration | Design / Prototype |
| Protection Structure | Design |
| Sensor Integration | Design / Prototype |
| Electronics Integration | Design / Prototype |
| Antenna Integration | Design |
| Full CAD Assembly | Design / Prototype |
| CAD-to-Prototype Comparison | Planned |
| Final Assembly Validation | Planned |

Update the status according to actual evidence.

---

## 22. Evidence Classification

Use:

- **Concept** — proposed arrangement
- **Design** — CAD model completed
- **Prototype** — physically assembled
- **Tested** — tested under documented conditions
- **Validated** — supported by defined evidence

A CAD assembly should not be described as physically validated unless the physical assembly and corresponding evidence exist.

---

## 23. Related Files

Main CAD documentation:

```text
05_CAD/README.md
```

Battery CAD:

```text
05_CAD/battery/README.md
```

PHP CAD:

```text
05_CAD/php/README.md
```

Thermal-management CAD:

```text
05_CAD/thermal_management/README.md
```

Thermal simulation:

```text
06_Simulation/
```

Testing:

```text
07_Testing/
```

Prototype media:

```text
10_Media/
```
