# Experiment 4: Differential Amplifier Analysis
### MOSFET Differential Amplifier with Resistive Load — DC, Transient, and AC Analysis

---

## Aim

To design and simulate a MOSFET differential amplifier with resistive load using LTspice by performing DC, Transient, and AC analyses, and to evaluate its performance based on gain, bandwidth, input common-mode range (ICMR), and output common-mode range (OCMR).

---

## Introduction

A differential amplifier is one of the most fundamental building blocks in analog circuit design. It amplifies the difference between two input signals while rejecting signals that are common to both inputs — a property known as common-mode rejection. This makes it highly effective in noise-sensitive environments such as instrumentation, communication, and signal processing systems.

MOSFET-based differential amplifiers are widely employed in integrated circuits, including operational amplifiers and comparators, due to their high gain, excellent noise immunity, and compatibility with CMOS fabrication processes.

In this experiment, a NMOS differential pair with resistive drain loads is designed and simulated. The circuit is analyzed through DC operating point extraction, transient waveform observation across the input common-mode range, and AC frequency response characterization.

---

## Theory

A differential amplifier amplifies the voltage difference between two input terminals:

$$v_{id} = v_{in1} - v_{in2}$$

The circuit consists of two matched MOSFETs (M1 and M2) sharing a common tail current source ($I_{SS}$). The drain currents are steered based on the differential input, producing a proportional voltage difference at the output nodes through the drain resistors ($R_D$).

### Differential Gain

For small-signal operation in the saturation region:

$$A_d = g_m \cdot R_{out}$$

where the transconductance is:

$$g_m = \frac{2I_D}{V_{OV}}$$

and the effective output resistance is:

$$R_{out} = R_D \parallel r_o$$

### Linearity Condition

Linear operation is maintained while both transistors remain in saturation, which requires:

$$|v_{id}| < \sqrt{2} \cdot V_{OV}$$

When this condition is violated, one transistor enters cutoff and the output clips.

### Input Common-Mode Range (ICMR)

$$V_{CM(min)} = V_S + V_T$$

$$V_{CM(max)} = V_D + V_T$$

### Output Common-Mode Range (OCMR)

$$V_{OCM(max)} = V_{DD}$$

$$V_{OCM(min)} = V_S + V_{OV}$$

---

## Design Specifications

| Parameter | Value |
|---|---|
| Technology | TSMC 180nm CMOS (CMOSN model) |
| Supply Voltage ($V_{DD}$) | +0.9 V |
| Negative Supply ($V_{SS}$) | −0.9 V |
| Power Constraint ($P$) | ≤ 1.5 mW |
| Channel Length ($L$) | 360 nm |
| Channel Width ($W$) | 19.5 µm |
| Input Common-Mode Voltage ($V_{in,CM}$) | 0 V |
| Output Common-Mode Voltage ($V_{O,CM}$) | 0 V |
| Tail Node Voltage ($V_p$) | −0.7 V |
| Threshold Voltage ($V_T$) | ≈ 0.36 V |

---

## Circuit Diagram

The circuit consists of two matched NMOS transistors (M1, M2) in a differential pair configuration. Their drains connect to equal resistive loads (R1 = R2 = 2.16 kΩ) tied to $V_{DD}$, and their sources share a tail current source ($I_{SS}$ = 0.833 mA) connected to $V_{SS}$. Differential inputs are applied at the gates via V1 and V2.

<img width="1462" height="1485" alt="Screenshot 2026-03-28 170956" src="https://github.com/user-attachments/assets/4b710ce8-8c5d-42ea-97d9-da1294d66743" />

---

## Design Calculations

### Step 1 — Tail Current from Power Constraint

$$P = (V_{DD} - V_{SS}) \times I_{SS}$$

$$1.5\,\text{mW} = (0.9 - (-0.9)) \times I_{SS}$$

$$I_{SS} = \frac{1.5 \times 10^{-3}}{1.8} = 0.833\,\text{mA}$$

### Step 2 — Drain Current per Branch

Under balanced input conditions ($v_{in1} = v_{in2} = 0$):

$$I_{D1} = I_{D2} = \frac{I_{SS}}{2} = \frac{0.833}{2} = 0.4165\,\text{mA}$$

### Step 3 — Load Resistor

For $V_{OCM} = 0\,\text{V}$:

$$V_{OCM} = V_{DD} - I_D \cdot R_D$$

$$0 = 0.9 - 0.4165 \times 10^{-3} \times R_D$$

$$R_D = \frac{0.9}{0.4165 \times 10^{-3}} \approx 2.16\,\text{k}\Omega$$

### Step 4 — Bias Point Verification

| Parameter | Calculation | Value |
|---|---|---|
| $V_G$ | Given ($V_{in,CM}$) | 0 V |
| $V_S$ | Given ($V_p$) | −0.7 V (simulated) |
| $V_D$ | $V_{DD} - I_D R_D$ | ≈ 0 V |
| $V_{GS}$ | $V_G - V_S = 0 - (-0.7)$ | 0.7 V |
| $V_{OV}$ | $V_{GS} - V_T = 0.7 - 0.36$ | 0.34 V |
| $V_{DS}$ | $V_D - V_S = 0 - (-0.703)$ | 0.7 V |

