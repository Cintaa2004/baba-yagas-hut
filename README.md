# baba-yagas-hut
Phase Lab simulation for ECSE 351

# Baba Yaga’s Hut – Phase Lab Simulation Design

## Project Overview

This project is a conceptual and design-based simulation of the Phase Lab from EECS 351, implemented through GNU Radio Companion (GRC). Due to software limitations, I was unable to run GNU Radio directly on my machine, but I have designed the flowgraph and explained its behavior and logic in detail.

The goal of this assignment is to create a graphical interface in which a user can manipulate the amplitude, frequency, and phase of a signal in real time and observe its effect in both time and phasor (XY) domains.

---

## Flowgraph Description

### 🔷 1. GUI Inputs (Controls)

Three QT GUI Range blocks are used to give the user interactive sliders:

| Parameter   | Range       | Default | Units   |
|-------------|-------------|---------|---------|
| Amplitude   | 0.0 – 5.0   | 1.0     | unitless |
| Frequency   | 100 – 5000  | 1000    | Hz      |
| Phase       | 0 – 6.28    | 0.0     | radians |

These variables are called `amplitude`, `frequency`, and `phase` respectively.

---

### 🔷 2. Signal Generation

A **Signal Source block** is used to generate a cosine wave, configured like this:

- **Waveform**: Cosine  
- **Output type**: Complex  
- **Frequency**: `frequency`  
- **Amplitude**: `amplitude`  
- **Phase**: `phase`  
- **Sample rate**: `samp_rate = 32000`

This produces a signal of the form:  
**x(t) = A·cos(2πft + φ)**  
which represents a rotating phasor in the complex plane.

---

### 🔷 3. Throttle Block

A **Throttle block** is inserted between the Signal Source and the visualization sinks to ensure the flowgraph runs at real-time speed, avoiding CPU overload during simulation.

- Sample rate: `samp_rate`
- Type: Complex

---

### 🔷 4. Output Visualization

Two sinks are used:

#### 🟦 QT GUI Time Sink
- Displays the real/imaginary parts over time.
- Helps observe how the waveform changes with each slider.

#### 🟩 QT GUI Vector Sink (or XY Plot)
- Plots the signal in the complex plane (phasor representation).
- You can see the vector rotate as frequency or phase changes, and stretch/shrink as amplitude changes.

---

## Expected Behavior

- **Increasing frequency**: The phasor rotates faster (tighter spirals in XY view), and waveform oscillates faster in time view.
- **Increasing amplitude**: The phasor’s radius increases (spiral becomes wider), and waveform height increases in time view.
- **Increasing phase**: Rotates the starting point of the phasor around the origin, shifting the time signal horizontally.

---

## Conceptual Diagram (Text-based)

```text
[ QT GUI Range: amplitude ]
[ QT GUI Range: frequency ] ──→ [ Signal Source ] ──→ [ Throttle ] ──┬──→ [ QT Time Sink ]
[ QT GUI Range: phase     ]                                       │
                                                                   └──→ [ QT Vector Sink ]
