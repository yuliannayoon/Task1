# Task1

## 📦 Deliverables
### 📄 Schematic Document

Below is the completed schematic design integrating the nRF52840 MCU, TPS62840 Buck Converter, LTC4311 I2C Bus Accelerator, and the BME680 / MS5607 sensor cluster.

> 📂 **[View Full Schematic (PDF)](./TASK1_Sch.pdf)**

---

### 📝 Design Decisions & Assumptions (147 words)

The proposed system consists of three primary functional blocks Power Management, Sensor Detection, and the Microprocessor. 
The system is centered around the nRF52840 chipset, configured in Normal Voltage Mode to supply a uniform 3.3V operating voltage across all onboard sensors. It also leverages the chip's internal USB-to-Serial capability, using the physical PHY circuit to automatically detect PC connections.To maximize efficiency, the power distribution utilizes a high-efficiency DC-DC buck converter with an ultra-low quiescent current ($I_q$) of 30nA. This replaces conventional LDO regulators, eliminating excessive thermal dissipation caused by voltage differentials and output current.The sensor detection block integrates two sensors that share identical default $I^2C$ address options (0x76 and 0x77). To prevent address collision on the same bus, the hardware was configured to allocate unique addresses by tying the SDO pin of the MS5607 to Low (GND) and the CSB pin of the BME680 to High (VCC).To support 400kHz high-speed $I^2C$ communication over a 2-meter cable, an $I^2C$ bus accelerator (rise-time accelerator) was implemented. This actively counters signal distortion caused by increased cable capacitance and guarantees sharp rise times, ensuring robust signal integrity. 

# Task2

## **Power Budget and Long-term Viability Analysis**
* **Target Operational Life:** 1 Year (365 Days)
* **Power Source Specification:** 3.6V 1200mAh Lithium Primary Battery
* **Theoretical Daily Energy Budget:** 3.29 mAh / day (Equivalent to a continuous 137 µA average current)
* **Practical Daily Energy Budget (20% Design Margin):** 2.63 mAh / day (Equivalent to a continuous 110 µA average current)


## 📦 Deliverables
## 📊 Integrated Sensor Analysis Master Table

* **Power Source:** 3.6V 1200mAh Lithium Primary Battery
* **MS5607 Configuration:** OSR 4096 (Highest Resolution: $I_{active} = 1.4\text{ mA}$, $T_{active} = 8.22\text{ ms}$)
* **BME680 Configuration:** Default Mode with Gas Sensor Active ($I_{active} = 13\text{ mA}$, $T_{active} = 350\text{ ms}$)
* **Base System Sleep Current:** $50\,\mu\text{A}$ (Includes MCU Stop Mode baseline)

| Sampling Frequency | Sensor Model | ① Energy Usage (Average Current) | ② Measurement Approach & State | ③ Long-term Viability (Expected Lifespan) |
| :---: | :---: | :---: | :--- | :--- |
| **1 sec**<br>**(1 Hz)** | **MS5607** | **$61.1\,\mu\text{A}$**<br>(Low) | **Continuous Tracking**<br>• Triggers once per second.<br>• Short conversion time ($8.22\text{ ms}$). | **Approx. 2.2 Years (818 Days)**<br>• Highly viable for real-time tracking (drones/navigation). |
| | **BME680** | **$4,583.5\,\mu\text{A}$**<br>($4.58\text{ mA}$ - Critical) | **Heavy Continuous Heating**<br>• Gas heater stays on frequently.<br>• Extremely long active time ($350\text{ ms}$). | **Approx. 10.9 Days**<br>• **Not viable** for long-term battery deployment at this rate. |
| **30 sec** | **MS5607** | **$50.4\,\mu\text{A}$**<br>(Very Low) | **Intermittent LP Mode**<br>• Sleep state for 29.99 seconds.<br>• Duty Cycle: $0.027\%$ | **Approx. 2.7 Years (992 Days)**<br>• Optimized for smart wearables and ambient tracking. |
| | **BME680** | **$195.8\,\mu\text{A}$**<br>(Moderate) | **Pulsed Heating LP Mode**<br>• Heater budget is split into 30s intervals.<br>• Duty Cycle: $1.17\%$ | **Approx. 255.3 Days (0.7 Year)**<br>• Moderate viability; acceptable for short-term field tests. |
| **300 sec**<br>**(5 min)** | **MS5607** | **$50.0\,\mu\text{A}$**<br>(Minimum Baseline) | **Ultra Low Power (ULP)**<br>• 99.99% of time spent in deep sleep.<br>• Converges to hardware baseline current. | **Approx. 2.7 Years (1,000 Days)**<br>• **Physical Limit Reached:** Dominated by battery self-discharge (1–2%/year). |
| | **BME680** | **$64.6\,\mu\text{A}$**<br>(Low) | **Ultra Low Power (ULP)**<br>• Heater triggers only once every 5 mins.<br>• Maximizes sleep efficiency. | **Approx. 2.1 Years (773 Days)**<br>• **Highly viable** for long-term air quality monitoring stations. |



Sampling Frequency vs Battery Life

Battery life increases significantly as the sampling interval grows, due to reduced wake-up cycles and lower average current draw.

