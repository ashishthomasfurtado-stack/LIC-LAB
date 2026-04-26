# Lab Report: Precision Half-Wave Rectifiers Using Op-Amp (µA741)

**Lab:** 8 | **Date:** 26 April 2026
**Subject:** Linear Integrated Circuits — LTspice Simulation Lab

---

## Aim

To simulate and analyse the operation of:
1. An **Inverting Precision Half-Wave Rectifier** using the µA741 op-amp.
2. A **Non-Inverting Precision Half-Wave Rectifier** using the µA741 op-amp.

And to verify that the op-amp feedback loop eliminates the 0.7 V forward-voltage drop of the diode, enabling precision rectification of small-amplitude signals.

---

## Components Used

| Component | Value / Part Number | Quantity |
|---|---|---|
| Op-Amp | µA741 | 1 per circuit |
| Diode | 1N4148 | 2 (Circuit 1), 1 (Circuit 2) |
| Resistor R1 | 10 kΩ (Ckt 1), 1 kΩ (Ckt 2) | 1 per circuit |
| Resistor R2 | 20 kΩ (Ckt 1), 1 kΩ (Ckt 2) | 1 per circuit |
| Load Resistor R3 | 10 kΩ (Ckt 1), 1 kΩ (Ckt 2) | 1 per circuit |
| Input Supply V1 | SINE(0 0.8 1000) — 0.8 V peak, 1 kHz | 1 per circuit |
| Dual Power Supply | ±12 V | 1 per circuit |

---

## Theory

### Why Precision Rectification is Needed

A standard diode rectifier introduces a **knee voltage** drop of approximately 0.7 V. For small signals (e.g., 0.8 V peak), this drop significantly distorts or even eliminates the rectified output. The precision rectifier solves this by placing the diode **inside the feedback loop** of an op-amp. The op-amp's high open-loop gain compensates internally for the diode drop, presenting a near-ideal rectifier characteristic to the external circuit.

### Virtual Ground Principle

In both circuits, the inverting terminal of the op-amp is held at **Virtual Ground (0 V)** by negative feedback whenever the feedback path is closed. This is the key mechanism that allows the circuit to track the input precisely without the diode drop appearing at the output.

---

## Circuit 1: Inverting Precision Half-Wave Rectifier

### Circuit Diagram

<img width="2998" height="1605" alt="Screenshot 2026-04-26 154522" src="https://github.com/user-attachments/assets/87a8b624-405e-4acb-bebb-a0404227c227" />


**Key Parameters:**
- Input: SINE(0 0.8 1000) — 0.8 V peak, 1 kHz sinusoid
- R1 = 10 kΩ, R2 = 20 kΩ → Voltage Gain: **Av = −R2/R1 = −2**
- D1, D2: 1N4148 (catcher diode and output diode respectively)
- Load R3 = 10 kΩ
- Supply: ±12 V

### Stage-by-Stage Operation

#### Positive Half-Cycle (Vin > 0)

When the input goes positive, current is pushed into the inverting terminal (node 2). To maintain virtual ground, the op-amp output swings **negative**.

- **D1** (catcher diode): Forward-biased — **ON**. The feedback loop closes through D1, keeping the op-amp active and preventing saturation.
- **D2** (output diode): Reverse-biased — **OFF**. No current flows through R2 or to the output.
- **Vout = 0 V**

#### Negative Half-Cycle (Vin < 0)

When the input goes negative, the op-amp output swings **positive** to maintain virtual ground.

- **D1**: Reverse-biased — **OFF**.
- **D2**: Forward-biased — **ON**. The circuit now behaves as a standard inverting amplifier.
- The gain is: **Av = −R2/R1 = −20k/10k = −2**
- A negative input multiplied by −2 produces a **positive output**.
- **Vout = positive half-sine wave with amplitude = 2 × 0.8 = 1.6 V peak**

### Role of the Catcher Diode (D1)

Without D1, during the positive half-cycle the feedback loop would be completely open, driving the op-amp output to the negative rail (−12 V saturation). When the signal crosses zero again, the op-amp would need significant recovery time, causing severe distortion (glitches) at zero-crossing points. D1 keeps the op-amp in the **active (linear) region at all times**, dramatically improving switching speed and output accuracy.

### Simulation Waveform Analysis

<img width="2999" height="1610" alt="Screenshot 2026-04-26 154425" src="https://github.com/user-attachments/assets/dbc8c8f7-51db-444a-839c-6447d9d0ab35" />