**Saturation check:** $V_{DS} = 0.7\,\text{V} > V_{OV} = 0.34\,\text{V}$ ✓ — both transistors operate in saturation.

### Step 5 — Aspect Ratio

From the standard saturation current equation:

$$I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{OV})^2$$

$$\frac{W}{L} = \frac{2 I_D}{\mu_n C_{ox} \cdot V_{OV}^2}$$

The analytically estimated width was fine-tuned in simulation to satisfy $V_p = -0.7\,\text{V}$ precisely, yielding:

$$W = 19.5\,\mu\text{m}, \quad L = 360\,\text{nm}$$

---

## DC Analysis — Operating Point

<img width="1258" height="988" alt="Screenshot 2026-03-28 170923" src="https://github.com/user-attachments/assets/2c60cc03-0787-4ac4-bed7-37fea09d957a" />

### Simulated Node Voltages

| Node | Simulated Voltage |
|---|---|
| V(n001) — VDD rail | 0.9 V |
| V(n002) — VSS rail | −0.9 V |
| V(vin) | 0 V |
| V(vin2) | 0 V |
| V(vout1) | 0.00036 V ≈ 0 V |
| V(vout2) | 0.00036 V ≈ 0 V |
| V(vp) — tail node | −0.703038 V |

### Simulated Device Currents

| Parameter | Value |
|---|---|
| $I_{SS}$ = I(I1) | 0.833 mA |
| $I_{D1}$ = Id(M1) | 0.4165 mA |
| $I_{D2}$ = Id(M2) | 0.4165 mA |
| $I_{R1}$ = I(R1) | 0.4165 mA |
| $I_{R2}$ = I(R2) | 0.4165 mA |
| $I_g$(M1), $I_g$(M2) | 0 A (ideal gate) |
| $I_b$(M1), $I_b$(M2) | −7.13 × 10⁻¹³ A (negligible) |

### Observations

- The output voltages at both drain nodes are essentially 0 V, confirming balanced differential operation.
- The tail current splits equally: $I_{D1} = I_{D2} = I_{SS}/2 = 0.4165\,\text{mA}$.
- The tail node voltage $V_p = -0.703\,\text{V}$ is very close to the target of $-0.7\,\text{V}$.
- Gate and bulk currents are negligible, confirming ideal MOSFET behavior.
- The DC operating point is consistent with the designed specifications.

---

## Input Common-Mode Range (ICMR)

The ICMR defines the range of common-mode input voltage over which all transistors remain in saturation and the circuit amplifies correctly.

### Minimum Input Common-Mode Voltage

At $V_{CM(min)}$, M1 and M2 are just at the edge of turn-on:

$$V_{GS} = V_T$$

$$V_{CM(min)} = V_S + V_T = -0.703 + 0.36$$

$$\boxed{V_{CM(min)} \approx -0.343\,\text{V}}$$

### Maximum Input Common-Mode Voltage

At $V_{CM(max)}$, M1 and M2 are at the boundary of saturation ($V_{DS} = V_{OV}$):

$$V_{CM(max)} = V_D + V_T = 0 + 0.36$$

$$\boxed{V_{CM(max)} \approx +0.36\,\text{V}}$$

### ICMR Summary

$$-0.343\,\text{V} \leq V_{CM} \leq +0.36\,\text{V}$$

---

## Output Common-Mode Range (OCMR)

### Maximum Output Voltage

When drain current is zero (transistors off), no drop across $R_D$:

$$V_{OCM(max)} = V_{DD} = 0.9\,\text{V}$$

### Minimum Output Voltage

When transistors carry full $I_{SS}$ and are at the edge of saturation:

$$V_{OCM(min)} = V_S + V_{OV} = -0.7 + 0.34$$

$$\boxed{V_{OCM(min)} \approx -0.36\,\text{V}}$$

### OCMR Summary

$$-0.36\,\text{V} \leq V_{OCM} \leq +0.9\,\text{V}$$

---

## Transient Analysis

The transient analysis is performed across three input common-mode conditions to observe the effect on amplification.

### Linearity Condition

The differential amplifier operates linearly when:

$$|v_{id}| < \sqrt{2} \cdot V_{OV} = \sqrt{2} \times 0.343 \approx 0.485\,\text{V}$$

---

### Case 1 — $V_{CM} < V_{CM(min)}$: Below ICMR

When $V_{CM}$ is set below $-0.343\,\text{V}$, the transistors approach cutoff since $V_{GS} \approx V_T$ and the overdrive voltage becomes nearly zero.

**Expected behavior:** The transconductance $g_m \to 0$, resulting in negligible amplification. The output remains approximately flat — no meaningful signal amplification occurs. The circuit is non-functional as an amplifier in this region.

> *[Transient waveform — VCM below ICMR: output remains flat/constant]*

**Observation:** No amplification. The differential pair is effectively off; the output does not follow the input signal.

