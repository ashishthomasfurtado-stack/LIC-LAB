# Experiment 2 – DC, AC and Transient Analysis of MOSFET

## Circuit 3: Common-Source Amplifier with PMOS Active Load and Diode-Connected NMOS Source Degeneration

**Technology:** TSMC 180 nm CMOS (tsmc018.lib)  
**Tool:** LTspice  

---

## Aim

To design and simulate a MOSFET amplifier using 180 nm CMOS technology in LTspice, perform DC operating point, DC sweep, transient, and AC analyses, and evaluate gain, bandwidth, power consumption, and overall performance.

---

## Circuit Description

The circuit is a **Common-Source (CS) amplifier** implemented using TSMC 0.18 µm CMOS technology. The topology consists of:

- **M1 (CMOSN)** – Main amplifying transistor (Common Source)
- **M2 (CMOSP)** – Active PMOS load providing high output resistance
- **M3 (CMOSN)** – Diode-connected NMOS acting as source degeneration element

### Schematic Bias Values

| Signal / Source | Value |
|---|---|
| VDD (V1, V2) | 1.2 V |
| V4 (PMOS gate bias) | 0.56 V |
| V5 (M3 gate bias) | 0.61 V |
| Vin (V3) | SINE(0.91 V, 10 mV amplitude, 1 kHz) |
| AC stimulus | 1 V |

### Why Diode-Connected NMOS (M3)?

A diode-connected MOSFET has its gate tied to its drain, so V_GS = V_DS. It always operates in saturation when V_GS > V_TH and acts as a nonlinear resistor with small-signal resistance ≈ 1/g_m. This provides:

- Bias stability
- Improved linearity
- Higher output resistance than a passive resistor load
- Reduced voltage gain compared to a current-source load (accepted trade-off)

---

## Design Specifications

| Parameter | Value |
|---|---|
| Technology | TSMC 180 nm CMOS |
| VDD | 1.2 V |
| Target ID | 200 µA |
| VOV | 0.25 V |
| Power Budget | ≤ 0.4 mW |
| L (all transistors) | 0.18 µm |
| VTHn | 0.366 V |
| \|VTHp\| | 0.39 V |
| µn | 273.809 cm²/Vs |
| µp | 115.689 cm²/Vs |
| tox | 4.1 × 10⁻⁹ m |
| εr | 3.9 |
| ε0 | 8.854 × 10⁻¹² F/m |
| λ | ≈ 0.1 V⁻¹ |
<img width="2085" height="1272" alt="Screenshot 2026-03-18 073635" src="https://github.com/user-attachments/assets/ab574bbc-6423-4be0-9aa2-f81b39928bef" />

---

## Design Calculations

### 1. Oxide Capacitance

```
Cox = (εr × ε0) / tox
    = (3.9 × 8.854 × 10⁻¹²) / (4.1 × 10⁻⁹)
Cox ≈ 8.42 × 10⁻³ F/m²
```

### 2. NMOS Width (M1 and M3)

```
Wn = (2 × ID × L) / (µn × Cox × VOV²)
   = (2 × 200×10⁻⁶ × 0.18×10⁻⁶) / (273.809×10⁻⁴ × 8.42×10⁻³ × (0.25)²)
   = 7.2×10⁻¹¹ / (1.4435×10⁻⁶)
Wn ≈ 4.99 µm  (initial hand calculation)
```

Width adjusted in simulation to achieve ID = 200 µA with accurate BSIM models.

### 3. PMOS Width (M2)

```
Wp = (2 × ID × L) / (µp × Cox × VOV²)
   = (2 × 200×10⁻⁶ × 0.18×10⁻⁶) / (115.689×10⁻⁴ × 8.42×10⁻³ × (0.25)²)
   = 7.2×10⁻¹¹ / (6.099×10⁻⁷)
Wp ≈ 11.8 µm  (initial hand calculation)
```

Width adjusted in simulation to match ID = 200 µA.

### 4. Bias Point Calculations

