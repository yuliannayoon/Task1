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





## Summary of your proposed solution


## 📊 Integrated Sensor Analysis Master Table
## 📦 Deliverables

## Summary of your proposed solution
본 솔루션은 ultra-low $I_q$ Buck Converter 기반의 전원단 설계와 독립적인 $I^2C$ 주소 할당을 통해 시스템 안정성을 극대화했습니다. 특히 2m 장거리 통신에서 발생하는 선로 커패시턴스 문제를 활성 상승 시간 가속기(LTC4311)로 극복하여 신호 무결성(Signal Integrity)을 확보했습니다. 배터리 수명 1년 만족을 위해 센서 동작 조건은 가장 효율적인 밸런스를 지닌 **Sampling Interval 300s 및 OSR 1024** 조합을 제안합니다.


## 📊 Integrated Sensor Analysis Master Table

아래 마스터 테이블은 시스템의 전원 제약 조건, 센서의 하드웨어 설정, 그리고 배터리 수명 최적화를 위한 핵심 파라미터를 통합한 분석 결과입니다.

| 분류 (Category) | 항목 (Item) | 명세 및 설정값 (Specification & Configuration) | 비고 (Remarks) |
| :--- | :--- | :--- | :--- |
| **전원 제어**<br>*(Power)* | 메인 공급 전압 ($V_{CC}$)<br>배터리 사양<br>목표 수명 기준 | **3.3V** (Normal Voltage Mode)<br>3.6V, 1200mAh Lithium Primary Battery<br>**연간 권장 연속 전류 소비량 $\le$ 110 µA** | nRF52840 MCU 내부 LDO/DC-DC 연계<br>20% 설계 마진 반영 결과 |
| **하드웨어 주소**<br>*(I2C Addressing)* | **BME680** 주소 설정<br>**MS5607** 주소 설정<br>버스 가속기 | **0x77** (CSB 핀 $\rightarrow$ VCC 결선)<br>**0x76** (SDO 핀 $\rightarrow$ GND 결선)<br>**LTC4311** 적용 | 동일 디폴트 주소 충돌 방지<br>독립 주소 할당 완료<br>2m 케이블 시그널 왜곡 방지 |
| **샘플링 주기 수명**<br>*(Sampling Frequency)* | 1s 주기<br>10s 주기<br>30s 주기<br>60s 주기<br>**300s 주기 (제안)** | 2.5 Hours<br>8.0 Hours<br>18.0 Hours<br>30.0 Hours<br>**72.0 Hours (배터리 효율 극대화)** | 주기가 길어질수록 슬립 모드(<br>Sleep Mode) 진입 시간 확대로<br>배터리 보존 시간 비약적 상승 |
| **센서 해상도 에너지**<br>*(Resolution vs Energy)* | OSR 256<br>OSR 512<br>**OSR 1024 (제안)**<br>OSR 2048<br>OSR 4096 | 0.756 µAs<br>1.484 µAs<br>**2.912 µAs (최적 밸런스)**<br>5.782 µAs<br>11.508 µAs | 최저 해상도 대비 소모 전류는<br>증가하나, 데이터 정밀도와<br>에너지 비용 간의 최적 타협점 |

---

### 📈 TX Power vs Current Consumption Reference
추가적으로, 송신 출력(TX Power) 변화에 따른 시스템의 전류 소모 트렌드는 아래와 같습니다. 무선 송신 주기 및 출력 세기 제어 시 가이드라인으로 활용됩니다.

<svg width="600" height="320" viewBox="0 0 600 320" xmlns="http://www.w3.org/2000/svg">
  <rect width="600" height="320" fill="#0d1117" rx="8"/>
  <line x1="80" y1="20" x2="80" y2="240" stroke="#30363d" stroke-width="1"/>
  <line x1="80" y1="240" x2="560" y2="240" stroke="#30363d" stroke-width="1"/>
  <line x1="80" y1="180" x2="560" y2="180" stroke="#21262d" stroke-width="1" stroke-dasharray="3,3"/>
  <line x1="80" y1="120" x2="560" y2="120" stroke="#21262d" stroke-width="1" stroke-dasharray="3,3"/>
  <line x1="80" y1="60" x2="560" y2="60" stroke="#21262d" stroke-width="1" stroke-dasharray="3,3"/>
  <text x="70" y="244" fill="#58a6ff" font-size="10" text-anchor="end" font-family="monospace">0</text>
  <text x="70" y="184" fill="#58a6ff" font-size="10" text-anchor="end" font-family="monospace">68</text>
  <text x="70" y="124" fill="#58a6ff" font-size="10" text-anchor="end" font-family="monospace">136</text>
  <text x="70" y="64"  fill="#58a6ff" font-size="10" text-anchor="end" font-family="monospace">204</text>
  <text x="18" y="140" fill="#58a6ff" font-size="10" text-anchor="middle" font-family="monospace" transform="rotate(-90,18,140)">Current (uA)</text>
  <text x="160" y="258" fill="#8b949e" font-size="10" text-anchor="middle" font-family="monospace">-20 dBm</text>
  <text x="280" y="258" fill="#8b949e" font-size="10" text-anchor="middle" font-family="monospace">-4 dBm</text>
  <text x="400" y="258" fill="#8b949e" font-size="10" text-anchor="middle" font-family="monospace">0 dBm</text>
  <text x="520" y="258" fill="#8b949e" font-size="10" text-anchor="middle" font-family="monospace">8 dBm</text>
  <text x="340" y="285" fill="#8b949e" font-size="11" text-anchor="middle" font-family="monospace">TX Power (dBm)</text>
  <polyline points="160,192 280,149 400,152 520,31" fill="none" stroke="#58a6ff" stroke-width="2.5" stroke-linejoin="round" stroke-linecap="round"/>
  <polygon points="160,192 280,149 400,152 520,31 520,240 160,240" fill="#58a6ff" fill-opacity="0.07"/>
  <circle cx="160" cy="192" r="4" fill="#0d1117" stroke="#58a6ff" stroke-width="2"/>
  <circle cx="280" cy="149" r="4" fill="#0d1117" stroke="#58a6ff" stroke-width="2"/>
  <circle cx="400" cy="152" r="4" fill="#0d1117" stroke="#58a6ff" stroke-width="2"/>
  <circle cx="520" cy="31"  r="4" fill="#0d1117" stroke="#58a6ff" stroke-width="2"/>
  <text x="160" y="184" fill="#58a6ff" font-size="9" text-anchor="middle" font-family="monospace">54.3uA</text>
  <text x="280" y="141" fill="#58a6ff" font-size="9" text-anchor="middle" font-family="monospace">103uA</text>
  <text x="400" y="144" fill="#58a6ff" font-size="9" text-anchor="middle" font-family="monospace">100uA</text>
  <text x="520" y="23"  fill="#58a6ff" font-size="9" text-anchor="middle" font-family="monospace">238uA</text>
  <text x="320" y="14" fill="#f0f6fc" font-size="12" text-anchor="middle" font-family="monospace" font-weight="bold">TX Power vs Current Consumption</text>
</svg>

