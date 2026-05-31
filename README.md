# Task1
## 📄 Schematic Document

Below is the completed schematic design integrating the nRF52840 MCU, TPS62840 Buck Converter, LTC4311 I2C Bus Accelerator, and the BME680 / MS5607 sensor cluster.
You can view the full schematic here: **[[TASK1_Sch.pdf](./TASK1_Sch.pdf)]**


### 📝 Design Decisions & Assumptions (137 words)

The proposed system consists of three primary functional blocks: Power Management, Sensor Detection, and the Microprocessor. The system is centered around the nRF52840 chipset, configured in Normal Voltage Mode to supply a uniform 3.3V operating voltage across all onboard sensors. It also leverages the chip's internal USB-to-Serial capability, using the physical PHY circuit to automatically detect PC connections.
To maximize efficiency, the power distribution utilizes a high-efficiency DC-DC buck converter with an ultra-low quiescent current ($I_q$) of 30nA. This replaces conventional LDO regulators, eliminating excessive thermal dissipation caused by voltage differentials and output current.
The sensor detection block integrates two sensors that share identical default $I^2C$ address options (0x76 and 0x77). To prevent address collision on the same bus, the hardware was configured to allocate unique addresses by tying the SDO pin of the MS5607 to Low (GND) and the CSB pin of the BME680 to High (VCC).Signal Integrity Optimization: To support 400kHz high-speed $I^2C$ communication over a 2-meter cable, an $I^2C$ bus accelerator (rise-time accelerator) was implemented. This actively counters signal distortion caused by increased cable capacitance and guarantees sharp rise times, ensuring robust signal integrity.


# Task1
## 📄 Schematic Document

Below is the completed schematic design integrating the nRF52840 MCU, TPS62840 Buck Converter, LTC4311 I2C Bus Accelerator, and the BME680 / MS5607 sensor cluster.

> 📂 **[View Full Schematic (PDF)](./TASK1_Sch.pdf)**

---

### 📝 Design Decisions & Assumptions (147 words)

The system efficiently distributes power from a **5V input** using the high-efficiency **TPS62840 DC-DC buck converter** ($I_q = 30\text{nA}$). This replaces conventional LDOs, eliminating thermal dissipation while regulating a uniform **3.3V logic domain** for all components.

The system is centered around the **nRF52840 MCU**, selected for its **native USB 2.0 controller**, which enables direct Type-C serial UART communication for data output without external bridge chips. An onboard **SWD interface** is included for programming and debugging.

To integrate two $I^2C$ sensors sharing identical default addresses (0x76/0x77), **address collision is prevented** by hardware configuration: tying MS5607’s SDO to GND and BME680’s CSB to VCC. 

To ensure reliable **400 kHz $I^2C$ operation over 2-meter cable runs**, the **LTC4311 bus accelerator** is implemented. It actively counters high cable capacitance by delivering strong rise-time pull-up currents, maintaining sharp signal integrity across the extended bus.