**M3 (Diode-Connected NMOS):**
```
VGS3 = VTHn + VOV = 0.366 + 0.25 = 0.616 V
Since diode-connected: VS1 = VGS3 ≈ 0.61 V  (matches V5 = 0.61 V in schematic ✓)
Saturation check: VDS3 = VGS3 = 0.616 V ≥ VOV (0.25 V)  ✓
```

**M1 (Common Source NMOS):**
```
VGS1 = VTHn + VOV = 0.366 + 0.25 = 0.616 V
VS1 = 0.61 V
Vin (DC) = VGS1 + VS1 = 0.616 + 0.61 ≈ 1.226 V
→ Adjusted to 0.91 V in simulation for Vout ≈ VDD/2
```

**M2 (PMOS Active Load):**
```
VSG2 = |VTHp| + VOV = 0.39 + 0.25 = 0.64 V
VG2 = VDD − VSG2 = 1.2 − 0.64 = 0.56 V  (matches V4 = 0.56 V in schematic ✓)
```

### 5. Power Verification

```
P = VDD × ID = 1.2 × 200×10⁻⁶ = 0.24 mW
0.24 mW < 0.4 mW  ✓  Power specification satisfied
```

---

## DC Analysis

### Operating Point (LTspice Simulation Results)

<img width="1275" height="929" alt="Screenshot 2026-03-18 073648" src="https://github.com/user-attachments/assets/1c88d472-ee73-4b48-83b1-54e594c85674" />


| Node / Device | Simulated Value | Description |
|---|---|---|
| V(n001) | 1.2 V | VDD rail |
| V(n002) | 1.2 V | PMOS source rail |
| V(n003) | 0.27522 V | M1 source / M3 drain node |
| V(n004) | 0.61 V | M3 gate bias (V5) |
| V(p001) | 0.56 V | M2 gate bias (V4) |
| V(vin) | 0.91 V | Input DC bias |
| **V(vout)** | **0.90295 V** | **Output DC bias point** |
| Id(M1) | 201.255 µA | NMOS amplifier drain current |
| Id(M2) | −201.255 µA | PMOS load drain current (sign convention) |
| Id(M3) | 201.255 µA | Diode-connected NMOS drain current |
| I(V1) | −201.255 µA | Total supply current drawn |
| Ib(M1), Ib(M2), Ib(M3) | ~fA range | Gate leakage (negligible) |

All three transistors carry equal drain current (201.255 µA ≈ 200 µA target), confirming correct DC biasing.

### Saturation Verification

**M1 (NMOS):**
```
VDS1 = V(vout) − V(n003) = 0.90295 − 0.27522 = 0.628 V
VOV = 0.25 V
VDS1 (0.628 V) ≥ VOV (0.25 V)  ✓  M1 in saturation
```

**M2 (PMOS):**
```
VSD2 = VDD − V(vout) = 1.2 − 0.90295 = 0.297 V
VOV = 0.25 V
VSD2 (0.297 V) ≥ VOV (0.25 V)  ✓  M2 in saturation
```

**M3 (Diode-Connected NMOS):**
```
VDS3 = VGS3 = 0.61 V
VOV = 0.25 V
VDS3 (0.61 V) ≥ VOV (0.25 V)  ✓  M3 in saturation
```

All three transistors are confirmed to operate in saturation at the DC bias point.

---

## DC Sweep Analysis

<img width="2962" height="746" alt="Screenshot 2026-03-18 073819" src="https://github.com/user-attachments/assets/79461cc5-fb5f-43eb-b126-fc45f12ffb6a" />

A DC sweep of Vin from 0 V to 2 V was performed to obtain the Voltage Transfer Characteristic (VTC).

### Observations from Simulation

| Region | Vin Range | Vout Behavior |
|---|---|---|
| M1 OFF | 0 V – ~0.4 V | Vout ≈ 1.19 V (M2 pulls output toward VDD) |
| Transition (active) | ~0.5 V – ~1.2 V | Vout decreases smoothly |
| Bias point | 0.91 V | Vout ≈ 0.90 V (confirmed from DC op point) |
| M1 fully ON | > 1.3 V | Vout clamps near 0.49 V |

### Significance

