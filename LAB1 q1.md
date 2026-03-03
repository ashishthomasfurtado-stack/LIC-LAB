
# Experiment 1: CS Amplifier Characterization in LTspice (180nm Technology – TSMC018)

## Aim
To simulate a Common Source (CS) amplifier using 180nm CMOS technology in LTspice and analyze DC bias, AC gain, bandwidth, and transient response.

---

## Components Required
- NMOS (CMOSN – TSMC018 model)
- Resistor (3kΩ)
- Capacitor (1fF)
- DC Supply (1.2V)
- AC Input Source
- Ground
- Technology file: tsmc018.lib

---

## Theory

A Common Source (CS) amplifier is a basic MOSFET configuration used for voltage amplification.

- Input is applied to the **Gate**
- Output is taken from the **Drain**
- Source is connected to **Ground**
- Drain resistor develops output voltage
- Small AC signal modulates drain current producing amplified output


Voltage gain:

Av = −gm * RD

---
Equation of drain current for saturation region of operation of MOSFET 

ID = (1/2) * μn * Cox * (W/L) * (VGS - VTH)^2
.
## Procedure 

Step 1: Open LTspice and create a New schematic Open LTspice, navigate to file to New Schematic .Save the schematic with an appropriate name.

Step 2:  Place the following components in the schematic-MOSFET (CMOSN or NMOS 180nm technology model)Resistor (R1 = 1kΩ)Voltage sources (V1 for DC supply, V2 for AC input signal)Ground connection (every circuit must have a ground reference).

Step 3: Set Component values DC supply (V1) = 1.2V (connect to drain through R1).AC input source V2 = SINE(0.9 10m 1k) (0.9V DC bias, 10mV AC signal, 1kHz frequency).Resistor (R1) = 3kΩ (between V1 and drain). MOSFET (M1) = 180nm NMOS model (drain connected to R1, source connected to ground, gate connected to V2).
Import MOSFET s(CMOS) datsheet which contains threshold and other specifications of mosfet 

Step 4: Configure simulation click on simulation then edit Simulation Cmd and enter the following:
 
DC Analysis:  .op to find DC operating point.
AC Analysis: .ac dec 20 0.1 1T to analyze gain and frequency response.
Transient Analysis: .tran 5m to observe the output waveform over time. Place these commands in the schematic.

Step 5: Run SimulationClick Run (green 'Play' button).View results in waveform viewer. Observing DC bias values, gain vs frequency response, and time-domain output waveform.

Step 6: Interpret The results for DC analysis, check VGS, VDS, and ID to confirm the MOSFET is operating. For AC analysis, check the gain plot and bandwidth.Respectively for transient analysis, observe the shape of the output waveform and check that amplification took place.

## Circuit Parameters Used

- Technology: 180nm
- VDD = 1.2V
- RD = 3kΩ
- Input = SINE(0.9V, 10mV, 1kHz)
- Channel Length (L) = 180nm

---
## **CIRCUIT DESCRIPTION**

The basic Common Source amplifier used in this experiment consists of:

- NMOS transistor from TSMC 180nm technology library  
- Drain resistor (RD)  
- Supply voltage (VDD = 1.2V)  
- Input signal applied at the gate  
- Output taken from the drain  
- Source terminal connected to ground  

This circuit structure is implemented in LTSpice to perform:

- DC operating point analysis  
- Transient response analysis  
- AC frequency response analysis  
 <img width="2999" height="1584" alt="Screenshot 2026-02-24 125936" src="https://github.com/user-attachments/assets/37b8fe8e-56d3-4932-b218-117a7b3658f5" />





## **3. DESIGN CALCULATIONS**

### **3.1 GIVEN PARAMETERS**

Technology: TSMC 180nm  
Supply Voltage, VDD = 1.2 V  
Power Constraint ≤ 0.4 mW  
Channel Length, Ln = 360 nm  
Threshold Voltage, VT ≈ 0.36 V  
Electron Mobility, μn = 273.81 × 10^-4 m²/V·s  
Load Capacitor, CL = 0.5 pF  
Gate Oxide Thickness, tox = 4.1 × 10^-9 m  

---

## **3.2 POWER CONSTRAINT**

The total power consumed by the circuit is given by:

P = VDD × ID  

Maximum allowed power:

P ≤ 0.4 mW  

Therefore,

ID ≤ (0.4 × 10^-3) / 1.2  

ID ≤ 333 µA  

To ensure safe operation within the limit and maintain controlled biasing, I selected:

ID = 200 µA  

Power dissipated:

P = 1.2 × 200 µA  
P = 0.24 mW  

Since 0.24 mW < 0.4 mW, the power constraint is satisfied.

---

## **3.3 BIAS POINT SELECTION**

For maximum symmetrical output swing:

VDS ≈ VDD / 2  

VDS = 1.2 / 2  
VDS = 0.6 V  

Since the source terminal is grounded:

VGS = VG − VS  

Assuming gate bias:

VGS = 0.9 − 0  
VGS = 0.9 V  

From the technology library:

VT ≈ 0.36 V  

The overdrive voltage is:

VOV = VGS − VT  

VOV = 0.9 − 0.36  
VOV = 0.54 V  

Now checking saturation condition:

VDS ≥ VOV  

0.6 V ≥ 0.54 V  

Since the condition is satisfied, the MOSFET operates in the saturation region, which is required for proper amplification.

---