<p align="center">
<svg width="680" height="400" viewBox="0 0 680 400" xmlns="http://www.w3.org/2000/svg" font-family="'Courier New', monospace">
  <!-- Background -->
  <rect width="680" height="400" fill="#0d1117" rx="12"/>
  <!-- Grid lines -->
  <line x1="80" y1="40" x2="80" y2="320" stroke="#21262d" stroke-width="1"/>
  <line x1="80" y1="320" x2="640" y2="320" stroke="#21262d" stroke-width="1"/>
  <!-- Horizontal grid -->
  <line x1="80" y1="250" x2="640" y2="250" stroke="#21262d" stroke-width="1" stroke-dasharray="4,4"/>
  <line x1="80" y1="180" x2="640" y2="180" stroke="#21262d" stroke-width="1" stroke-dasharray="4,4"/>
  <line x1="80" y1="110" x2="640" y2="110" stroke="#21262d" stroke-width="1" stroke-dasharray="4,4"/>
  <line x1="80" y1="40" x2="640" y2="40" stroke="#21262d" stroke-width="1" stroke-dasharray="4,4"/>
  <!-- Y-axis labels -->
<text x="70" y="324" fill="#8b949e" font-size="11" text-anchor="end">0h</text>
<text x="70" y="254" fill="#8b949e" font-size="11" text-anchor="end">24h</text>
<text x="70" y="184" fill="#8b949e" font-size="11" text-anchor="end">48h</text>
<text x="70" y="114" fill="#8b949e" font-size="11" text-anchor="end">72h</text>
<text x="70" y="44" fill="#8b949e" font-size="11" text-anchor="end">96h</text>
  <!-- X axis points: 1s→130, 10s→242, 30s→354, 60s→466, 300s→578 -->
  <!-- Y values (hours): 2.5→315, 8→299, 18→278, 30→252, 72→180 -->
  <!-- Mapping: 0h=320, 96h=40 → 1h = (320-40)/96 = 2.916px -->
  <!-- Data points Y calculation:
    2.5h  → 320 - 2.5*2.916  = 312.7 ≈ 313
    8h    → 320 - 8*2.916    = 296.7 ≈ 297
    18h   → 320 - 18*2.916   = 267.5 ≈ 268
    30h   → 320 - 30*2.916   = 232.5 ≈ 233
    72h   → 320 - 72*2.916   = 110.0 ≈ 110
  -->
  <!-- Gradient fill under curve -->
  <defs>
    <linearGradient id="areaGrad" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0%" stop-color="#39d353" stop-opacity="0.25"/>
      <stop offset="100%" stop-color="#39d353" stop-opacity="0.02"/>
    </linearGradient>
  </defs>
  <!-- Area fill -->
<polygon
 points="130,313 242,297 354,268 466,233 578,110 578,320 130,320"
 fill="url(#areaGrad)"
/>
  <!-- Line -->
<polyline
 points="130,313 242,297 354,268 466,233 578,110"
 fill="none"
 stroke="#39d353"
 stroke-width="2.5"
 stroke-linejoin="round"
 stroke-linecap="round"
/>
  <!-- Data point dots -->
  <circle cx="130" cy="313" r="5" fill="#0d1117" stroke="#39d353" stroke-width="2"/>
  <circle cx="242" cy="297" r="5" fill="#0d1117" stroke="#39d353" stroke-width="2"/>
  <circle cx="354" cy="268" r="5" fill="#0d1117" stroke="#39d353" stroke-width="2"/>
  <circle cx="466" cy="233" r="5" fill="#0d1117" stroke="#39d353" stroke-width="2"/>
  <circle cx="578" cy="110" r="5" fill="#0d1117" stroke="#39d353" stroke-width="2"/>
  <!-- Value labels above dots -->
<text x="130" y="302" fill="#39d353" font-size="10" text-anchor="middle">2.5h</text>
<text x="242" y="286" fill="#39d353" font-size="10" text-anchor="middle">8h</text>
<text x="354" y="257" fill="#39d353" font-size="10" text-anchor="middle">18h</text>
<text x="466" y="222" fill="#39d353" font-size="10" text-anchor="middle">30h</text>
<text x="578" y="99"  fill="#39d353" font-size="10" text-anchor="middle">72h</text>
  <!-- X-axis labels -->
<text x="130" y="340" fill="#8b949e" font-size="11" text-anchor="middle">1 s</text>
<text x="242" y="340" fill="#8b949e" font-size="11" text-anchor="middle">10 s</text>
<text x="354" y="340" fill="#8b949e" font-size="11" text-anchor="middle">30 s</text>
<text x="466" y="340" fill="#8b949e" font-size="11" text-anchor="middle">60 s</text>
<text x="578" y="340" fill="#8b949e" font-size="11" text-anchor="middle">300 s</text>
  <!-- Axis titles -->
<text x="360" y="370" fill="#8b949e" font-size="12" text-anchor="middle">Sampling Interval (seconds)</text>
<text x="20" y="200" fill="#8b949e" font-size="12" text-anchor="middle" transform="rotate(-90,20,200)">Battery Life (hours)</text>
  <!-- Chart title -->
<text x="360" y="22" fill="#f0f6fc" font-size="14" text-anchor="middle" font-weight="bold">Sampling Frequency vs Battery Life</text>
</svg>
</p>
Sampling IntervalBattery Life (est.)1 s~2.5 h10 s~8 h30 s~18 h60 s~30 h300 s~72 h

Note: Values are illustrative. Actual battery life depends on MCU sleep current, radio duty cycle, sensor draw, and battery capacity.