| Parameter | Observed Value |
|---|---|
| Input signal (V(n004) — blue) | 0.8 V peak, 1 kHz |
| Output signal (V(vout) — green) | ~1.6 V peak |
| Output during positive half-cycle | ~0 V (blocked) |
| Output during negative half-cycle | ~1.6 V positive half-sine |
| Measured Gain (1.6013 V / 0.8 V) | **≈ 2.0** ✓ |
| Cursor 1 (Vout peak) | 2.7513 ms, **1.6013 V** |
| Cursor 2 (Vout trough/zero) | 2.4991 ms, **−568 µV ≈ 0 V** |

**Observation:** The output is a clean positive half-sine wave at exactly twice the input amplitude, confirming the gain of 2. The near-zero output during the positive half-cycle (−568 µV) demonstrates that the diode drop is effectively eliminated by the op-amp feedback — a hallmark of precision rectification.

### Effect of Reversing Both Diodes

| Condition | D1 State (Pos. cycle) | D2 State (Pos. cycle) | Vout |
|---|---|---|---|
| Standard direction | ON | OFF | 0 V |
| Standard direction | OFF | ON | +1.6 V (neg. input → pos. output) |
| **Reversed direction** | **OFF** | **ON** | **0 V (neg. input blocked)** |
| **Reversed direction** | **ON** | **OFF** | **−1.6 V (neg. half-sine)** |

Reversing both diodes causes the circuit to pass the **positive half-cycle** and invert it, producing a **negative half-sine wave** at the output.

---

## Circuit 2: Non-Inverting Precision Half-Wave Rectifier

### Circuit Diagram

<img width="2999" height="1667" alt="Screenshot 2026-04-26 154602" src="https://github.com/user-attachments/assets/9d41b482-c26f-47c3-89c2-3649cc1ea158" />


**Key Parameters:**
- Input: SINE(0 0.8 1000) — 0.8 V peak, 1 kHz sinusoid
- R1 = R2 = 1 kΩ → Voltage Gain: **Av = +1 (unity gain)**
- D1: 1N4148 (single diode in output path)
- Load R1 = 1 kΩ
- Supply: ±12 V

### Stage-by-Stage Operation

#### Positive Half-Cycle (Vin > 0)

The input is applied to the **non-inverting (+) terminal**. The op-amp output goes positive, forward-biasing D1.

- **D1**: Forward-biased — **ON**.
- Feedback is closed through R2 (1 kΩ). The circuit acts as a **voltage follower** (unity gain buffer).
- The diode drop is internally compensated by the op-amp.
- **Vout follows Vin exactly: +0.8 V peak**

#### Negative Half-Cycle (Vin < 0)

The op-amp output would swing negative, but D1 becomes reverse-biased — breaking the feedback loop.

- **D1**: Reverse-biased — **OFF**. No feedback path exists.
- The output node is only connected to the load resistor R1, which pulls it to ground.
- The op-amp **saturates** at the negative rail (−12 V) internally, but this does not appear at Vout.
- **Vout = 0 V**

> **Note on Saturation:** Unlike the inverting circuit (which uses a catcher diode to prevent saturation), this simpler single-diode circuit **does allow the op-amp to saturate** during the negative half-cycle. This is its key limitation — the recovery from saturation at each zero-crossing introduces brief distortion glitches, which become more prominent at higher frequencies.

### Simulation Waveform Analysis
<img width="2996" height="1598" alt="Screenshot 2026-04-26 154705" src="https://github.com/user-attachments/assets/9d730c56-f699-4d87-9d46-684cf63baac9" />




| Parameter | Observed Value |
|---|---|
| Input signal (V(n004) — blue) | 0.8 V peak, 1 kHz |
| Output signal (V(vout) — green) | ~0.8 V peak |
| Output during positive half-cycle | ~0.8 V (faithful reproduction) |
| Output during negative half-cycle | ~0 V (blocked) |
| Cursor 1 (Vout peak) | 2.2591 ms, **798.76 mV ≈ 0.8 V** |
| Cursor 2 (Vout at zero-crossing) | 2.4991 ms, **8.59 mV ≈ 0 V** |
| Measured Gain (0.799 V / 0.8 V) | **≈ 1.0** ✓ |

