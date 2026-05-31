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


<svg width="600" height="300" viewBox="0 0 600 300" xmlns="http://www.w3.org/2000/svg">
  <rect width="600" height="300" fill="#0d1117" rx="8"/>
  <line x1="60" y1="20" x2="60" y2="240" stroke="#30363d" stroke-width="1"/>
  <line x1="60" y1="240" x2="580" y2="240" stroke="#30363d" stroke-width="1"/>
  <line x1="60" y1="180" x2="580" y2="180" stroke="#21262d" stroke-width="1" stroke-dasharray="3,3"/>
  <line x1="60" y1="120" x2="580" y2="120" stroke="#21262d" stroke-width="1" stroke-dasharray="3,3"/>
  <line x1="60" y1="60" x2="580" y2="60" stroke="#21262d" stroke-width="1" stroke-dasharray="3,3"/>
  <text x="50" y="244" fill="#8b949e" font-size="10" text-anchor="end" font-family="monospace">0h</text>
  <text x="50" y="184" fill="#8b949e" font-size="10" text-anchor="end" font-family="monospace">24h</text>
  <text x="50" y="124" fill="#8b949e" font-size="10" text-anchor="end" font-family="monospace">48h</text>
  <text x="50" y="64" fill="#8b949e" font-size="10" text-anchor="end" font-family="monospace">72h</text>
  <!-- x positions: 1s=110, 10s=215, 30s=320, 60s=425, 300s=530 -->
  <!-- y: 0h=240, 72h=20 → 1h=3.056px | 2.5h→232, 8h→216, 18h→185, 30h→149, 72h→20 -->
  <polyline points="110,232 215,216 320,185 425,149 530,20" fill="none" stroke="#58a6ff" stroke-width="2" stroke-linejoin="round" stroke-linecap="round"/>
  <polygon points="110,232 215,216 320,185 425,149 530,20 530,240 110,240" fill="#58a6ff" fill-opacity="0.08"/>
  <circle cx="110" cy="232" r="4" fill="#0d1117" stroke="#58a6ff" stroke-width="2"/>
  <circle cx="215" cy="216" r="4" fill="#0d1117" stroke="#58a6ff" stroke-width="2"/>
  <circle cx="320" cy="185" r="4" fill="#0d1117" stroke="#58a6ff" stroke-width="2"/>
  <circle cx="425" cy="149" r="4" fill="#0d1117" stroke="#5

