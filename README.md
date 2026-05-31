# Task1
## 📄 Schematic Document

Below is the completed schematic design integrating the nRF52840 MCU, TPS62840 Buck Converter, LTC4311 I2C Bus Accelerator, and the BME680 / MS5607 sensor cluster.


### 📝 Design Decisions & Assumptions (137 words)

This schematic presents an engineering sample driven by a 5V USB Type-C input. To prevent heat and efficiency loss during 5V to 3.3V conversion, the high-efficiency TPS62840 buck converter was selected. The nRF52840 MCU operates in Normal Voltage Mode to isolate power noise, routing data via USB CDC. For the I2C bus, the BME680 and MS5607 sensors are hardware-configured to addresses 0x76 and 0x77 to avoid collision. To overcome the high rise-time delay caused by the 2-meter cable, the LTC4311 active accelerator was introduced, ensuring signal integrity at 400 kHz. Finally, a 6-pin connector with a GND shield wire between SCL/SDA, along with ESD122 and TVS diodes, minimizes cross-talk and provides robust ESD protection.
