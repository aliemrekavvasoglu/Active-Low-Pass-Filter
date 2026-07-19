# Active-Low-Pass-Filter
First-order active low-pass filter — design and LTspice simulation

# Active Low-Pass Filter

First-order active low-pass filter — design and LTspice simulation.

## Overview

This project is a first-order active low-pass filter designed and simulated in LTspice. The circuit consists of two parts that perform two different tasks.

The first part is the filter stage. It is arranged like a voltage divider, but with a capacitor in place of one resistor. Since the capacitor's impedance depends on frequency, this stage determines which frequencies pass. Its output is connected to the non-inverting (+) input of the op-amp.

The second part is the amplifier stage. It consists of two resistors whose ratio sets the gain — the gain depends only on the ratio of these resistors, not on their absolute values.

## Design Specifications

| Parameter | Value |
|---|---|
| Filter type | First-order active low-pass |
| Topology | Non-inverting op-amp |
| Cutoff frequency | 1 kHz |
| Passband gain | 2 (6 dB) |
| Op-amp | UniversalOpamp2 |
| Supply | ±15 V |
| Roll-off | −20 dB/decade |

## Circuit

![Circuit schematic](Schematic.jpeg)

| Component | Value | Function |
|---|---|---|
| R | 16 kΩ | Sets cutoff frequency with C |
| C | 10 nF | Sets cutoff frequency with R |
| R1 | 10 kΩ | Gain-setting resistor (to ground) |
| Rf | 10 kΩ | Feedback resistor (output to inverting input) |

## Design Calculations

**Cutoff frequency**

The cutoff frequency is the point where the capacitor's impedance equals the resistance:

1 / (2π · fc · C) = R  →  fc = 1 / (2π · R · C)

Choosing C = 10 nF for a target of fc = 1 kHz:

R = 1 / (2π × 1000 × 10n) = 15.9 kΩ → 16 kΩ (nearest standard value)

**Passband gain**

In the non-inverting configuration, negative feedback forces V+ = V− (virtual short). The R1–Rf divider gives V− = Vout × R1 / (R1 + Rf), and since V− = Vin:

A = Vout / Vin = 1 + Rf / R1 = 1 + (10k / 10k) = 2  →  6 dB

## Simulation Results

### AC analysis (Bode plot)

Simulation command: `.ac dec 100 10 1Meg`

![Bode plot](Bode_Plot.jpeg)

- Passband gain is flat at 6 dB from 10 Hz to approximately 1 kHz, matching the designed gain of 2.
- The response begins to roll off at approximately 1 kHz, as designed.
- Roll-off is −20 dB/decade, reaching about −54 dB at 1 MHz.
- Phase shifts from 0° to −90°, passing through −45° at the cutoff frequency.

### Transient analysis — 500 Hz (passband)

Simulation command: `.tran 10m` — input: 1 V sine at 500 Hz

![Transient 500 Hz](Transient_Analysis_1.jpeg)

Output amplitude is approximately 1.8 V. This is slightly below the ideal 2 V because 500 Hz is close enough to the cutoff frequency that mild attenuation has already begun.

### Transient analysis — 10 kHz (stopband)

Simulation command: `.tran 0 1m 0.5m` — input: 1 V sine at 10 kHz

![Transient 10 kHz](Transient_Analysis_2_10k_frequency.png)

Output amplitude is approximately 200 mV, about 9× attenuation compared to the passband. The first 0.5 ms is excluded from the plot to skip the initial transient and show only the steady-state response.

The AC and transient results are consistent with each other and with the hand calculations.

## Files

| File | Description |
|---|---|
| `Schematic.jpeg` | Circuit diagram |
| `Bode_Plot.jpeg` | AC analysis result |
| `Transient_Analysis_1.jpeg` | Transient response in the passband (500 Hz) |
| `Transient_Analysis_2_10k_frequency.png` | Transient response in the stopband (10 kHz) |

## Tools

LTspice XVII

## Possible Improvements

- Second-order Sallen-Key implementation for a sharper −40 dB/decade roll-off
- Comparison with a passive RC filter to show the effect of the op-amp buffer under load
