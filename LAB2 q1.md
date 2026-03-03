# Experiment 1: CS Amplifier Characterization in LTspice (180nm Technology – TSMC018)
## Aim:

To design and analyze a Common Source amplifier using 180nm CMOS technology with current source load and to determine:
## Components Required

- NMOS transistor (CMOSN – TSMC018 model)
- PMOS transistor (CMOSP – TSMC018 model)
- Source resistor (1 kΩ)
- DC Supply (1.2 V)
- Bias voltage source (0.56 V)
- AC Input Source (SINE input)
- Ground
- Technology file: `tsmc018.lib`

---

## Circuit Parameters Used

- Technology: 180 nm (TSMC018)
- VDD = 1.2 V
- Source Resistor (RS) = 1 kΩ
- Gate Bias Voltage = 0.81 V
- PMOS Bias Voltage = 0.56 V
- Input Signal = SINE(0.81 V, 10 mV, 1 kHz)
- Channel Length (L) = 180 nm

---

## Circuit Description

The implemented circuit is a Common Source amplifier with an active PMOS load.

It consists of:

- An NMOS transistor (M1) acting as the amplifying device  
- A PMOS transistor (M2) functioning as an active load  
- A source degeneration resistor (RS = 1 kΩ)  
- A 1.2 V DC supply  
- A bias voltage applied to the PMOS gate  
- An AC input signal applied at the NMOS gate  
- Output taken at the drain of the NMOS  
- Ground reference connected to the source resistor  

### Operation Principle

- The NMOS transistor (M1) operates in the saturation region to provide voltage amplification.
- The PMOS transistor (M2) serves as an active load, increasing output resistance and improving gain.
- The source resistor provides source degeneration, enhancing stability and linearity.
- The AC input signal modulates the drain current of the NMOS.
- Variations in drain current create voltage variations at the output node.
- The output signal is inverted with respect to the input (180° phase shift).

---

## Simulation Purpose

This circuit is simulated in LTspice to perform:

- DC Operating Point Analysis (.op)
- DC Sweep Analysis (.dc)
- AC Frequency Response Analysis (.ac)
- Transient Time-Domain Analysis (.tran)

The simulation validates proper biasing, confirms saturation operation, measures voltage gain, evaluates bandwidth, and verifies phase inversion.

<img width="2999" height="1617" alt="image" src="https://github.com/user-attachments/assets/b6db7fc2-a538-4a6d-ac29-5302dab9ffea" />

## **3. DESIGN CALCULATIONS**

### **3.1 GIVEN PARAMETERS**

Technology: TSMC 180nm  
Supply Voltage, VDD = 1.2 V  
Power Constraint ≤ 0.4 mW  
Channel Length, Ln(n) = 180 nm  
Threshold Voltage, VT(n) ≈ 0.36 V 
Gate Oxide Thickness, tox(n) = 4.1 × 10^-9 m  
Electron Mobility, μn(n) = 273.81 × 10^-4 m²/V·s  
Threshold Voltage, VT(p) ≈-0.39 V 
Gate Oxide Thickness, tox(9) = 4.1 × 10^-9 m  
Electron Mobility, μn(p) =  115.69 × 10^-4 m²/V·s 
Load Capacitor, CL = 0.5 pF



---

### **3.2 POWER CONSTRAINT**

The total power consumed by the circuit is given by:

P = VDD × ID  

Maximum allowed power:

P ≤ 0.4 mW  

Therefore,

ID ≤ (0.4 × 10^-3) / 1.2  

ID ≤ 333 µA  

To ensure safe operation within the limit and maintain controlled biasing, I selected:

ID = 200 µA  

### **3.3 CALCULATION OF SOURCE RESISTANCE (Rs)**

Using KVL:

Vout = VDD/2 + 0.2
ID × Rs = 0.2 

(200 × 10^-6) × RD = 0.2  

RD = 1 kΩ  

---
### **3.4 BIAS POINT SELECTION**
### **For M1(NMOS)**

For maximum symmetrical output swing:

VDS1 ≈ VDD / 2  

VDS1 = 1.2 / 2  

VDS1 = 0.6 V  

From the technology library:

VT ≈ 0.36 V  

The overdrive voltage is:
VOV  = 0.25V

VGS1 − VT1 = VOV

VGS1 = 0.25+0.36

VGS1 = .61

VG = VGS + (ID*Rs)

VG = 0.61 + 0.2

VG = .81

Now checking saturation condition:

VDS ≥ VGS - VT1 

0.6 V ≥ 0.81 - 0.36 V  

Since the condition is satisfied, the MOSFET operates in the saturation region, which is required for proper amplification.

### **For M2(PMOS)**
VDD = 1.2 

From the technology library:

VT ≈ 0.39 V  

ID = 200 uA 

The overdrive voltage is:
VOV2 = 0.25V

VSG2 -VT2 = 0.25V

VS2 - VG2 = 0.64V

VG2 = VS2 - 0.64V

VG2 = 0.56 V

Now checking saturation condition:

VDS2 ≥ VGS2 - VT2 

0.4 V ≥ 0.25 V  

## **3.5 CALCULATION OF TRANSISTOR WIDTH (W)**

The drain current equation in saturation is:

ID = (1/2) × μnCox × (W/L) × (VOV)^2 


Using calculated values:

ID = 200 × 10^-6 A  
VOV = 0.25 V  
L = 180 nm = 180 × 10^-9 m  

Substituting and solving:
## **M1 NMOS**
W ≈ 5 µm  
## **M2 PMOS**
W ≈ 69 µm  

---
# DC Analysis
<img width="1261" height="718" alt="Screenshot 2026-03-03 192700" src="https://github.com/user-attachments/assets/c19487d1-ce87-4d8f-bb55-7da2b4ff21a0" />


