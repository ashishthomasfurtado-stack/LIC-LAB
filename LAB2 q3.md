# Circuit 3
# Common-Source Amplifier with Active PMOS Load and Source Degeneration
### TSMC 0.18 µm Technology — LTspice Simulation Report

---

## 1. Introduction

This report presents the design, analysis, and simulation of a CMOS common-source amplifier implemented in TSMC 0.18 µm technology. The circuit employs source degeneration for improved linearity and bias stability, with an active PMOS load for high gain.

**Circuit topology:**
- **M1 (NMOS)** — Main amplifying transistor (common-source)
- **M2 (PMOS)** — Active load (diode-connected)
- **M3 (NMOS)** — Source degeneration transistor (diode-connected)

### Diode-Connected MOSFET
A diode-connected MOSFET has its gate tied to its drain, so V_GS = V_DS. It always operates in saturation when V_GS > V_TH, acts as a nonlinear resistor, and provides bias stability. In the small-signal model, it behaves as (1/g_m) in parallel with r_o, giving an effective resistance ≈ 1/g_m.

---

## 2. Design Specifications

<img width="1659" height="1455" alt="Screenshot 2026-03-24 200823" src="https://github.com/user-attachments/assets/fd0db54c-cb94-4c1d-9914-456689e3e435" />

| Parameter | Symbol | Value |
|-----------|--------|-------|
| Drain Current | I_D | 200 µA |
| Overdrive Voltage | V_OV | 0.2 V |
| Supply Voltage | V_DD | 1.2 V |
| NMOS Threshold | V_THn | 0.366 V |
| PMOS Threshold | \|V_THp\| | 0.39 V |
| Channel Length Modulation | λ | 0.1 V⁻¹ |
| Channel Length | L | 0.18 µm |
| NMOS Mobility | µn | 273.809 cm²/Vs |
| PMOS Mobility | µp | 115.689 cm²/Vs |
| Oxide Thickness | T_ox | 4.1 nm |
| Relative Permittivity | εr | 3.9 |

---

## 3. DC Analysis


### 3.1 Oxide Capacitance

```
Cox = (εr × ε0) / Tox
    = (3.9 × 8.854×10⁻¹²) / (4.1×10⁻⁹)
    ≈ 8.42×10⁻³ F/m²
```

### 3.2 M3 — Source Degeneration NMOS (Diode-Connected)

Since M3 is diode-connected, V_GS3 = V_DS3:

```
VGS3 = VTHn + VOV = 0.366 + 0.2 = 0.566 V
VS1  = VGS3 = 0.566 V
```

**Saturation check:** V_DS3 = V_GS3 = 0.566 V ≥ V_OV = 0.2 V ✓ → M3 in saturation

### 3.3 M1 — Common-Source NMOS

```
VGS1 = VTHn + VOV = 0.366 + 0.2 = 0.566 V
Vin  = VGS1 + VS1 = 0.566 + 0.566 = 1.132 V
```

This matches the simulation input bias V3 = 1.132 V (confirmed from operating point).

### 3.4 M2 — Active PMOS Load (Diode-Connected)

```
VSG2 = |VTHp| + VOV = 0.39 + 0.2 = 0.59 V
V3 (gate/drain of M2) = VDD − VSG2 = 1.2 − 0.59 = 0.61 V
```

Biasing voltage applied: V4 = 0.56 V (simulation value, close to calculated 0.59 V).

**Saturation check at Vout ≈ 0.678 V (simulation):**
```
VSD2 = VDD − Vout = 1.2 − 0.678 = 0.522 V ≥ VOV = 0.2 V ✓  → M2 in saturation
```

### 3.5 Operating Point (Simulation Results)

From LTspice `.op` simulation:
<img width="874" height="917" alt="Screenshot 2026-03-24 200847" src="https://github.com/user-attachments/assets/5b335565-b30b-4e18-ad5a-4660ba07be96" />


| Node/Device | Parameter | Simulated Value |
|-------------|-----------|-----------------|
| V(n001) | VDD | 1.2 V |
| V(n002) | V4 bias | 0.56 V |
| V(n003) | V2 bias | 1.132 V |
| V(n004) | VS1 (Source of M1) | 0.556 V |
| V(vin) | Input DC bias | 1.132 V |
| V(vout) | Output DC voltage | 0.6775 V |
| Id(M1) | Drain current M1 | 202.34 µA |
| Id(M2) | Drain current M2 | −202.34 µA |
| Id(M3) | Drain current M3 | 202.34 µA |