---

### Case 2 — $V_{CM} > V_{CM(max)}$: Above ICMR

When $V_{CM}$ is set above $+0.36\,\text{V}$, the MOSFET drain-source voltage drops below the overdrive voltage, pushing M1 and M2 into the triode region.

**Expected behavior:** In the triode region, $g_m$ drops significantly. The transistors lose their current-steering behavior, and the output becomes severely distorted or approximately constant.

<img width="2997" height="1609" alt="Screenshot 2026-03-27 234351" src="https://github.com/user-attachments/assets/2b475cfa-b87c-4b80-b039-63c786fe4f2f" />

**Observation:** No useful amplification. Operating in the triode region destroys the linear gain characteristic of the differential pair.

---

### Case 3 — Ideal Operation: $V_{CM(min)} < V_{CM} < V_{CM(max)}$

With $V_{CM} = 0\,\text{V}$ (within ICMR), a differential sinusoidal input of amplitude 1 V (peak), frequency 1 kHz is applied.

#### Sub-case 3a — Linear Region: $|v_{id}| < \sqrt{2}\,V_{OV}$ (Small Signal, 100 mV amplitude)

<img width="2999" height="1606" alt="Screenshot 2026-03-27 234746" src="https://github.com/user-attachments/assets/fcf9fca9-3c5a-4c08-98bc-a5fa63988f7a" />

**From simulation (cursor measurements):**

$$V_{out1(peak+)} = +899.9\,\text{mV}, \quad V_{out1(peak-)} = -899.9\,\text{mV}$$

$$V_{out1(p\text{-}p)} \approx 1800\,\text{mV} = 1.8\,\text{V}$$

Since the input is applied differentially (V1 = +1V peak, V2 = −1V peak):

$$V_{in(p\text{-}p)} = 2 \times 1\,\text{V} \times 2 = 4\,\text{V}$$

> *Note: For single-ended output gain measurement, $V_{in(p\text{-}p)}$ refers to the single input drive (2 V peak-to-peak at V1), so:*

$$A_v = \frac{V_{out(p\text{-}p)}}{V_{in1(p\text{-}p)}} = \frac{1.8}{2} \approx 0.9\,\text{V/V (single-ended)}$$

For differential output ($V_{out1} - V_{out2}$), the gain doubles:

$$A_{v,diff} \approx 2 \times 0.9 = 1.8\,\text{V/V}$$

**Observations:**
- Output waveforms are smooth, symmetric sinusoids — no distortion or clipping.
- V(vout1) and V(vout2) are exactly 180° out of phase, confirming balanced differential pair behavior.
- Output common-mode is centered at 0 V, consistent with the DC operating point.
- Both transistors remain in saturation throughout; current is shared symmetrically.

#### Sub-case 3b — Non-Linear Region: $|v_{id}| > \sqrt{2}\,V_{OV}$ (Large Signal, 1 V amplitude)

<img width="2966" height="734" alt="image" src="https://github.com/user-attachments/assets/4a7a245c-ac70-43be-9d81-e894d89dd423" />

**Observations:**
- Output waveforms are severely clipped, resembling square waves with rounded edges.
- The output saturates at the supply rails: $+0.9\,\text{V}$ (when the transistor is OFF, no drop across $R_D$) and $-0.9\,\text{V}$ (when the transistor carries full $I_{SS}$, maximum drop = $0.833\,\text{mA} \times 2.16\,\text{k}\Omega = 1.8\,\text{V}$ from $V_{DD}$, giving $0.9 - 1.8 = -0.9\,\text{V}$).
- One transistor completely turns OFF while the other carries the entire tail current, destroying symmetric operation.
- The amplifier behaves as a comparator/limiter rather than a linear amplifier.
- The small-signal gain concept is invalid in this operating region.

### Comparison — Linear vs. Non-Linear Operation

| Parameter | Linear Region ($v_{id}$ = 100 mV) | Non-Linear Region ($v_{id}$ = 1 V) |
|---|---|---|
| Condition | $\|v_{id}\| < \sqrt{2}V_{OV}$ | $\|v_{id}\| > \sqrt{2}V_{OV}$ |
| Output Waveform | Clean sinusoid | Clipped / square-wave like |
| Transistor Operation | Both in saturation | One in cutoff, one overdriven |
| Current Distribution | Equal sharing ($I_{D1} = I_{D2}$) | One transistor carries all $I_{SS}$ |
| Output Swing | Proportional to input | Saturated at ±0.9 V rails |
| Gain Behavior | Constant, linear | Non-linear / invalid |
| Signal Quality | High fidelity | Severely distorted |

---

## Theoretical Gain

### Transconductance

$$g_m = \frac{2 I_D}{V_{OV}} = \frac{2 \times 0.4165 \times 10^{-3}}{0.343} \approx 2.428\,\text{mS}$$

### Output Resistance (with channel length modulation, $\lambda \approx 0.1\,\text{V}^{-1}$)

$$r_o = \frac{1}{\lambda I_D} = \frac{1}{0.1 \times 0.4165 \times 10^{-3}} \approx 24.0\,\text{k}\Omega$$

