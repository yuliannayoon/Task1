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

## Power Budget and Long-term Viability Analysis
* **Target Operational Life:** 1 Year (365 Days)
* **Power Source Specification:** 3.6V 1200mAh Lithium Primary Battery
* **Theoretical Daily Energy Budget:** 3.29 mAh / day (Equivalent to a continuous 137 µA average current)
* **Practical Daily Energy Budget (20% Design Margin):** 2.63 mAh / day (Equivalent to a continuous 110 µA average current)

* ##


##  Sampling Frequency vs Battery Life 
```mermaid
xychart-beta
    title "Sampling Frequency vs Battery Life"
    x-axis ["1s", "10s", "30s", "60s", "300s"]
    y-axis "Battery Life (h)" 0 --> 80
    line [2.5, 8, 18, 30, 72]
```



## **Sensor Resolution vs Energy Cost per Sample**
```mermaid
xychart-beta
    title "Sensor Resolution vs Energy Cost per Sample"
    x-axis ["OSR 256", "OSR 512", "OSR 1024", "OSR 2048", "OSR 4096"]
    y-axis "Energy Cost (uAs)" 0 --> 13
    bar [0.756, 1.484, 2.912, 5.782, 11.508]
```
Lowering the resolution minimizes energy consumption to a near-negligible level, but it inevitably degrades the sensor's detection performance. On the other hand, maximizing the resolution pushes the energy cost up to approximately 15 times that of the OSR 256 baseline. Therefore, looking at the data, OSR 1024 can be considered the most viable option as it provides the ideal balance between energy cost and precision.


## 📦 Deliverables

## Summary of your proposed solution

🏗️ Hardware Block Diagram Comprehensive Layout
================================================================================

The overall layout is structured so that Power flows from top to bottom, 
while Data flows from left to right, ultimately transmitting wirelessly via BLE.

1. Power Management Layer (Top Layer - Red Routing)
--------------------------------------------------------------------------------
This block represents the primary power distribution path, starting from the battery 
down to the system rails. Position this at the very top of your diagram.

[ 3.6V Primary Lithium Battery ] (Usable Capacity: 85% / 1020mAh Management Margin)
       ⬇️ (Main Power Input Line)
[ TPS62840 DC-DC Buck Converter ] (Iq = 60 nA, 88% Conversion Efficiency)
       ⬇️ (Output Stage: Low-ESR Bulk Decoupling Capacitor Network)
          ※ Reservoir circuit designed to suppress the 23.84 mA peak active load.
       ⬇️ 
[ 3.3V System Power Rail ] 
       │
       ├──────────────────────────┬──────────────────────────┐
       ▼ (3.3V Power)             ▼ (3.3V Power)             ▼ (3.3V Power)
       
2. Control & Connectivity Layer (Center Layer - Main Core)
--------------------------------------------------------------------------------
Place the main microcontroller prominently in the center of the diagram as the brain of the system.

[ nRF52840 MCU ] (Configured in Normal Voltage Mode / 3.3V Operation)
   • Firmware Operational Profile: 1-Second Wake-up Cycle (5 ms Active / 995 ms Sleep)
   • Time-Weighted Daily Average Current: 51.00 µA
       ➡️ (Right Arrow: RF TX Signal) ➔ [ BLE Antenna Block ] (Tx Power: Optimized at 0 dBm)

3. Sensor Input Layer (Left Layer - Inputs)
--------------------------------------------------------------------------------
Position the sensor blocks on the left side, establishing a clear left-to-right data flow 
toward the MCU via communication buses.

[ MS5607 Barometric Pressure Sensor ]
   • Operational Profile: OSR 1024 High-Resolution / 1-Second Sampling Rate
   • Time-Weighted Daily Average Current: 2.91 µA
       ➡️ (Arrow pointing to MCU): Interconnected via I2C / SPI Bus ➔ [ nRF52840 MCU ]

[ BME680 Environmental Sensor ]
   • Operational Profile: Integrated Gas Heater / 300-Second (5-Min) Wide Sampling Rate
   • Time-Weighted Daily Average Current: 50.00 µA
       ➡️ (Arrow pointing to MCU): Interconnected via I2C / SPI Bus ➔ [ nRF52840 MCU ]

================================================================================
📈 System Power Verification Scorecard
================================================================================
   • Total Peak Current: 23.84 mA (Simultaneous Active State)
   • Total System Average Current: 103.97 µA
   • Target Power Budget Limit: 110 µA (➔ 🌟 PASS)
   • Estimated Operational Lifespan: 409 Days (1.12 Years)
================================================================================


## Summary of your proposed solution


## 📊 Integrated Sensor Analysis Master Table

