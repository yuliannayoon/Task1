# Task1

## 📦 Deliverables
### 📄 Schematic Document

Below is the completed schematic design integrating the nRF52840 MCU, TPS62840 Buck Converter, LTC4311 I2C Bus Accelerator, and the BME680 / MS5607 sensor cluster.

> 📂 **[View Full Schematic (PDF)](./TASK1_Sch.pdf)**

---

### 📝 Design Decisions & Assumptions (147 words)

The system efficiently distributes power from a **5V input** using the high-efficiency **TPS62840 DC-DC buck converter** ($I_q = 30\text{nA}$). This replaces conventional LDOs, eliminating thermal dissipation while regulating a uniform **3.3V logic domain** for all components.

The system is centered around the **nRF52840 MCU**, selected for its **native USB 2.0 controller**, which enables direct Type-C serial UART communication for data output without external bridge chips. An onboard **SWD interface** is included for programming and debugging.

To integrate two $I^2C$ sensors sharing identical default addresses (0x76/0x77), **address collision is prevented** by hardware configuration: tying MS5607’s SDO to GND and BME680’s CSB to VCC. 

To ensure reliable **400 kHz $I^2C$ operation over 2-meter cable runs**, the **LTC4311 bus accelerator** is implemented. It actively counters high cable capacitance by delivering strong rise-time pull-up currents, maintaining sharp signal integrity across the extended bus.