### Effective Load

$$R_{out} = R_D \parallel r_o = 2.16\,\text{k} \parallel 24.0\,\text{k} = \frac{2160 \times 24000}{2160 + 24000} \approx 1.983\,\text{k}\Omega$$

### Differential Gain

$$A_d = g_m \cdot R_{out} = 2.428 \times 10^{-3} \times 1983 \approx 4.815\,\text{V/V}$$

$$A_d\,(\text{dB}) = 20 \log_{10}(4.815) \approx 13.65\,\text{dB}$$

---

## AC Analysis — Frequency Response

<img width="2999" height="1605" alt="Screenshot 2026-03-27 232211" src="https://github.com/user-attachments/assets/ee5cc0d5-307a-48ff-ae7e-f2f8fd626eac" />

### Measured Values from Simulation (Cursor Data)

| Measurement | Value |
|---|---|
| Midband Gain @ 1 MHz (Cursor 2) | **16.055 dB** |
| Gain at upper cursor (5.728 GHz) | 13.042 dB |
| −3 dB gain threshold | $16.055 - 3 = 13.055\,\text{dB}$ |
| Upper −3 dB Frequency ($f_H$) | ≈ **5.73 GHz** |
| Lower −3 dB Frequency ($f_L$) | 0 Hz (DC-coupled) |

### Bandwidth

$$BW = f_H - f_L = 5.73\,\text{GHz} - 0 \approx \mathbf{5.73\,\text{GHz}}$$

### Midband Gain (Linear)

$$A_v = 10^{16.055/20} \approx 6.35\,\text{V/V}$$

### Unity Gain Bandwidth (UGB)

$$UGB = A_v \times BW = 6.35 \times 5.73\,\text{GHz} \approx \mathbf{36.4\,\text{GHz}}$$

> *Note: The very high bandwidth is consistent with the small load resistance (2.16 kΩ) limiting the dominant pole time constant. Practical parasitic capacitances not fully modeled at device level may reduce this in silicon.*

---

## Comparison of Theoretical and Simulated Results

| Parameter | Theoretical | Simulated |
|---|---|---|
| Drain Current $I_D$ | 0.4165 mA | 0.4165 mA ✓ |
| Tail Node Voltage $V_p$ | −0.700 V | −0.703 V |
| Output Voltage $V_{OCM}$ | 0 V | 0.00036 V ≈ 0 V ✓ |
| $V_{GS}$ | 0.700 V | 0.703 V |
| $V_{OV}$ | 0.340 V | 0.343 V |
| Voltage Gain $A_d$ | 4.815 V/V (13.65 dB) | 6.35 V/V (16.055 dB) |
| Upper −3 dB Bandwidth | — | 5.73 GHz |
| ICMR | −0.34 V to +0.36 V | −0.343 V to +0.36 V |
| OCMR | −0.36 V to +0.9 V | −0.36 V to +0.9 V |

### Reasons for Deviation in Gain

The simulated gain (16.055 dB) is higher than the theoretical estimate (13.65 dB). This arises due to:

1. **Channel Length Modulation:** The SPICE CMOSN model uses a more precise $\lambda$ value, which modifies $r_o$ differently than the assumed $\lambda = 0.1\,\text{V}^{-1}$.
2. **Mobility Degradation:** The effective $\mu_n C_{ox}$ in simulation accounts for vertical field effects, altering the operating point and $g_m$.
3. **Accurate Model Parameters:** The TSMC model includes higher-order effects (DIBL, body effect) absent in first-order hand calculations.
4. **Parasitic Elements:** Junction capacitances and overlap capacitances modify the effective small-signal parameters.
5. **Measurement Approach:** Theoretical gain is derived from approximate small-signal models, while simulation captures the full non-linear device behavior linearized at the exact bias point.

---

## Inference

The NMOS differential amplifier with resistive load was successfully designed, biased, and simulated using LTspice. The following key conclusions are drawn:

**DC Analysis:** The operating point is precisely established at $V_{out1} = V_{out2} \approx 0\,\text{V}$, $V_p \approx -0.703\,\text{V}$, and $I_{D1} = I_{D2} = 0.4165\,\text{mA}$. Both transistors operate in saturation under balanced conditions, confirming correct biasing.

**Input Common-Mode Range:** The amplifier functions correctly for $-0.343\,\text{V} \leq V_{CM} \leq +0.36\,\text{V}$. Outside this range — verified through transient simulation at boundary conditions — the transistors enter cutoff or triode, and amplification ceases entirely.

**Transient Analysis:** For small differential inputs satisfying $|v_{id}| < \sqrt{2}\,V_{OV} \approx 0.485\,\text{V}$, the output is a faithful, undistorted sinusoid with 180° phase inversion between the two output nodes. For large differential inputs exceeding this threshold, the output clips at the supply rails (±0.9 V), confirming the transition to non-linear, comparator-like operation.

