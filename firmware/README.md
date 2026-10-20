# Firmware Architecture

Firmware implementation for REV A is still pending. The current project milestone is the hardware design, so this document describes the planned firmware architecture only.

The planned firmware responsibilities are:

```mermaid
flowchart TD
    A[Initialize MCU and peripherals] --> B[Configure motor-control outputs]
    B --> C[Configure encoder A/B inputs]
    C --> D[Read incremental encoder]
    D --> E[Estimate direction and speed]
    E --> F[Apply motor command / PWM]
    F --> D
```

Future firmware work may include:

- PWM generation;
- direction control;
- quadrature A/B acquisition;
- RPM calculation;
- speed control;
- closed-loop tuning;
- optional position-control experiments.

No closed-loop performance is claimed until it is implemented and tested with physical hardware.