**Observation:** The output faithfully reproduces the positive half-cycles of the input at unity gain (0.8 V peak), with the negative half-cycles cleanly clamped to 0 V. The near-zero output during the blocked half-cycle (8.59 mV) again confirms that the op-amp feedback eliminates the diode forward voltage drop.

### Effect of Reversing the Diode

| Condition | Diode State (Pos. cycle) | Diode State (Neg. cycle) | Vout |
|---|---|---|---|
| Standard (Anode → op-amp out) | ON | OFF | +0.8 V half-sine |
| **Reversed (Cathode → op-amp out)** | **OFF** | **ON** | **−0.8 V half-sine (negative half-cycles passed)** |

Reversing the single diode causes the circuit to pass only the **negative half-cycles** of the input, producing a negative half-sine at the output.

---

## Comparative Analysis: Circuit 1 vs Circuit 2

| Parameter | Inverting Rectifier (Lab 8a) | Non-Inverting Rectifier (Lab 8b) |
|---|---|---|
| No. of Diodes | 2 (D1 catcher + D2 output) | 1 |
| Gain | −R2/R1 = −2 (configurable) | +1 (unity, non-inverting) |
| Output Half-Cycle Passed | Negative input → Positive output | Positive input → Positive output |
| Peak Output Voltage | **1.6 V** (2 × 0.8 V) | **0.8 V** (same as input) |
| Op-Amp Saturation | **Prevented** (D1 keeps op-amp linear) | **Occurs** during blocked half-cycle |
| Zero-Crossing Distortion | Minimal (D1 suppresses glitches) | More prominent (saturation recovery) |
| Circuit Complexity | Higher | Lower / Simpler |
| Accuracy | Higher | Moderate |

---

## Zero-Crossing Spikes: Cause and Mitigation

Both circuits exhibit minor glitches at zero-crossing points. These arise from three factors:

1. **Op-Amp Slew Rate Limitation:** The µA741 has a slew rate of only ~0.5 V/µs. At the moment of switching, the op-amp output must jump ~1.4 V (from −0.7 V to +0.7 V) to switch diode states. It cannot do this instantaneously, causing a brief "dead zone."

2. **Diode Junction Capacitance:** The 1N4148 stores a small charge during conduction. When it turns OFF, this charge is suddenly released as a transient voltage spike before the op-amp can restore control.

3. **Frequency Dependence:** At higher input frequencies, the op-amp has less time per cycle to complete this transition, making spikes relatively wider and more distorted.

**Mitigation Strategies:**
- Replace µA741 with a high-speed op-amp (e.g., LF351, TL081) to improve slew rate.
- Reduce input frequency (e.g., 100 Hz) to give the op-amp more recovery time per cycle.
- Add a small capacitor (10–100 pF) in parallel with the feedback resistor to filter high-frequency switching transients.

---

## Summary Table: All Configurations

| Circuit | Diode Direction | Half-Cycle Passed | Output Polarity | Peak Vout |
|---|---|---|---|---|
| Inverting (8a) | Standard | Negative input | **Positive** | 1.6 V |
| Inverting (8a) | Reversed | Positive input | **Negative** | −1.6 V |
| Non-Inverting (8b) | Standard | Positive input | **Positive** | 0.8 V |
| Non-Inverting (8b) | Reversed | Negative input | **Negative** | −0.8 V |

---

## Conclusion

Both precision rectifier circuits successfully demonstrated the fundamental advantage of op-amp-based rectification: **elimination of the diode's 0.7 V forward voltage drop** through negative feedback compensation.

- The **Inverting Precision Rectifier** (Lab 8a) produced a clean positive half-sine output at a measured peak of **1.6013 V**, confirming a gain magnitude of 2.0 (R2/R1 = 20k/10k). The catcher diode D1 kept the op-amp in its linear region throughout, minimising zero-crossing distortion.

- The **Non-Inverting Precision Rectifier** (Lab 8b) produced a clean positive half-sine output at **798.76 mV ≈ 0.8 V**, confirming unity gain operation. The simpler single-diode configuration is adequate for moderate-frequency signals but is inherently more susceptible to saturation-induced glitches.

Both simulations validate the theoretical predictions and confirm the suitability of these circuits for precision signal processing, instrumentation, and signal conditioning applications where diode drop errors cannot be tolerated.

---

*Simulated using LTspice XVII | Op-Amp Model: µA741 (lm741.lib) | Input: 0.8 V peak, 1 kHz*