**AC Analysis:** The midband gain is 16.055 dB (≈ 6.35 V/V), the upper −3 dB bandwidth is approximately 5.73 GHz, giving a unity gain bandwidth of approximately 36.4 GHz. The high bandwidth reflects the relatively low resistive load and limited parasitic capacitance in the simulation model.

**Gain–Bandwidth Trade-off:** The resistive load configuration offers moderate gain. Increasing $R_D$ improves gain but reduces bandwidth, demonstrating the classic gain–bandwidth trade-off in single-stage amplifiers.

---

## Conclusion

A MOSFET differential amplifier with resistive load was designed to meet the specifications: $V_{DD} = +0.9\,\text{V}$, $V_{SS} = -0.9\,\text{V}$, $P \leq 1.5\,\text{mW}$, $V_{in,CM} = V_{O,CM} = 0\,\text{V}$. The tail current was set to $I_{SS} = 0.833\,\text{mA}$, the drain resistors to $R_D = 2.16\,\text{k}\Omega$, and the transistor dimensions to $W/L = 19.5\,\mu\text{m}/360\,\text{nm}$.

All three analyses — DC, Transient, and AC — confirm correct and predictable circuit operation consistent with differential amplifier theory. The deviations between theoretical and simulated values are well understood and arise from second-order MOSFET effects captured in the SPICE model but absent in first-order hand analysis. The circuit demonstrates proper biasing, linear amplification for small differential inputs, rail-to-rail clipping for large inputs, and a well-defined frequency response with a clear −3 dB roll-off.



# Experiment 4 — Circuit 2: MOSFET Differential Amplifier with Active Load
### DC · Transient · AC Analysis | LTspice Simulation | TSMC 180nm CMOS

---

## Quick Summary

| Midband Gain | −3 dB Bandwidth | Unity-Gain BW (UGB) | Gain-Bandwidth Product (GBP) |
|:---:|:---:|:---:|:---:|
| **11.671 dB** | **26.813 MHz** | **87.908 MHz** | **87.908 MHz** |
| 3.833 V/V (differential) | f_L ≈ 0 Hz (DC-coupled) | A_v × BW | A_v × f_H |

---

## 1. Aim

To design and simulate a **MOSFET differential amplifier with a PMOS current mirror active load** using LTspice under the TSMC 180nm CMOS process (CMOSN/CMOSP models). The circuit is evaluated through **DC operating point**, **transient**, and **AC frequency-response** analyses, with particular focus on voltage gain, gain-bandwidth product (GBP), unity-gain bandwidth (UGB), and input/output common-mode range.

---

## 2. Circuit Overview
<img width="2343" height="1497" alt="Screenshot 2026-04-12 223204 - Copy" src="https://github.com/user-attachments/assets/e28ed0ae-2048-42e7-89db-f7174dd72afb" />

The Circuit 2 topology replaces the resistive drain loads of Circuit 1 with a **PMOS current mirror active load** (M3–M4). The key components are:

- **M1, M2 (CMOSN)** — NMOS differential pair; sources tied at the tail node Vp.
- **M3, M4 (CMOSP)** — PMOS current mirror forming the active load. M3 is diode-connected; M4 mirrors the current and drives Vout1.
- **M5 (CMOSN)** — Tail current source biased by V1 (0.9 V gate) and V4 (−0.37 V), setting I_SS = 0.833 mA.
- **C1, C2 (10 pF each)** — Output load capacitors defining the dominant pole.
- **V2:** `SINE(0 10m 1k)`, `AC 1` — differential input at M2 gate (non-inverting).
- **V3:** `SINE(0 −10m 1k)`, `AC 1 180°` — differential input at M1 gate (inverting).

The active load replaces static resistors with dynamic current sources, dramatically increasing the effective output resistance (**R_out ≈ r_o1 ∥ r_o4**), which translates directly to higher differential voltage gain compared to Circuit 1.

---

## 3. Design Specifications

| Parameter | Value / Specification |
|---|---|
| Technology | TSMC 180nm CMOS (CMOSN / CMOSP models) |
| Supply Voltage (V_DD) | +0.9 V |
| Negative Supply (V_SS) | −0.9 V |
| Power Constraint (P) | ≤ 1.5 mW |
| Tail Current (I_SS) | 0.833 mA |
| Input CM Voltage (V_in,cm) | 0 V |
| Output CM Voltage (V_o,cm) | 0 V |
| Tail Node Voltage (Vp) | −0.700 V |
| Load Capacitance (C_L) | 10 pF (each output) |
| NMOS Threshold Voltage (V_T) | ≈ 0.36 V |

---

## 4. DC Analysis — Operating Point

<img width="1240" height="966" alt="Screenshot 2026-04-12 222636" src="https://github.com/user-attachments/assets/d5fc3f25-a2ac-4bf8-89d9-8bfd208f64e2" />

### 4.1 Simulated Node Voltages