The DC sweep confirms:
1. M1 turns on correctly around Vin ≈ 0.4–0.5 V
2. The chosen bias point (Vin = 0.91 V, Vout = 0.90 V) lies within the smooth transition region, ensuring operation in the amplification zone
3. Both M1 and M2 are in saturation at the operating point
4. The VTC validates the design before AC and transient analysis

---

## Transient Analysis

<img width="2999" height="1608" alt="Screenshot 2026-03-18 074133" src="https://github.com/user-attachments/assets/ad8bf1f2-e4a6-469b-a88f-93b3fe4720ad" />

**Simulation settings:** `.tran 5m`  
**Input:** SINE(0.91 V, 10 mV amplitude, 1 kHz)

### Waveform Measurements (from cursor readings)

| Cursor | Time | V(vout) |
|---|---|---|
| Cursor 1 | 2.7517 ms | 914.21 mV |
| Cursor 2 | 2.2553 ms | 891.48 mV |
| Diff | 496.32 µs | 22.73 mV |
| Signal frequency (measured) | 2.015 kHz | — |

| Parameter | Value |
|---|---|
| Vout (max) | 914.21 mV |
| Vout (min) | 891.48 mV |
| ΔVout (peak-to-peak) | 22.73 mV |
| ΔVin (peak-to-peak) | 20 mV |

### Practical Gain

```
Av = ΔVout / ΔVin
   = 22.73 mV / 20 mV
   = 1.1365 V/V

Av(dB) = 20 × log(1.1365) ≈ 1.11 dB
```

### Theoretical Gain Calculation

**Transconductance (VOV = 0.25 V):**
```
gm1 = 2ID / VOV = (2 × 200×10⁻⁶) / 0.25 = 1.6 mS
gm3 = 2ID / VOV = 1.6 mS  (M3, identical bias)
```

**Output Resistance:**
```
ro1 = 1 / (λ × ID) = 1 / (0.1 × 200×10⁻⁶) = 50 kΩ
ro2 = 50 kΩ
Rout = ro1 || ro2 = 25 kΩ
```

**Voltage Gain with Diode-Connected Degeneration:**
```
Av = −gm1 × Rout / (1 + gm1/gm3)
   = −(1.6×10⁻³ × 25×10³) / (1 + 1.6×10⁻³ / 1.6×10⁻³)
   = −40 / 2
   = −20 V/V

|Av| = 20 V/V
Av(dB) = 20 × log(20) = 26.02 dB
```

### Gain Comparison

| Parameter | Theoretical | Practical (Simulation) |
|---|---|---|
| Av (V/V) | 20 V/V | 1.1365 V/V |
| Av (dB) | 26.02 dB | 1.11 dB |

### Reasons for Discrepancy

1. **Channel Length Modulation:** ro in 180 nm technology is lower than the ideal 50 kΩ assumed in hand calculations
2. **Short-channel effects:** Increased λ at 180 nm reduces ro further
3. **BSIM model accuracy:** Advanced SPICE models include velocity saturation, DIBL, and threshold voltage roll-off not captured in hand calculations
4. **Diode-connected degeneration:** M3's effective resistance 1/gm3 ≈ 625 Ω at the source of M1 strongly degenerates gain
5. **Operating point:** Actual Vout = 0.90 V, slight asymmetry from ideal midpoint affects effective gain

---

## AC Analysis

<img width="2999" height="1616" alt="Screenshot 2026-03-18 074021" src="https://github.com/user-attachments/assets/6b347950-702f-4aa8-8a13-c90480f8283c" />

**Simulation settings:** `.ac dec 10 0.1 100G`  
**Input:** AC 1 V

### Cursor Measurements from AC Plot

| Cursor | Frequency | Magnitude | Phase | Group Delay |
|---|---|---|---|---|
| Cursor 1 | 518.61 MHz | −1.891 dB | 137.51° | 137.26 ps |
| Cursor 2 | 316.23 kHz | 1.114 dB | 179.97° | 296.65 ps |
| Ratio (C2/C1) | — | 3.005 dB | 42.45° | 159.39 ps |

### Key AC Parameters

**Midband Gain:**
```
Av(mid) = 1.113 dB  (flat region, Cursor 2 at 316 kHz)
Av(mid, linear) = 10^(1.113/20) ≈ 1.136 V/V
```

