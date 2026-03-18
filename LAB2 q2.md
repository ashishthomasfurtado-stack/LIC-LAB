# Experiment 2  
# DC, AC and Transient Analysis of MOSFET Amplifiers (180 nm CMOS)

## Circuit 2 – Cascode Common Source Amplifier with PMOS Active Load

---

# Aim

To design and simulate a **Cascode Common Source MOSFET amplifier using 180 nm CMOS technology in LTspice**, perform **DC, transient, and AC analyses**, and evaluate its gain, bandwidth, and operating characteristics.

---

# Circuit Schematic

<img width="2999" height="1626" alt="Screenshot 2026-03-04 163556" src="https://github.com/user-attachments/assets/1543e023-1620-4c64-987a-362de8fe2e65" />

The circuit represents a **Cascode Common Source amplifier implemented using CMOS technology**.

### Transistor Roles

| Transistor | Type | Function |
|-------------|------|----------|
| M1 | NMOS | Input transistor (Common Source stage) |
| M3 | NMOS | Cascode transistor |
| M2 | PMOS | Active load current source |

Supply Voltage:

VDD = **1.2 V**

---

# Working Principle

## Cascode Structure (M1–M3)

The cascode amplifier consists of two stacked NMOS transistors.

### M1 – Common Source Transistor

- Receives the input signal  
- Converts input voltage into drain current  

### M3 – Cascode Transistor

- Maintains nearly constant **VDS across M1**  
- Reduces **Miller capacitance**  
- Increases **output resistance**  

### PMOS Active Load (M2)

The PMOS transistor acts as a **current source load**, providing high output resistance.

### Advantages

- Higher gain  
- Higher output resistance  
- Reduced Miller effect  
- Improved high-frequency performance  

---

# DC Operating Point

The operating point obtained from LTspice simulation is summarized below.

<img width="1261" height="718" alt="Screenshot 2026-03-03 192700" src="https://github.com/user-attachments/assets/3bcf6877-0f9a-4b37-93f5-fe74eacdcbe3" />

### Node Voltages

| Node | Voltage |
|------|--------|
| VDD | 1.2 V |
| Bias Voltage (M3 Gate) | 0.61 V |
| PMOS Gate Bias | 0.56 V |
| Vin | 0.91 V |
| Vout | 0.9029 V |

---

# Drain Current Verification

From LTspice operating point:

```
Id ≈ 0.000201255 A
```

Therefore:

```
Id ≈ 201 µA
```

This matches the design specification:

```
ID ≈ 200 µA
```

---

# Bias Verification

## NMOS M1

Condition for saturation:

```
VDS ≥ VOV
```

From simulation:

```
VDS ≈ Vout − Vs
VDS ≈ 0.9029 − 0.275
VDS ≈ 0.627 V
```

Assume:

```
VOV = 0.2 V
```

Since

```
0.627 > 0.2
```

M1 operates in **saturation region**.

---

## NMOS M3 (Cascode)

Condition:

```
VDS ≥ VOV
```

The cascode transistor maintains constant voltage across M1 and therefore operates in **saturation region**.

---

## PMOS M2

Condition:

```
VSD ≥ VOV
```

From simulation:

```
VSD = VDD − Vout
VSD = 1.2 − 0.9029
VSD = 0.297 V
```

Since

```
0.297 > 0.2
```

PMOS transistor operates in **saturation region**.

---

# DC Sweep Analysis
<img width="2999" height="1611" alt="Screenshot 2026-03-03 204511" src="https://github.com/user-attachments/assets/7e9e5718-e265-4646-9948-1b49851994ed" />

The DC sweep plot shows the **transfer characteristics of the amplifier**.

### Observations

- At low input voltage the NMOS transistor remains OFF  
- When input voltage exceeds threshold voltage the transistor turns ON  
- Output voltage decreases as current increases  
- The linear region of the curve represents the **amplification region**

---

# Transient Analysis

The transient simulation verifies amplifier behavior using a sinusoidal input signal.