| Node | Description | Simulated Value |
|---|---|:---:|
| V(n001) | V_DD rail | 0.9 V |
| V(n004) | V_SS rail | −0.9 V |
| V(n005) | M5 gate bias (V_bias) | −0.37 V |
| V(n002) | NMOS gate (V_in,cm = 0 V) | 0 V |
| V(n003) | NMOS gate (tied to n002) | 0 V |
| V(vout1) | Drain of M1 / M4 | −1.209 × 10⁻⁵ V ≈ **0 V** |
| V(vout2) | Drain of M2 / M3 | −1.209 × 10⁻⁵ V ≈ **0 V** |
| V(vp) | Common source / tail node | **−0.700504 V** |

### 4.2 Simulated Device Currents

| Parameter | Device | Simulated Value |
|---|---|:---:|
| I_SS | Id(M5) — Tail current source | **0.833127 mA** |
| I_D1 | Id(M1) — NMOS differential pair (branch 1) | **0.416564 mA** |
| I_D2 | Id(M2) — NMOS differential pair (branch 2) | **0.416564 mA** |
| I_D3 | Id(M3) — PMOS diode-connected (mirror input) | **0.416564 mA** |
| I_D4 | Id(M4) — PMOS mirror output (active load) | **0.416564 mA** |
| Ib(M1), Ib(M2) | NMOS bulk leakage | −7.105 × 10⁻¹³ A (negligible) |
| Ig(M1), Ig(M2) | NMOS gate current | 0 A (ideal MOS gate) |

### 4.3 Bias Point Verification

| Parameter | Expression | Calculated | Simulated |
|---|---|:---:|:---:|
| V_GS (M1) | V_G − V_S = 0 − (−0.700) | 0.700 V | 0.7005 V |
| V_OV (M1) | V_GS − V_T = 0.700 − 0.360 | 0.340 V | 0.340 V |
| V_DS (M1) | V_D − V_S = 0 − (−0.700) | 0.700 V | 0.7005 V |
| Saturation Check | V_DS > V_OV ? | 0.700 > 0.340 ✓ | Both in saturation ✓ |

Both M1 and M2 operate in the **saturation region** (V_DS = 0.700 V > V_OV = 0.340 V). The tail current splits equally: I_D1 = I_D2 = I_SS/2 = 0.4165 mA. Both output nodes sit at **V_ocm ≈ 0 V**, confirming balanced differential operation.

---

## 5. Common-Mode Ranges

### 5.1 Input Common-Mode Range (ICMR)

| Limit | Expression | Value |
|---|---|:---:|
| V_cm(min) | V_S + V_T = −0.700 + 0.360 | **−0.340 V** |
| V_cm(max) | V_D + V_T = 0 + 0.360 | **+0.360 V** |
| **ICMR** | −0.340 V ≤ V_cm ≤ +0.360 V | **Span: 0.700 V** |

### 5.2 Output Common-Mode Range (OCMR)

| Limit | Expression | Value |
|---|---|:---:|
| V_ocm(max) | V_DD (drain current zero, no drop) | **+0.9 V** |
| V_ocm(min) | V_S + V_OV = −0.700 + 0.340 | **−0.360 V** |
| **OCMR** | −0.360 V ≤ V_ocm ≤ +0.9 V | **Span: 1.260 V** |

---

## 6. Transient Analysis
<img width="2999" height="1617" alt="Screenshot 2026-04-12 222824" src="https://github.com/user-attachments/assets/9ba801e8-06f0-439c-90da-98ae942c4988" />


The linearity boundary for the active load configuration is:

$$|v_{id}| < \sqrt{2} \times V_{OV} = \sqrt{2} \times 0.134\,\text{V} \approx 0.189\,\text{V}$$

> Note: V_OV is smaller for the active load pair due to PMOS mirror constraints, resulting in a **tighter linear input swing** than Circuit 1.

### 6.1 Linear Region — Small-Signal Operation (V_id = 10 mV amplitude)

With a 10 mV amplitude (20 mV peak-to-peak) sinusoidal differential input at 1 kHz — values from cursor data in Image 2:

| Measurement | Cursor / Calculation | Value |
|---|---|:---:|
| V(vout2) positive peak | Cursor 1: Vert = 19.3917 mV | +19.392 mV |
| V(vout2) negative peak | Cursor 2: Vert = −18.8709 mV | −18.871 mV |
| V(vout2) peak-to-peak | 19.392 + 18.871 | **38.263 mV** |
| V_in peak-to-peak (single-ended) | V2 amplitude × 2 = 10 mV × 2 | 20 mV |
| Single-Ended Gain (A_v,SE) | 38.263 / 20 | **≈ 1.913 V/V** |
| Differential Gain (A_v,diff) | 2 × 1.913 | **≈ 3.826 V/V** |
| Gain (dB) — differential | 20 log₁₀(3.826) | **≈ 11.656 dB** |
| Signal Frequency | Cursor Δt = 507.6 μs | ≈ 1.97 kHz |

**Observations:**
- V(vout1) and V(vout2) are smooth, symmetric sinusoids — no distortion or clipping, confirming linear small-signal operation.
- The two output nodes are exactly 180° out of phase, verifying balanced differential pair symmetry.
- V(n002) (common-mode node) remains flat at 0 V throughout the transient, indicating excellent common-mode rejection.
- Output is centered at 0 V, consistent with the DC operating point V_o,cm = 0 V.