---

## 4. Device Width Calculation

### 4.1 Theoretical Width Formula

From the saturation drain current equation:

```
ID = (1/2) µ Cox (W/L) VOV²

W = (2 ID L) / (µ Cox VOV²)
```

### 4.2 NMOS Width (M1 and M3) — VOV = 0.2 V

```
Wn = (2 × 200×10⁻⁶ × 0.18×10⁻⁶) / (0.02738 × 8.42×10⁻³ × (0.2)²)

Numerator   = 7.2×10⁻¹¹
Denominator = 9.22×10⁻⁶

Wn ≈ 7.81 µm  (theoretical)
```

> Note: Theoretical calculation gives a minimum device size. In simulation, larger widths are used to set the correct bias point and current.

### 4.3 PMOS Width (M2) — VOV = 0.2 V

```
Wp = (2 × 200×10⁻⁶ × 0.18×10⁻⁶) / (0.01157 × 8.42×10⁻³ × (0.2)²)

Numerator   = 7.2×10⁻¹¹
Denominator = 3.90×10⁻⁶

Wp ≈ 18.5 µm  (theoretical)
```

### 4.4 Final Device Sizing — Simulation-Optimized (Trial & Error)

The theoretical widths were used as a starting point. Final widths were iteratively tuned in LTspice to achieve the target I_D = 200 µA, correct DC operating point, and symmetric operation.

| Transistor | Role | Theoretical W (µm) | Final W (µm) | L (µm) |
|------------|------|--------------------|--------------|--------|
| M1 (NMOS) | Common-source amplifier | 7.81 | 30 | 0.18 |
| M2 (PMOS) | Active load (diode-connected) | 18.5 | 34 | 0.18 |
| M3 (NMOS) | Source degeneration (diode-connected) | 7.81 | 34 | 0.18 |

**Key observations:**
- Larger widths (30–34 µm vs theoretical 7.8–18.5 µm) provide higher g_m and more accurate current biasing in the TSMC model.
- M2 and M3 having equal width (34 µm) improves symmetry and gain balance.
- The simulated drain current of 202.34 µA confirms close agreement with the 200 µA target.

---

## 5. Small-Signal Analysis

### 5.1 Transconductance

```
gm = 2ID / VOV = (2 × 200×10⁻⁶) / 0.2 = 2 mS
```

### 5.2 Output Resistance

```
ro = 1 / (λ ID) = 1 / (0.1 × 200×10⁻⁶) = 50 kΩ
```

### 5.3 Theoretical Voltage Gain

With source degeneration from diode-connected M3 (effective resistance ≈ 1/gm3):

```
Av = − gm1 × ro2 / (1 + gm1/gm3)
   = − (2 mS × 50 kΩ) / (1 + 1)
   = − 50 V/V

|Av| = 50 V/V  →  20 log(50) = 33.97 dB
```

---

## 6. DC Transfer Characteristic
<img width="2982" height="746" alt="Screenshot 2026-03-24 200407" src="https://github.com/user-attachments/assets/3d79393e-33bf-479d-a17d-05f77b2756e6" />


The DC sweep (V3: 0 → 2 V) shows the output voltage transfer curve. The amplifier operates in the linear region for V_in between approximately 0.9 V and 1.2 V, with the output swinging from ~1.17 V (high) down to ~0.56 V (low). The midpoint of the transition corresponds to the quiescent operating point.

---

## 7. Transient Analysis

<img width="2996" height="1615" alt="Screenshot 2026-03-24 200456" src="https://github.com/user-attachments/assets/c583f809-34db-4732-a303-bb78cbde6b8c" />

**Input:** `SINE(1.132 10m 1000)` — 1 kHz sinusoid, 10 mV amplitude, biased at 1.132 V DC

### 7.1 Simulation Cursor Readings

| Cursor | Time | V(vout) |
|--------|------|---------|
| Cursor 1 (peak) | 2.7508 ms | 767.69 mV |
| Cursor 2 (trough) | 2.2581 ms | 636.48 mV |
| Difference (Δ) | 0.4927 ms | 131.21 mV |

### 7.2 Practical Gain Calculation