### Input Signal

```
Vin = SINE(0.91 10m 1k)
```

| Parameter | Value |
|----------|------|
| DC Bias | 0.91 V |
| Amplitude | 10 mV |
| Frequency | 1 kHz |

---

# Transient Waveforms

<img width="2999" height="1613" alt="Screenshot 2026-03-03 204749" src="https://github.com/user-attachments/assets/02308fe4-99ea-41ff-93cf-47c80239439e" />

<img width="2999" height="1614" alt="Screenshot 2026-03-03 204935" src="https://github.com/user-attachments/assets/d6adfa2b-c624-40c9-99ff-83cc143e9634" />

### Observations

- Output waveform is **inverted**
- Output amplitude is **larger than input amplitude**
- Confirms **Common Source amplification**

---

# Practical Gain Calculation

### Output Voltage

```
Vout(max) = 914.21 mV
Vout(min) = 891.51 mV
```

```
ΔVout = 914.21 − 891.51
ΔVout = 22.70 mV
```

---

### Input Voltage

```
Vin(max) = 919.97 mV
Vin(min) = 900.00 mV
```

```
ΔVin = 919.97 − 900.00
ΔVin = 19.97 mV
```

---

### Voltage Gain

```
Av = ΔVout / ΔVin
Av = 22.70 / 19.97
Av ≈ 1.136 V/V
```

---

### Gain in dB

```
Av(dB) = 20 log(Av)
Av(dB) ≈ 1.11 dB
```

---

# AC Analysis

<img width="2998" height="1604" alt="Screenshot 2026-03-03 205435" src="https://github.com/user-attachments/assets/cc294e12-7190-41b1-9827-e2aa9e6c2435" />


AC analysis is used to determine:

- Midband gain  
- Cutoff frequencies  
- Bandwidth  
- Unity Gain Bandwidth  

---

# Midband Gain

From AC plot:

```
Midband Gain ≈ 1.139 dB
```

Convert to linear:

```
Av(mid) = 10^(1.139 / 20)
Av(mid) ≈ 1.136 V/V
```

---

# Bandwidth Calculation

3 dB bandwidth level:

```
1.139 − 3 = −1.861 dB
```

From AC plot:

```
Upper cutoff frequency fH ≈ 673.17 MHz
Lower cutoff frequency fL ≈ 0 Hz
```

Therefore:

```
Bandwidth ≈ 673 MHz
```

---

# Unity Gain Bandwidth (UGB)

From AC plot:

```
UGB ≈ 316.2 MHz
```

---

# Summary of Circuit Performance

| Parameter | Value |
|----------|------|
| Supply Voltage | 1.2 V |
| Drain Current | 200 µA |
| Midband Gain | 1.136 V/V |
| Gain (dB) | 1.139 dB |
| Upper Cutoff Frequency | 673 MHz |
| Bandwidth | 673 MHz |
| Unity Gain Bandwidth | 316 MHz |

---

# Discussion

Observations from the simulation:

1. The cascode structure significantly increases **output resistance**.  
2. Limited voltage headroom exists due to **1.2 V supply**.  
3. This reduces achievable voltage gain.  
4. The amplifier demonstrates **good high-frequency performance**.  
5. The cascode transistor suppresses **Miller capacitance**, improving bandwidth.

---

# Applications

Cascode amplifiers are widely used in:

- RF amplifiers  
- Low Noise Amplifiers (LNA)  
- Operational amplifier gain stages  
- Analog front-end circuits  
- High-frequency CMOS amplifiers  

---

# Result

1. A **Cascode Common Source amplifier** was designed using **180 nm CMOS technology**.  
2. The circuit was simulated using **LTspice**.  
3. **DC, transient, and AC analyses** were successfully performed.  
4. The amplifier achieved a practical gain of **1.136 V/V (1.139 dB)**.  
5. The circuit demonstrated a bandwidth of approximately **673 MHz**.  
6. Unity Gain Bandwidth obtained from AC analysis is **316 MHz**.

---
