# HIFLY — Pulsating Heat Pipe (PHP) CAD

## 1. Purpose

This directory documents the Computer-Aided Design (CAD) representation and physical integration of the Pulsating Heat Pipe (PHP) used in the HIFLY thermal-management architecture.

The PHP is intended to provide a passive thermal-transfer path within the thermal-management system.

This documentation covers:

- PHP geometry
- Routing
- Battery interface
- Evaporator region
- Transport section
- Condenser region
- Mechanical support
- Thermal interface
- Assembly integration
- CAD evidence
- Prototype comparison

---

## 2. PHP Role in HIFLY

The conceptual thermal-management path is:

```text
Battery / Heat Source
        ↓
   Evaporator Region
        ↓
   PHP Transport Path
        ↓
   Condenser Region
        ↓
 Heat Rejection / Transfer
```

The PHP is integrated with the surrounding thermal-management architecture rather than being treated as an isolated component.

---

## 3. PHP CAD Objectives

The CAD model should support:

1. Definition of PHP geometry
2. Identification of thermal interfaces
3. Mechanical integration with the battery system
4. Definition of routing
5. Support and mounting
6. Clearance verification
7. Fabrication planning
8. Thermal-analysis preparation
9. CAD-to-prototype comparison

---

## 4. PHP Assembly Concept

```text
             PHP ASSEMBLY
                   │
        ┌──────────┴──────────┐
        │                     │
        ↓                     ↓
 Evaporator Region       Condenser Region
        │                     │
        └──────────┬──────────┘
                   │
             Transport Path
                   │
                   ↓
             Thermal System
```

The actual geometry should follow the finalized CAD model.

---

## 5. PHP Main Components

The CAD documentation should identify the following regions where applicable:

```text
PHP
│
├── Evaporator Region
│
├── Transport Section
│
├── Condenser Region
│
├── Bends / Turns
│
└── Mechanical Supports
```

The exact configuration depends on the fabricated PHP design.

---

## 6. Evaporator Region

The evaporator region is the portion of the PHP associated with the heat-input side.

For HIFLY, it is positioned in relation to the battery or intended thermal source.

Conceptually:

```text
Battery
   │
   ↓
Thermal Interface
   │
   ↓
PHP Evaporator
   │
   ↓
PHP Transport Path
```

The CAD model should document:

- Location
- Contact area
- Geometry
- Mounting
- Clearance
- Thermal interface

Actual dimensions should be taken from the finalized CAD model.

---

## 7. Transport Section

The transport section provides the physical path between the thermal regions.

The CAD model should document:

- Tube/path routing
- Bends
- Length
- Supports
- Clearance
- Interface with surrounding structure

Avoid unnecessary sharp bends where the physical PHP design does not permit them.

---

## 8. Condenser Region

The condenser region is associated with the heat-rejection or heat-transfer side of the PHP.

Conceptually:

```text
Evaporator
    │
    ↓
Transport Section
    │
    ↓
Condenser
    │
    ↓
Heat Transfer / Rejection
```

The CAD documentation should show its location and relationship to the surrounding structure.

---

## 9. PHP Routing

PHP routing should be designed to fit within the available mechanical envelope.

Consider:

- Battery dimensions
- Insulation
- Protection chamber
- Mechanical supports
- Wiring
- Sensors
- Antenna/electronics
- Fabrication constraints

Example:

```text
       ┌───────────────┐
       │   Battery     │
       │               │
       └───────┬───────┘
               │
        PHP Evaporator
               │
        ╭──────┴──────╮
        │             │
        │ PHP Path    │
        │             │
        ╰──────┬──────╯
               │
          Condenser
```

This diagram is conceptual and is not a manufacturing drawing.

---

## 10. PHP Dimensions

Do not enter assumed dimensions.

Document the actual CAD dimensions when available.

| Parameter | Value |
|---|---|
| PHP overall length | TBD |
| PHP overall width | TBD |
| PHP overall height | TBD |
| Tube/channel diameter | TBD |
| Number of turns | TBD |
| Evaporator length | TBD |
| Condenser length | TBD |
| Transport length | TBD |
| Bend radius | TBD |
| Total mass | TBD |

---

## 11. PHP Material