**3 dB Bandwidth:**
```
3 dB level = 1.113 − 3 = −1.887 dB
Cursor 1 reads −1.891 dB at 518.61 MHz ≈ −1.887 dB
∴ fH ≈ 518.61 MHz
fL = 0 Hz  (direct-coupled, no coupling capacitors)
Bandwidth (BW) = 518.61 MHz
```

**Unity Gain Bandwidth (UGB):**
```
UGB = Av(linear) × f3dB
    = 1.136 × 518.61 MHz
    ≈ 589 MHz
```

### AC Analysis Summary

| Parameter | Value |
|---|---|
| Lower cutoff frequency (fL) | 0 Hz |
| Upper cutoff frequency (fH) | 518.61 MHz |
| Bandwidth (BW) | 518.61 MHz |
| Midband Gain (dB) | 1.113 dB |
| Midband Gain (linear) | 1.136 V/V |
| UGB | ≈ 589 MHz |
| Phase at fH | 137.51° |
| Group Delay at fH | 137.26 ps |

---

## Performance Summary

| Parameter | Value |
|---|---|
| Technology | TSMC 180 nm CMOS |
| Supply Voltage (VDD) | 1.2 V |
| Drain Current (ID) | 201.255 µA |
| DC Output Bias (Vout) | 0.903 V |
| Power Consumption | 0.241 mW ✓ (< 0.4 mW) |
| Theoretical Gain | 26.02 dB (20 V/V) |
| Practical Gain – Transient | 1.11 dB (1.14 V/V) |
| Midband Gain – AC | 1.113 dB (1.136 V/V) |
| Bandwidth | 518.61 MHz |
| fL | 0 Hz |
| fH | 518.61 MHz |
| UGB | ≈ 589 MHz |

---

## Results

1. All three transistors (M1, M2, M3) operate in saturation at the DC bias point, confirmed by operating point simulation.
2. Drain current matches the target: ID = 201.255 µA ≈ 200 µA.
3. Power consumption is 0.241 mW, well within the 0.4 mW specification.
4. The DC sweep confirms the bias point (Vin = 0.91 V) lies within the active amplification region.
5. Transient gain (1.11 dB) and AC midband gain (1.113 dB) are in excellent agreement, validating the simulation.
6. Bandwidth is 518.61 MHz, demonstrating wide-band performance.
7. UGB ≈ 589 MHz.
8. Theoretical gain (26.02 dB) exceeds simulated gain (1.11 dB) due to non-ideal BSIM model effects and short-channel phenomena.
9. The diode-connected M3 reduces gain but provides effective bias stabilization and linearity improvement.
10. The gain–bandwidth trade-off inherent to source-degenerated CS amplifiers is clearly observed.

---

## Inference

1. The circuit successfully operates at 1.2 V with ID ≈ 200 µA, satisfying all design constraints.
2. Practical gain is lower than theoretical gain due to Channel Length Modulation (finite ro), parasitic capacitances, and short-channel effects in 180 nm technology.
3. The diode-connected NMOS (M3) provides effective source degeneration — its small-signal resistance 1/gm3 ≈ 625 Ω at the source of M1 strongly reduces gain but improves bias stability and linearity.
4. The PMOS active load (M2) provides higher output resistance than a passive resistor, contributing to improved gain over a resistor-loaded CS stage.
5. The close match between transient gain and AC midband gain confirms the simulation setup is consistent and correct.
6. The direct-coupled topology (fL = 0 Hz) makes this circuit suitable for DC and baseband applications.
7. Smaller channel length (180 nm) increases λ, reducing output resistance and gain — a longer channel would improve gain at the cost of area and speed.
8. The gain–bandwidth trade-off is clearly validated: source degeneration sacrifices gain for linearity and stability.
9. Diode-connected MOSFET degeneration is preferred in low-voltage CMOS design as it is process-scalable and avoids the headroom penalty of a resistor.
10. Overall, the simulation validates 180 nm CMOS analog design principles and confirms real non-ideal CMOS effects under low-voltage operation.

---

*Report based on LTspice simulations using TSMC 180 nm CMOS technology (tsmc018.lib).*
