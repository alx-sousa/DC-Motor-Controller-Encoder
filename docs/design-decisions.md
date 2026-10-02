# REV A — Design Decisions

This document captures the main engineering decisions behind the first hardware revision.

## 1. Component-level architecture

The power-conversion and motor-control stages were designed around individual ICs and external components instead of using ready-made buck or H-bridge modules. This was intentional: the project was used to practice datasheet interpretation, component selection, power routing, decoupling, feedback design, thermal handling and manufacturability.

The ESP32-C6 Super Mini remains a removable controller module so firmware development and replacement can be performed without permanently soldering the MCU board to the carrier.

## 2. 12 V input protection

The input path is organized as:

```text
VCC12 → 3 A slow-blow fuse → P-channel reverse-polarity MOSFET → 12V_PROTECTED
```

A unidirectional SMBJ15A TVS is connected from the protected rail to GND.

The intent is to protect the prototype from common bench-level wiring and transient conditions around a regulated 12 V source. REV A is not documented as an automotive transient-qualified design.

## 3. Motor driver selection

A Texas Instruments **DRV8871DDAR** was selected for the motor stage.

Reasons for the architecture:

- integrated H-bridge for bidirectional motor control;
- external ILIM programming;
- exposed PowerPAD package suitable for thermal spreading;
- direct MCU control through two logic inputs.

The motor outputs remain floating with respect to supply/GND because the bridge reverses their polarity.

## 4. DRV8871 thermal strategy

The PowerPAD is tied to GND and uses four GND vias in a 2 × 2 arrangement to connect the exposed pad to the lower GND plane and improve heat transfer.

These are via-in-pad features. For a later production-oriented revision, filled/capped/tented via strategies may be considered depending on the assembly process.

## 5. Buck regulator

The **TPS54302DDCR** generates 5V_LOGIC from 12V_PROTECTED.

REV A implements the regulator and supporting network directly:

- input bypass and bulk capacitance;
- bootstrap capacitor;
- shielded inductor;
- output capacitors;
- feedback network;
- enable/UVLO divider.

The 5 V connector should be treated as access to the board logic rail, not as a generic independent 3 A bench supply.

## 6. Encoder interface

The incremental Hall encoder is powered from 3.3 V to maintain direct logic compatibility with the ESP32.

Each A/B channel uses a series resistor and pull-up. Pull-ups were retained because the encoder output topology was not completely explicit in the available documentation and the arrangement remains compatible with an open-collector/open-drain style output.

## 7. PCB zoning

The PCB is divided into four functional regions:

1. 12V POWER
2. BUCK CONVERTER
3. DRIVER MOTOR
4. MCU ENCODER

This keeps input protection and switching power compact, places the driver near the motor connector, and separates the encoder/logic area from higher-current switching paths.

## 8. Routing strategy

Signal and low-current control lines use narrower tracks than the main motor/input paths. Wider routes are used for 12 V input and motor current, with short local neck-down only where package pads physically require it.

The motor outputs remain on the Top layer to avoid unnecessary current-path transitions through vias.

## 9. Grounding

Solid GND copper regions are used on both PCB layers. Stitching vias connect the planes throughout the board, providing short return paths and a more continuous ground structure.

## 10. MCU serviceability

The ESP32-C6 Super Mini mounts through two removable female headers. USB-C is oriented toward the board edge for easier programming/debugging.

Before fabrication, the physical module dimensions, header spacing, antenna location and copper keepout should be checked against the exact Super Mini board revision.

## 11. REV A philosophy

REV A prioritizes:

- clear electrical architecture;
- manufacturability;
- accessible connectors;
- serviceability;
- structured bring-up;
- measurement before optimization.

Miniaturization is intentionally secondary. REV B changes will be driven by physical measurements rather than assumptions.