The material should be documented based on the actual fabricated PHP.

| Parameter | Value |
|---|---|
| Tube/channel material | TBD |
| Material grade | TBD |
| Surface treatment | TBD |
| Joining method | TBD |
| Working-fluid specification | TBD |

The working-fluid information should only be documented when confirmed from the actual PHP design.

---

## 12. Thermal Interface

The PHP must have a defined physical interface with the component from which it receives or transfers heat.

The CAD model should identify:

- Contact surface
- Interface material
- Contact geometry
- Mechanical retention
- Relative orientation

Conceptually:

```text
Heat Source
    ↓
Thermal Interface
    ↓
PHP
```

Thermal performance must be established through analysis and/or testing rather than inferred from CAD geometry alone.

---

## 13. Mechanical Support

The PHP should be mechanically supported where required.

Possible supports include:

- Brackets
- Clamps
- Mounting plates
- Printed supports
- Silicone interfaces
- Enclosure supports

The selected support method should be documented in the CAD and hardware sections.

---

## 14. PHP and Battery Clearance

The assembly should maintain suitable clearance between the PHP and battery.

Check:

```text
PHP ↔ Battery
PHP ↔ Heater
PHP ↔ Insulation
PHP ↔ Wiring
PHP ↔ Protection Structure
PHP ↔ Sensors
PHP ↔ Mechanical Supports
```

Any CAD interference should be recorded and corrected before fabrication where practical.

---

## 15. PHP and Heater Integration

Where the heating element is used for thermal testing or battery thermal management, the CAD model should show its relationship to the PHP.

Conceptually:

```text
Heating Element
       ↓
Thermal Input
       ↓
PHP Evaporator
       ↓
Transport Section
       ↓
Condenser
```

The exact arrangement should match the physical prototype.

---

## 16. PHP and Insulation

Thermal insulation may be positioned around selected regions of the PHP and battery thermal-management system.

The CAD model should distinguish between:

- PHP geometry
- Thermal interface
- Insulation
- External environment

Example:

```text
┌─────────────────────────────┐
│       Insulation            │
│                             │
│    ┌───────────────────┐    │
│    │       PHP         │    │
│    └───────────────────┘    │
│                             │
└─────────────────────────────┘
```

The actual coverage must follow the finalized design.

---

## 17. PHP Protection

The PHP should be protected against unintended mechanical damage during handling and operation.

Potential protection may include:

- Mechanical supports
- Enclosure
- Flexible silicone
- Protective outer layer
- Insulation

The selected protection method should be documented with actual evidence.

---

## 18. CAD Assembly Relationship

The PHP should be referenced from the main assembly.

```text
05_CAD/
│
├── assembly/
│   └── Main Assembly
│
├── battery/
│   └── Battery CAD
│
└── php/
    └── PHP CAD
```

The PHP model should use a consistent coordinate system and origin/reference scheme where practical.

---

## 19. PHP CAD File Organization

Recommended directory:

```text
05_CAD/php/
│
├── README.md
├── cad/
├── drawings/
├── screenshots/
└── prototype_comparison/
```

Add actual files only when they are available.

---

## 20. Recommended PHP CAD Files

Possible files include:

```text
HIFLY_PHP
HIFLY_PHP_Assembly
HIFLY_PHP_Evaporator
HIFLY_PHP_Condenser
HIFLY_PHP_Mount
HIFLY_PHP_Drawing
```

Use the native CAD format and neutral exchange format where appropriate.

---

## 21. CAD Screenshots

Recommended screenshots include:

### Complete PHP

Shows the entire PHP geometry.

### Battery Interface

Shows the relationship between PHP and battery.

### Evaporator

Shows the thermal-input region.

### Condenser

Shows the heat-transfer/rejection region.

### Assembly

Shows the PHP installed in the HIFLY system.

### Section View

Shows the PHP relationship with insulation and surrounding structure.

Only use screenshots from the actual CAD model.

---

## 22. CAD-to-Prototype Comparison

Where the physical PHP is available, compare:

```text
CAD Model
    ↓
Fabricated PHP
    ↓
Physical Assembly
```

Recommended comparison table:

| Feature | CAD | Prototype | Difference |
|---|---|---|---|
| Overall geometry | Defined | TBD | TBD |
| Evaporator location | Defined | TBD | TBD |
| Condenser location | Defined | TBD | TBD |
| Routing | Defined | TBD | TBD |
| Supports | Defined | TBD | TBD |
| Battery interface | Defined | TBD | TBD |

Do not mark a feature as matching until it has been physically inspected.

---

## 23. PHP Thermal Analysis

The PHP CAD geometry can be used as part of thermal analysis.

Possible workflow:

```text
PHP CAD
   ↓
Thermal Assembly
   ↓
Material Assignment
   ↓
Boundary Conditions
   ↓
Thermal Analysis
   ↓
Temperature Distribution
   ↓
Heat Flux
```

A conventional steady-state thermal analysis should be described as an evaluation of the PHP-assisted thermal architecture.

It should not be presented as a direct simulation of internal PHP pulsating two-phase flow unless an appropriate multiphase simulation has actually been performed.

---

## 24. Simulation Geometry Simplification

The complete CAD model may contain small mechanical details that are unnecessary for thermal analysis.

Possible simplifications include:

- Removing small fasteners
- Removing cosmetic geometry
- Simplifying supports
- Simplifying cable geometry
- Simplifying enclosure details

Any simplification should preserve the geometry relevant to the intended analysis.

---

## 25. PHP Manufacturing Documentation

If the PHP is fabricated specifically for HIFLY, record:

- Material
- Dimensions
- Joining method
- Bending/forming method
- Working-fluid information where applicable
- Sealing method
- Inspection procedure

Do not add unverified manufacturing details.

---

## 26. PHP Inspection

Before integration, inspect:

- Geometry
- Bends
- Joints
- Seals
- Mounting points
- Surface condition
- Interfaces
- Physical damage

Record inspection results in the testing section.

---

## 27. PHP Safety Considerations

The PHP design should consider:

- Mechanical integrity
- Sealing
- Working-fluid compatibility
- Temperature exposure
- Pressure-related considerations where applicable
- Protection from external damage
- Compatibility with surrounding materials

Specific safety limits should be based on the actual PHP design and verified documentation.

---

## 28. PHP Test Connection

The PHP CAD documentation should connect to experimental testing.

Suggested workflow:

```text
PHP CAD
   ↓
Fabricated PHP
   ↓
Integrated Prototype
   ↓
Thermal Test
   ↓
Temperature Measurements
   ↓
Data Analysis
```

Actual performance claims should be supported by recorded measurements.

---

## 29. Evidence Required

Recommended PHP evidence includes:

- CAD model
- CAD screenshots
- Technical drawing
- Physical PHP photograph
- Integrated assembly photograph
- Thermal test setup
- Temperature data
- Simulation results where available
- Test report

Do not label planned evidence as completed evidence.

---

## 30. Current Status

| Item | Status |
|---|---|
| PHP Concept | Design |
| PHP CAD Geometry | Design / Prototype |
| Evaporator Region | Design |
| Transport Section | Design |
| Condenser Region | Design |
| Battery Integration | Design / Prototype |
| Mechanical Support | Design |
| Insulation Integration | Design |
| CAD-to-Prototype Comparison | Planned |
| Thermal Simulation | Planned / In Progress |
| Thermal Testing | Planned / In Progress |
| PHP Performance Validation | Planned |

Update these statuses using actual project evidence.

---

## 31. Evidence Classification

Use:

- **Concept** — proposed PHP arrangement
- **Design** — CAD model developed
- **Prototype** — physical PHP fabricated/integrated
- **Tested** — tested under documented conditions
- **Validated** — supported by defined experimental or analytical evidence

A PHP CAD model alone does not demonstrate thermal performance.

---

## 32. Related Files

Main CAD documentation:

```text
05_CAD/README.md
```

Main assembly:

```text
05_CAD/assembly/README.md
```

Battery CAD:

```text
05_CAD/battery/README.md
```

Thermal-management CAD:

```text
05_CAD/thermal_management/README.md
```

Thermal control:

```text
04_Software/control_logic/thermal_control.md
```

Simulation:

```text
06_Simulation/
```

Testing:

```text
07_Testing/
```

Data:

```text
08_Data/
```