## **3.4 CALCULATION OF DRAIN RESISTANCE (RD)**

Using KVL:

VDD − ID × RD = VDS  

1.2 − (200 × 10^-6) × RD = 0.6  

200 × 10^-6 × RD = 0.6  

RD = 0.6 / (200 × 10^-6)  

RD = 3 kΩ  

---

## **3.5 CALCULATION OF TRANSISTOR WIDTH (W)**

The drain current equation in saturation is:

ID = (1/2) × μnCox × (W/L) × (VOV)^2  

Using calculated values:

ID = 200 × 10^-6 A  
VOV = 0.54 V  
L = 180 nm = 180 × 10^-9 m  

Substituting and solving:

W ≈ 1.07 µm  

---



# DC Analysis

<img width="1273" height="934" alt="Screenshot 2026-02-24 114557" src="https://github.com/user-attachments/assets/22ee17d2-57fb-4b3b-8eb6-d8fee2de18b1" />






### Operating Point Results
| Length (nm) | Width (µm) | Id (µA) |
|-------------|------------|---------|
| 180        | 1.07       | 140    |
| 180        | 1.63      | 200.4   |

- VDD = 1.2 V  
- Vin (DC Offset) = 0.9 V  
- Vout = 0.5987 V  
- ID = 0.000200427 A  

ID ≈ 200.4 µA

### Region of Operation

Since:

VDS > (VGS − VTH)

The MOSFET operates in the **Saturation Region**.

---

## **FINAL DESIGN VALUES**

ID = 200 µA  
RD = 3 kΩ  
Calculated Width (W) ≈ 1.63 µm  
VDS = 0.6 V  
Power Dissipation = 0.24 mW  

All design constraints are satisfied, and the device operates in saturation, ensuring proper voltage amplification.
## DC Sweep:
DC Sweep (Transfer characteristics )depict how the drain current (Id) varies with the gate-to-source voltage (Vgs).It's ltspice command is ".dc V2 0 2"
Common-Source NMOS Amplifier: DC Voltage Transfer Characteristic (VTC):

<img width="2975" height="1560" alt="Screenshot 2026-02-24 114651" src="https://github.com/user-attachments/assets/3290d1d9-dc21-4f3a-9f78-522a1579d35f" />





**Fig:** DC transfer characteristic of the Common Source amplifier showing $V_{out}$ versus $V_{in}$.

**Fig 1:** DC transfer characteristic of the CS amplifier ($V_{out}$ vs $V_{in}$).
# Transient Analysis
<img width="2997" height="1611" alt="Screenshot 2026-02-24 115144" src="https://github.com/user-attachments/assets/276112af-f297-4168-a7d9-32646b08a2fa" />
<img width="584" height="379" alt="Screenshot 2026-02-24 115316" src="https://github.com/user-attachments/assets/95f2bc9b-3cef-4510-bbca-c398d51d7efa" />
<img width="582" height="377" alt="Screenshot 2026-02-24 115229" src="https://github.com/user-attachments/assets/4bbb85e2-4a4e-48bc-a321-c70fffca4a92" />





- DC Offset = 0.9V  
- Amplitude = 10mV  
- Frequency = 1kHz

- * From the graph we can calculate the gain of the circuit.

* **Av​**=ΔVin/​ΔVout
  
   Av=(617.21-580.39)/(909.99-890)=1.84
* **Gain(dB)** = 20log10(Av)=5.296dB​​​
* **gm= 2Id / Vov=7.4*10^-4 S**
* **Av=gm×RD**

   Av=7.4*10^-4 ×3×10^3=2.22
* **Gain(dB)** = 20log10(Av)=6.92

### Observations


- Output is inverted  
- Phase shift ≈ 180°  
- No distortion observed  

---



# AC Analysis


### Results

<img width="2999" height="1608" alt="Screenshot 2026-02-24 114839" src="https://github.com/user-attachments/assets/97c4c4c7-06fa-43d1-862e-89287ec999d4" />




### Midband Gain

<img width="2997" height="1607" alt="image" src="https://github.com/user-attachments/assets/1f036a09-a810-4af3-a498-3008f4176a25" />

$$
A_v \approx 5.315\,dB
$$

### Half-Power (-3 dB) Frequency

$$
5.315- 3 = 2.315\,dB
$$

From the graph, gain reaches 2.315 dB at approximately:

$$
f_H \approx 33.87\,GHz
$$

### Bandwidth

Since no low-frequency cutoff is observed:

$$
BW = f_H
$$

$$
BW \approx 33.87\,GHz
$$

Final Gain:

Av ≈ −1.8439

(negative sign indicates phase inversion)

---
## Results Summary

| Parameter | Value |
|------------|--------|
| Technology | 180nm |
| VDD | 1.2 V |
| RD | 3kΩ |
| ID | 200 µA |
| DC Output | 0.5987 V |
| Midband Gain | 5.315 dB |
| Linear Gain | 1.8439 |
| Phase Shift | ~180° |
| Bandwidth | 33.87GHz Range |

---

## Inference

1. Proper DC bias ensures saturation operation.
2. Gain depends on gm and RD.
3. 180nm technology provides high-frequency response.
4. CS amplifier produces 180° phase inversion.
5. Simulation results match theoretical expectations.

---

##Conclusion

The Common Source amplifier designed using 180nm CMOS technology operates in saturation and provides stable DC bias, moderate voltage gain (~1.84), 180° phase inversion, and high-frequency bandwidth.