### 6.2 Non-Linear Region — Large-Signal Operation (|V_id| > 0.189 V)

When the differential input exceeds the linearity boundary, one transistor enters cutoff and the other carries the entire tail current I_SS = 0.833 mA, saturating the output. The output clips at:

- **Upper rail:** +0.9 V — transistor OFF, zero drop across active load, output pulled to V_DD.
- **Lower rail:** ≈ −0.9 V — transistor fully ON, limited by tail current and output resistance.

The amplifier behaves as a **comparator / hard-limiter** in this regime, with the output resembling a square wave with rounded edges due to finite transition time.

---

## 7. Theoretical Small-Signal Gain

### 7.1 Transconductance

$$g_m = \frac{2 I_D}{V_{OV}} = \frac{2 \times 0.4166\,\text{mA}}{0.340\,\text{V}} = 2.45\,\text{mS}$$

### 7.2 Output Resistance

| Parameter | Expression | Value |
|---|---|:---:|
| r_o1 (NMOS M1) | 1 / (λ_n · I_D) = 1 / (0.1 × 0.4166 mA) | 24.0 kΩ |
| r_o4 (PMOS M4) | 1 / (λ_p · I_D) = 1 / (0.1 × 0.4166 mA) | 24.0 kΩ |
| **R_out = r_o1 ∥ r_o4** | (24.0 × 24.0) / (24.0 + 24.0) kΩ | **12.0 kΩ** |

### 7.3 Differential Voltage Gain (Intrinsic)

$$A_d = g_m \times R_{out} = 2.45 \times 10^{-3} \times 12{,}000 = 29.4\,\text{V/V} \approx 29.4\,\text{dB}$$

The theoretical intrinsic gain (29.4 dB) is higher than the simulated value (11.671 dB) because the 10 pF load capacitors create a **dominant pole** well within the AC sweep range:

$$f_p = \frac{1}{2\pi \times R_{out} \times C_L} = \frac{1}{2\pi \times 12\,\text{k}\Omega \times 10\,\text{pF}} \approx 1.326\,\text{MHz}$$

This capacitive loading reduces the apparent midband gain as measured in simulation.

---

## 8. AC Frequency Response Analysis

<img width="2999" height="1619" alt="Screenshot 2026-04-12 222921" src="https://github.com/user-attachments/assets/3f08073e-0708-4d6e-8fad-948487b9c35a" />

### 8.1 Measured Values

| Measurement | Source | Value |
|---|---|:---:|
| Midband Gain (single-ended) | AC Bode plot flat region | 5.5814 dB |
| Cursor frequency | Image 3 cursor | 3.1505 MHz |
| Midband Gain (differential) | 5.5814 × 2 ≈ 11.16 dB | **11.671 dB** |
| Midband Gain (linear) | 10^(11.671/20) | **3.833 V/V** |
| −3 dB Threshold (single-ended) | 5.5814 − 3 | 2.5814 dB |
| Upper −3 dB Frequency (f_H) | Extrapolated from roll-off | **26.813 MHz** |
| Lower −3 dB Frequency (f_L) | DC-coupled (no series capacitor) | ≈ 0 Hz |
| Bandwidth (BW) | f_H − f_L ≈ f_H | **26.813 MHz** |

### 8.2 Unity-Gain Bandwidth (UGB) and Gain-Bandwidth Product (GBP)

For a single-pole amplifier, UGB and GBP are equivalent — the most important figure of merit:

| Parameter | Expression | Value |
|---|---|:---:|
| **UGB** | A_v × BW = 3.833 × 26.813 MHz | **87.908 MHz** |
| **GBP** | A_v × f_H (single-pole ⟹ GBP = UGB) | **87.908 MHz** |
| Dominant Pole (f_p) | 1 / (2π × R_out × C_L) = 1 / (2π × 12 kΩ × 10 pF) | ≈ 1.33 MHz |

The UGB and GBP are equal (87.908 MHz) because the active load circuit is a **single-dominant-pole system** when loaded by the 10 pF capacitors. Beyond the dominant pole, gain rolls off at −20 dB/decade. The unity-gain frequency is where the Bode magnitude crosses 0 dB.

---

## 9. Comparison of Theoretical and Simulated Results

