# REV A — Planned Bring-Up Procedure

This is the planned commissioning sequence for the first fabricated PCB. It documents the test order; it is not a record of tests already completed.

## Stage 1 — Visual inspection

- Inspect PCB orientation, polarity marks and component placement.
- Check the DRV8871 exposed-pad soldering and thermal-via area.
- Inspect the buck regulator, TVS, MOSFET, electrolytic/polymer capacitor and connectors.
- Confirm there are no visible bridges or damaged components.

## Stage 2 — Passive electrical checks

- Verify GND continuity across the board.
- Check for a short between 12 V input and GND.
- Check for a short between 12V_PROTECTED and GND.
- Check for a short between 5V_LOGIC and GND.
- Confirm motor outputs are not accidentally shorted to supply or GND.

## Stage 3 — Protected input rail

Use a bench supply with current limiting.

- Apply 12 V without the motor connected.
- Verify correct polarity behavior.
- Measure 12V_PROTECTED.
- Observe input current.
- Inspect Q2, TVS and fuse area for abnormal heating.

## Stage 4 — Buck converter

- Verify TPS54302 startup.
- Measure 5V_LOGIC.
- Check output stability under a light load.
- Inspect inductor and regulator temperature.
- Confirm the expected behavior of the enable/UVLO network.

## Stage 5 — MCU and encoder supply

- Install the ESP32-C6 only after power rails are confirmed.
- Verify the MCU receives the intended supply.
- Verify the 3.3 V encoder rail.
- Before using USB and external 5 V simultaneously, confirm the exact power-path behavior of the specific ESP32-C6 Super Mini revision.

## Stage 6 — Encoder interface

With the motor driver inactive:

- power the encoder;
- verify channels A and B;
- rotate the shaft manually;
- confirm clean state transitions;
- determine direction from channel phase relationship;
- establish the measurement method for counts/revolution.

## Stage 7 — Motor driver without mechanical load

- Verify DRV8871 idle state.
- Check IN1/IN2 control behavior.
- Test low-duty-cycle PWM first.
- Confirm OUT1/OUT2 polarity reversal.
- Monitor supply current and device temperature.

## Stage 8 — Motor test

- Connect the reference 12 V geared DC motor.
- Verify both directions.
- Sweep PWM gradually.
- Record current and speed.
- Compare measured encoder counts with the intended counting method.

## Stage 9 — Load and thermal checks

- Increase load in controlled steps.
- Observe DRV8871, input MOSFET and buck-regulator temperatures.
- Verify fuse behavior is compatible with normal transient motor current.
- Check 5 V rail behavior while the motor is switching.

## Stage 10 — Closed-loop development

Only after the hardware stages are validated:

- calculate RPM from the encoder;
- implement speed feedback;
- tune closed-loop speed control;
- evaluate position-control experiments if useful.

## Test record

Measured voltages, currents, temperatures, waveforms and pass/fail results should be added to the repository after the physical prototype is available.