```
ΔVout = 767.69 − 636.48 = 131.21 mV  (peak-to-peak)
Vin amplitude = 10 mV

Av (practical) = (131.21 mV / 2) / 10 mV ≈ 6.56 V/V
```

> The gain is lower than theoretical due to finite r_o of the diode-connected load, body effect on M3, and channel-length modulation in the 0.18 µm process.

### 7.3 Gain Comparison

| Parameter | Theoretical | Practical (Simulation) |
|-----------|-------------|------------------------|
| Av (V/V) | 50 V/V | ~6.56 V/V |
| Av (dB) | 33.97 dB | ~16.3 dB |

---

## 8. AC Analysis (Frequency Response)
<img width="2999" height="1617" alt="Screenshot 2026-03-24 200613" src="https://github.com/user-attachments/assets/d776ec5c-6209-4098-8e89-bcddb9ad607e" />

**Simulation:** `.ac dec 20 1 100G` | Input: `AC 1` (small signal)

### 8.1 Bode Plot Cursor Readings

| Cursor | Frequency | Magnitude | Phase | Group Delay |
|--------|-----------|-----------|-------|-------------|
| Cursor 1 | 818.31 MHz | 13.08 dB | 128.77° | 118.80 ps |
| Cursor 2 (midband) | 314.81 kHz | 16.01 dB | 179.98° | 215.97 ps |
| Ratio (C2/C1) | −818.00 MHz | 2.93 dB | 51.20° | 96.33 ps |

### 8.2 Key Frequency Parameters

```
Midband gain     = 16.01 dB
3 dB level       = 16.01 − 3 = 13.01 dB  →  f3dB ≈ 818 MHz
```

| Parameter | Value |
|-----------|-------|
| Midband Gain | 16.01 dB (≈ 6.32 V/V) |
| Lower Cutoff Frequency (fL) | 0 Hz (DC-coupled) |
| Upper Cutoff Frequency (fH = f3dB) | ~818 MHz |
| Bandwidth | ~818 MHz |
| Unity Gain Bandwidth (UGB) | 6.32 × 818 MHz ≈ 5.17 GHz |

---

## 9. Summary of Results

| Analysis | Parameter | Value |
|----------|-----------|-------|
| DC | Input bias (Vin) | 1.132 V |
| DC | Output bias (Vout) | 0.6775 V |
| DC | Drain current (ID) | 202.34 µA |
| DC | VS1 (source of M1) | 0.556 V |
| Small-Signal | gm (M1, M2, M3) | 2 mS |
| Small-Signal | ro | 50 kΩ |
| Gain | Theoretical \|Av\| | 50 V/V (33.97 dB) |
| Gain | Practical \|Av\| (transient) | ~6.56 V/V (~16.3 dB) |
| Gain | AC midband gain | 16.01 dB |
| Frequency | fH (3dB bandwidth) | ~818 MHz |
| Frequency | UGB | ~5.17 GHz |
| Sizing | Wn1 (M1) | 30 µm |
| Sizing | Wp (M2) | 34 µm |
| Sizing | Wn3 (M3) | 34 µm |

---

## 10. Discussion

### 10.1 Gain Discrepancy
The theoretical gain of 50 V/V assumes ideal long-channel behavior. In practice (0.18 µm), channel-length modulation (λ = 0.1 V⁻¹) significantly reduces r_o, and the diode-connected PMOS load introduces an effective load resistance of only ~1/gm2 ≈ 500 Ω rather than a high-impedance current source. The source degeneration by M3 further reduces gain by a factor of (1 + gm1/gm3) = 2.

### 10.2 Bandwidth
The circuit achieves a wide bandwidth (~818 MHz) due to the small parasitic capacitances at the output node in 0.18 µm technology. The gain-bandwidth product (UGB ≈ 5.17 GHz) is consistent with the small-signal parameters.

### 10.3 Device Sizing Rationale
The final widths (M1 = 30 µm, M2 = M3 = 34 µm) were determined by trial and error in simulation. Larger widths than the theoretical minimum are needed because the TSMC SPICE model includes short-channel effects, velocity saturation, and DIBL which reduce the effective drive current per unit width compared to the simple square-law model. Equal widths for M2 and M3 provide a good balance between gain and symmetry.

---

*End of Report — TSMC 0.18 µm CMOS Amplifier Design*