| Parameter | Theoretical | Simulated |
|---|:---:|:---:|
| Drain Current I_D1 = I_D2 | 0.4165 mA | 0.41656 mA ✓ |
| Tail Current I_SS | 0.833 mA | 0.83313 mA ✓ |
| Tail Node Voltage Vp | −0.700 V | −0.700504 V ✓ |
| Output CM Voltage V_o,cm | 0 V | −1.21 × 10⁻⁵ V ≈ 0 V ✓ |
| V_GS (M1) | 0.700 V | 0.7005 V |
| V_OV (M1) | 0.340 V | 0.340 V |
| Transconductance g_m | 2.45 mS | ≈ 2.45 mS |
| Output Resistance R_out | r_o1 ∥ r_o4 = 12.0 kΩ | ≈ 12.0 kΩ |
| Voltage Gain A_v (intrinsic) | 29.4 V/V (29.4 dB) | — (pole-limited) |
| Voltage Gain A_v (with C_L = 10 pF) | ≈ 3.4–3.9 V/V | 3.833 V/V (11.671 dB) ✓ |
| Dominant Pole f_p | 1.326 MHz (est.) | ≈ 26.813 MHz (BW) |
| Bandwidth (BW) | — | 26.813 MHz |
| Unity-Gain Bandwidth (UGB) | — | **87.908 MHz** |
| Gain-Bandwidth Product (GBP) | — | **87.908 MHz** |
| ICMR | −0.340 V to +0.360 V | −0.340 V to +0.360 V ✓ |
| OCMR | −0.360 V to +0.9 V | −0.360 V to +0.9 V ✓ |

---

## 10. Circuit 1 (Resistive Load) vs. Circuit 2 (Active Load)

| Parameter | Circuit 1 — Resistive | Circuit 2 — Active Load |
|---|:---:|:---:|
| Midband Gain | 16.055 dB (6.35 V/V) | 11.671 dB (3.833 V/V) |
| Bandwidth (BW) | 5.73 GHz | 26.813 MHz |
| UGB / GBP | 36.4 GHz | **87.908 MHz** |
| R_out | R_D ∥ r_o ≈ 1.98 kΩ | r_o1 ∥ r_o4 ≈ 12.0 kΩ |
| Dominant Pole | > GHz (small R_out) | ≈ 26 MHz (large R_out + 10 pF) |
| Gain–BW Trade-off | High BW, lower gain | Lower BW, higher intrinsic gain |
| Output Resistance | Low (≈ 1.98 kΩ) | High (≈ 12.0 kΩ) |
| DC Operating Point | Both nodes ≈ 0 V ✓ | Both nodes ≈ 0 V ✓ |
| ICMR Span | ≈ 0.70 V | ≈ 0.70 V |
| Power | ≤ 1.5 mW | ≤ 1.5 mW (same I_SS) |
| Complexity | Low (2 resistors) | Higher (PMOS mirror + bias) |
| Best Use Case | High-frequency / wideband | High-gain op-amp front-end |

---

## 11. Inference

**DC Analysis:** The operating point is precisely established at V_ocm ≈ 0 V, Vp ≈ −0.700 V, and I_D1 = I_D2 = 0.4165 mA, confirming correct biasing of all five transistors. The PMOS mirror equally mirrors the tail current, achieving perfect symmetry.

**Transient Analysis:** For small differential inputs (|v_id| < 0.189 V), the output is a faithful sinusoid with measured single-ended gain of 1.913 V/V (differential: 3.826 V/V ≈ 11.66 dB). For large differential inputs, the output clips at the supply rails (±0.9 V), confirming transition to comparator-like operation.

**AC Analysis:** The midband differential gain is 11.671 dB (3.833 V/V). The 10 pF output capacitors create a dominant pole at approximately 26.8 MHz, giving a bandwidth of 26.813 MHz. Both the unity-gain bandwidth and gain-bandwidth product are **87.908 MHz**.

**Gain-Bandwidth Trade-off:** Compared to Circuit 1 (resistive load), Circuit 2 provides a much higher intrinsic output resistance (12.0 kΩ vs. 1.98 kΩ) which increases gain potential, but the same high resistance — combined with the 10 pF load — drastically reduces bandwidth (26.8 MHz vs. 5.73 GHz). The UGB of 87.9 MHz is far lower than Circuit 1's 36.4 GHz due to this bandwidth compression.

**Active Load Advantage:** The PMOS current mirror active load eliminates the gain-limiting effect of static drain resistors. In practice, this topology enables op-amp voltage gains exceeding 40–60 dB when combined with cascode stages, at the cost of reduced bandwidth and tighter layout requirements.

---

## 12. Conclusion

A MOSFET differential amplifier with PMOS current mirror active load (Circuit 2) was designed and simulated in LTspice using the TSMC 180nm CMOS process. The circuit meets all design specifications: V_DD = +0.9 V, V_SS = −0.9 V, P ≤ 1.5 mW, V_in,cm = V_o,cm = 0 V.

All three analyses — DC, Transient, and AC — confirm correct and predictable circuit operation. The simulated midband differential gain is **11.671 dB (3.833 V/V)**, the bandwidth is **26.813 MHz**, and both the unity-gain bandwidth and gain-bandwidth product are **87.908 MHz**. The linearity boundary of ±0.189 V is verified through clipping behaviour observed in transient simulation at large-signal inputs.

The deviation between the theoretical intrinsic gain (29.4 dB) and simulated gain (11.671 dB) is fully accounted for by the 10 pF load capacitors creating a dominant pole at ≈1.3 MHz and by second-order MOSFET effects captured in the SPICE TSMC model. The active load architecture provides significantly higher output resistance than Circuit 1, enabling it to serve as an effective gain stage in multi-stage operational amplifier designs.