## Operating Point Results (.op)

From LTspice DC operating point simulation:

- VDD = 1.2 V  
- Vin (DC Bias) = 0.81 V  
- PMOS Gate Bias = 0.56 V  
- Vout = 0.800583 V  
- Source Node Voltage = 0.20038 V  
- Drain Current (Id) = 0.00020038 A  

Therefore,

ID ≈ 200 µA  

---

## Node Voltages

| Node | Voltage (V) |
|------|------------|
| VDD | 1.2 |
| Vin | 0.81 |
| PMOS Gate | 0.56 |
| Source (NMOS) | 0.20038 |
| Vout | 0.800583 |

---

## Device Currents

| Parameter | Value |
|------------|--------|
| Id(M1) | 200.38 µA |
| Id(M2) | 200.38 µA |
| I(R1) | 200.38 µA |

The drain current of NMOS and PMOS are equal in magnitude, confirming proper current mirror action and correct biasing.

---



## Final DC Design Values

- ID = 200 µA  
- Vout ≈ 0.8 V  
- VGS ≈ 0.61 V  
- VDS ≈ 0.60 V  
- Power Dissipation = 0.24 mW  

The circuit is properly biased and both transistors operate in saturation, ensuring correct amplification behavior.

## DC Sweep:
DC Sweep (Transfer characteristics )depict how the drain current (Id) varies with the gate-to-source voltage (Vgs).It's ltspice command is ".dc V2 0 2"
Common-Source NMOS Amplifier: DC Voltage Transfer Characteristic (VTC):
<img width="2999" height="1611" alt="image" src="https://github.com/user-attachments/assets/820b189c-661d-4763-8ba1-e014101c4d44" />


**Fig:** DC transfer characteristic of the Common Source amplifier showing $V_{out}$ versus $V_{in}$.

**Fig 1:** DC transfer characteristic of the CS amplifier ($V_{out}$ vs $V_{in}$).
# Transient Analysis
<img width="2999" height="1613" alt="Screenshot 2026-03-03 204749" src="https://github.com/user-attachments/assets/7139bf93-16ea-43c8-b6f3-eea107671377" />
<img width="2999" height="1614" alt="image" src="https://github.com/user-attachments/assets/83cf2d81-1000-4763-9921-cb559d565853" />



## Input Signal Parameters

- DC Offset = 0.81 V  
- Amplitude = 10 mV  
- Frequency ≈ 1 kHz  

---

## Measured Values from Waveforms

### Output Voltage (Vout)

- Vmax = 871.384 mV  
- Vmin = 719.363 mV  

ΔVout = 871.384 − 719.363  
ΔVout ≈ 152.021 mV  

---

### Input Voltage (Vin)

- Vmax = 819.979 mV  
- Vmin = 800.031 mV  

ΔVin = 819.979 − 800.031  
ΔVin ≈ 19.948 mV  

---

## Voltage Gain Calculation

Av = ΔVout / ΔVin  

Av = 152.021 / 19.948  

Av ≈ 7.62  

Since the output waveform is inverted:

Av ≈ −7.62  

---

## Gain in dB

Gain(dB) = 20 log10(|Av|)

Gain(dB) ≈ 17.64 dB  

---

## Observations

- Output signal is inverted with respect to input  
- Phase shift ≈ 180°  
- No visible distortion  
- Gain is significantly higher than simple resistive-load CS amplifier  
- PMOS active load increases output resistance and improves voltage gain  

---

# AC Analysis

## Results
<img width="2998" height="1604" alt="Screenshot 2026-03-03 205435" src="https://github.com/user-attachments/assets/8473dbb9-cd20-45f0-9938-938f8ad775c4" />


The AC frequency response of the active-load Common Source amplifier was obtained using LTspice.

---

## Midband Gain

From the Bode magnitude plot:

Midband Gain ≈ 18.099 dB  

Converting to linear gain:

Av = 10^(18.099 / 20)

Av ≈ 7.99

Since the amplifier inverts the signal:

Av ≈ −7.99

---

## Half-Power (−3 dB) Frequency

Half-power gain:

18.099 − 3 = 15.099 dB  

From the plot, gain reaches approximately 15.025 dB at:

f_H ≈ 430 MHz  

---

## Bandwidth

Since no low-frequency cutoff is observed in the simulated range:

BW ≈ f_H  

BW ≈ 430 MHz  

---

## Phase Response

- Midband phase ≈ −180°  
- Phase decreases further at high frequency due to parasitic capacitances  
- High-frequency phase shift approaches approximately −228°  

---

## Results Summary

| Parameter | Value |
|------------|--------|
| Technology | 180 nm |
| VDD | 1.2 V |
| Active Load | PMOS |
| ID | 200 µA |
| DC Output | 0.8006 V |
| Midband Gain | 18.099 dB |
| Linear Gain | 7.99 |
| Phase Shift | ~180° |
| Bandwidth | ~430 MHz |

---

## Inference

1. Proper DC bias ensures both transistors operate in saturation.
2. PMOS active load significantly increases voltage gain.
3. Gain (~8 V/V) matches transient analysis results.
4. High-frequency roll-off is due to intrinsic MOS capacitances.
5. The amplifier exhibits expected 180° phase inversion.

---

## Conclusion

The active-load Common Source amplifier implemented using 180 nm CMOS technology achieves:

- Stable DC biasing  
- High voltage gain (~8 V/V)  
- 180° phase inversion  
- Wide bandwidth (~430 MHz)  

The PMOS active load provides substantially higher gain compared to a simple resistive-load CS amplifier.


