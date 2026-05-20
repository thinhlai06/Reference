# Smart IV & Patient Safety Monitoring System  
## Reference Materials Organized by Topic

**Purpose:** This document organizes the main datasets, papers, technical documentation, and supporting references used for the Smart IV & Patient Safety Monitoring System project. It is intended as a concise reference file for technical discussion, project defense, and judging Q&A.

---

## 1. Hardware Platform: Silicon Labs xG26

### Silicon Labs xG26 MCU Reference

- **Key specification used in this project:**  
  - 3200 kB Flash  
  - 512 kB RAM

### Project Relevance

The Silicon Labs xG26 platform is used as the edge device for local sensing, signal processing, wireless communication, and lightweight AI inference.

---

## 2. Physiological Signal Datasets

### 2.1 Pulse Transit Time PPG Dataset

**Dataset link:**  
https://physionet.org/content/pulse-transit-time-ppg/1.1.0/


**Project relevance:**

This dataset is relevant for training, validating, or referencing physiological-signal processing models involving PPG, ECG, SpO₂, and blood-pressure-related estimation.

---

### 2.2 human-vital-sign-dataset

**Dataset link:**  
https://www.kaggle.com/datasets/nasirayub2/human-vital-sign-dataset


**Project relevance:**

This dataset is useful as a reference for multi-sensor physiological monitoring and synchronized vital-sign data acquisition.

---

## 3. Papers Using Physiological Datasets

### 3.1 Heart Rate Estimation Using PPG and Pressure/Load-Cell Signal

**Paper link:**  
https://ieeexplore.ieee.org/document/9585692

**Dataset used:**  
Pulse Transit Time PPG Dataset

**Signals used:**

- PPG
- Pressure/load-cell signal

**Objective:**

Improve heart-rate estimation accuracy.

**Method summary:**

- PPG signal is collected from a MAX30101 sensor.
- Pressure signal is used to identify poor sensor contact, loose wearing, excessive pressure, or unstable PPG signal quality.
- A deep-learning fusion model learns the relationship between PPG, pressure signal, and heart rate.

**Model type:**  
Deep learning fusion model

**Project relevance:**

This paper supports the idea of combining physiological sensing with pressure/contact-quality information to improve vital-sign estimation reliability.

---

### 3.2 Blood Pressure Estimation from PPG Using ResNet-50

**Paper link:**  
https://pubmed.ncbi.nlm.nih.gov/40632332/

**Dataset used:**  
Pulse Transit Time PPG Dataset

**Signals used:**

- PPG

**Objective:**

Estimate systolic and diastolic blood pressure.

**Method summary:**

- Original PPG signal is used.
- Additional transformed PPG representations are created:
  - Original PPG
  - vPPG: first derivative of PPG
  - aPPG: second derivative of PPG
- Signals are transformed into image-like representations suitable for deep learning.
- A deep learning model is used for blood pressure prediction.

**Model type:**  
ResNet-50

**Project relevance:**

This paper supports the feasibility of deriving blood-pressure-related information from PPG-based physiological signals.

---

### 3.3 Blood Pressure Prediction Using ECG and PPG

**Paper link:**  
https://www.sciencedirect.com/science/article/pii/S0010482525017317

**Datasets used:**

- Pulse Transit Time PPG Dataset
- Additional datasets

**Signals used:**

- ECG
- PPG

**Objective:**

Predict blood pressure.

**Method summary:**

- Detect R peaks from ECG.
- Detect PPG peaks.
- Calculate Pulse Arrival Time / Pulse Transit Time-like features using the time difference between ECG and PPG.
- Construct embedding or distance-matrix representations to capture relationships between signals.
- Feed extracted features into a deep learning model.

**Model type:**  
Attention-guided CNN

**Project relevance:**

This paper supports the use of ECG–PPG timing features for blood-pressure-related estimation.

---

## 4. IV Drip-Rate and Infusion Flow References

### 4.1 IV Drip-Rate Dataset Availability

**Current reference status:**

- Public datasets for IV drip oscillation or drip-period classification are limited or generally unavailable.
- Many IV drip-rate studies collect their own experimental data.
- The project can reasonably use self-collected drip data from the prototype system.

**Project relevance:**

This justifies collecting project-specific drip-rate data using the selected optical drip sensor and infusion setup.

---

### 4.2 Monitoring of Intravenous Drip Rate

**Reference topic:**  
Monitoring of Intravenous Drip Rate

**Data used:**

- Signal from photo-sensor / IR sensor placed across the drip chamber
- Each falling drop interrupts the light beam and generates one pulse
- The time interval between consecutive pulses is measured as:
  - Drop interval
  - Drip interval

**Method used:**

- Signal processing
- Pulse counting
- Averaging over time

**Project relevance:**

This reference directly supports the project’s drip detection method: detecting individual drops, calculating drop interval, and estimating drip rate.

---

### 4.3 IV Flow-Rate Classification and Medical Basis

The project should not define “fast”, “normal”, or “slow” infusion as fixed universal values for all patients. Infusion rate depends on medical order, patient condition, fluid type, medication, body weight, and clinical purpose.

**Key reference topics:**

1. NICE CG174 — Intravenous fluid therapy in adults in hospital  
2. Atanda et al. 2023 — Flow rate accuracy of infusion devices within healthcare settings: a systematic review  
3. Doyle et al. 2021 — Determining an Appropriate TKVO Infusion Rate for PIVC Usage  
4. Giaquinto et al. 2020 — Deep Learning-Based Computer Vision for Real Time IV Drip Infusion Monitoring  
5. Venkatesh et al. 2022 — DripOMeter: An open-source opto-electronic system for IV infusion monitoring  

**Project relevance:**

These references support a relative flow-classification approach based on deviation from a target rate rather than a fixed universal threshold.

**Recommended project interpretation:**

- The clinician or user defines the target infusion rate.
- The system measures actual drip rate.
- The system classifies flow status based on deviation from the target:
  - Slow Flow
  - Normal Flow
  - Fast Flow
  - No Drip
  - Unstable Flow

---

### 4.4 Drip Rate Conversion Formulas

**Formula references used in the project:**

```text
drops/min = (mL/hour × drop factor) / 60

mL/hour = (drops/min × 60) / drop factor

rate_error_percent = ((actual_flow_rate - target_flow_rate) / target_flow_rate) × 100
```

**Project relevance:**

These formulas connect optical drop counting with clinically interpretable flow-rate values.

---

## 5. Edge AI and Model Deployment

### 5.1 Edge Impulse

**Platform link:**  
https://www.edgeimpulse.com/

**Documentation:**  
https://docs.edgeimpulse.com/?utm_source=chatgpt.com

**Summary:**

Edge Impulse is a web-based platform for collecting data, training machine-learning models, optimizing models, and exporting them to edge devices or microcontrollers.

**Project relevance:**

Suitable for rapid prototyping when the team wants a complete workflow from data collection to deployment without building the full ML pipeline manually.

---

### 5.2 TensorFlow Lite / TensorFlow Lite Micro

**Documentation:**  
https://ai.google.dev/edge/litert/microcontrollers/get_started?utm_source=chatgpt.com&hl=vi

**Summary:**

TensorFlow Lite converts TensorFlow models into lightweight `.tflite` models. TensorFlow Lite for Microcontrollers is designed for running inference on microcontrollers with limited RAM and Flash.

**Project relevance:**

Suitable for deploying lightweight AI models directly on the edge node.

---

### 5.3 Silicon Labs AI/ML Extension

**Documentation:**  
https://siliconlabs.github.io/mltk/?utm_source=chatgpt.com

**Summary:**

Silicon Labs AI/ML Extension provides tools in the Silicon Labs ecosystem for developing, optimizing, and deploying machine-learning models on Silicon Labs hardware.

**Project relevance:**

Highly relevant because the project uses Silicon Labs xG26 as the edge processing platform.

---

## 6. Zigbee and Wireless Communication References

### 6.1 Silicon Labs Zigbee Fundamentals

**Documentation link:**  
https://docs.silabs.com/zigbee/8.2.3/zigbee-fundamentals

**Summary:**

Official Silicon Labs documentation explaining Zigbee fundamentals, network structure, node types, mesh networking, gateways, routing, and Zigbee application development on Silicon Labs platforms.

**Project relevance:**

Primary technical reference for using Zigbee communication with Silicon Labs hardware.

---

### 6.2 A Mobility Enabled Inpatient Monitoring System Using a ZigBee Medical Sensor Network

**Paper link:**  
https://openaccess.city.ac.uk/id/eprint/3675/1/A%20mobility%20enabled%20inpatient%20monitoring%20system%20using%20a%20ZigBee%20medical%20sensor%20network..pdf

**Summary:**

This paper presents an inpatient monitoring system using a Zigbee medical sensor network. Wearable or patient-side sensor nodes transmit data to a central hospital monitoring system.

**Project relevance:**

Supports the use of Zigbee for hospital patient-monitoring networks.

---

### 6.3 Remote Wireless Health Care Monitoring System Using ZIGBEE

**Paper link:**  
https://www.ijert.org/research/remote-wireless-health-care-monitoring-system-using-zigbee-IJERTV1IS6036.pdf

**Summary:**

This paper discusses a remote healthcare monitoring system using Zigbee. Physiological parameters such as heart rate, blood pressure, or biosignals are collected from sensors and transmitted wirelessly to a monitoring station.

**Project relevance:**

Supports the use of Zigbee for transmitting health-monitoring data from sensor nodes to a central system.

---

### 6.4 Design and Development of a Monitoring and Controlling System for Multi-Intravenous Infusion

**Paper link:**  
https://iopscience.iop.org/article/10.1088/1742-6596/1367/1/012075/pdf

**Summary:**

This paper presents a system for monitoring and controlling multiple IV infusion lines. It includes drip-rate measurement, volume monitoring, and support for managing multiple infusion cases through a monitoring interface.

**Project relevance:**

Directly related to multi-infusion monitoring and centralized IV management.

---

### 6.5 Real-Time Health Monitoring System on Wireless Sensor Network

**Paper link:**  
https://www.omicsonline.org/open-access/realtime-health-monitoring-system-on-wireless-sensor-network-2277-1891.1000126.pdf

**Summary:**

This paper presents a real-time health monitoring system based on wireless sensor networks.

**Project relevance:**

Supports the general architecture of wireless sensor-based health monitoring.

---

### 6.6 Design of a Wireless Sensor Network Platform for Tele-Homecare

**Paper link:**  
https://www.mdpi.com/1424-8220/13/12/17156

**Summary:**

This paper develops a wireless sensor network platform for tele-homecare. It focuses on using Zigbee to transmit vital-sign data from medical sensors to a management system. The paper highlights Zigbee advantages such as low power consumption, low cost, and support for multiple nodes.

**Project relevance:**

Supports the use of Zigbee for low-power, multi-node healthcare monitoring.

---

### 6.7 Design of Intelligent Monitoring System Based on ZigBee Wireless Network

**Paper link:**  
https://www.atlantis-press.com/proceedings/isrme-15/18334

**Summary:**

This paper discusses the design of an intelligent monitoring system based on a Zigbee wireless network.

**Project relevance:**

Supports Zigbee-based sensor data collection and wireless transmission to a monitoring center.

---

### 6.8 Energy Optimization of ZigBee Based WBAN for Patient Monitoring

**Paper link:**  
https://www.sciencedirect.com/science/article/pii/S1877050915032196

**Summary:**

This paper studies energy optimization for Zigbee-based Wireless Body Area Networks used in patient monitoring.

**Project relevance:**

Supports the use of Zigbee in battery-powered patient-monitoring sensor nodes.

---

### 6.9 IoT-Based Intelligent Infusion Monitoring System and IV Infusion Safety

**Paper link:**  
https://www.frontiersin.org/journals/medicine/articles/10.3389/fmed.2025.1709955

**Summary:**

This study evaluates an IoT-based intelligent infusion monitoring system for hospitalized patients. The system uses Zigbee wireless communication to transmit infusion data from bedside devices to a client/server system and central monitoring display.

**Project relevance:**

Directly supports the project’s use of Zigbee for intelligent IV infusion monitoring in hospital environments.

---

## 7. Wireless Technology Comparison

| Criterion | Zigbee | Wi-Fi | Bluetooth / BLE |
|---|---|---|---|
| Small sensor data | Very suitable | Excessive bandwidth | Suitable |
| Power saving | Good | Weaker | Good |
| Many beds / many nodes | Very good | May overload with many nodes | Harder to manage |
| Mesh network expansion | Supported | Not optimized for sensor mesh | BLE Mesh exists but is more complex |
| Hospital suitability | Suitable for dedicated sensor networks | Depends on hospital Wi-Fi infrastructure | Suitable for short-range personal devices |
| Best role | Sensor node → Gateway | Gateway → Server/Dashboard | Personal device near gateway |

**Project conclusion:**

Zigbee is suitable for bedside sensor nodes because it supports low-power communication, multi-node networks, and mesh expansion. Wi-Fi is more suitable for gateway-to-server or dashboard communication. BLE is suitable for short-range personal-device scenarios but is less convenient for managing many hospital beds.

---

## 8. Project Reference Mapping

| Project Area | Main Reference Type | Reference Links |
|---|---|---|
| Edge hardware | Silicon Labs xG26 specification | Silicon Labs xG26 reference |
| Physiological signal data | Public datasets | https://physionet.org/content/pulse-transit-time-ppg/1.1.0/ |
| Physiological signal data | Public datasets | https://physionet.org/content/simultaneous-measurements/1.0.2/ |
| Heart-rate estimation | Research paper | https://ieeexplore.ieee.org/document/9585692 |
| Blood-pressure estimation | Research paper | https://pubmed.ncbi.nlm.nih.gov/40632332/ |
| ECG + PPG blood-pressure prediction | Research paper | https://www.sciencedirect.com/science/article/pii/S0010482525017317 |
| IV drip monitoring | Research topic | Monitoring of Intravenous Drip Rate |
| Model deployment | Edge Impulse | https://www.edgeimpulse.com/ |
| Model deployment | Edge Impulse documentation | https://docs.edgeimpulse.com/?utm_source=chatgpt.com |
| Model deployment | TensorFlow Lite Micro | https://ai.google.dev/edge/litert/microcontrollers/get_started?utm_source=chatgpt.com&hl=vi |
| Silicon Labs AI deployment | Silicon Labs AI/ML Extension | https://siliconlabs.github.io/mltk/?utm_source=chatgpt.com |
| Zigbee fundamentals | Official Silicon Labs documentation | https://docs.silabs.com/zigbee/8.2.3/zigbee-fundamentals |
| Zigbee medical monitoring | Research paper | https://openaccess.city.ac.uk/id/eprint/3675/1/A%20mobility%20enabled%20inpatient%20monitoring%20system%20using%20a%20ZigBee%20medical%20sensor%20network..pdf |
| Zigbee healthcare monitoring | Research paper | https://www.ijert.org/research/remote-wireless-health-care-monitoring-system-using-zigbee-IJERTV1IS6036.pdf |
| Multi-IV infusion monitoring | Research paper | https://iopscience.iop.org/article/10.1088/1742-6596/1367/1/012075/pdf |
| Wireless health monitoring | Research paper | https://www.omicsonline.org/open-access/realtime-health-monitoring-system-on-wireless-sensor-network-2277-1891.1000126.pdf |
| Tele-homecare WSN | Research paper | https://www.mdpi.com/1424-8220/13/12/17156 |
| Zigbee intelligent monitoring | Research paper | https://www.atlantis-press.com/proceedings/isrme-15/18334 |
| Zigbee WBAN energy optimization | Research paper | https://www.sciencedirect.com/science/article/pii/S1877050915032196 |
| Intelligent infusion monitoring | Research paper | https://www.frontiersin.org/journals/medicine/articles/10.3389/fmed.2025.1709955 |

---

# Smart IV & Patient Safety Monitoring System  
## Sensor Component Reference

This document summarizes the main sensor options for the Smart IV & Patient Safety Monitoring System.  
The components are grouped by sensing function, including IV drip detection, IV bag weight measurement, heart rate and SpO₂ monitoring, and body temperature monitoring.

---

# 1. IV Drip Detection

## 1.1 Hanyoung PU-30 U-Shaped Optical Sensor, 12–24 VDC, NPN

### Structure and Operating Principle

The Hanyoung PU-30 is a U-shaped optical sensor. Inside the two arms of the sensor, there is an infrared transmitter and receiver system with focusing lenses.  
When the sensor is attached to the drip chamber, it creates a light path across the chamber.

### Detection Mechanism

When an IV drop falls through the sensing area, it changes the refractive index or partially blocks the infrared light path.  
The receiver, usually a phototransistor, detects the decrease in light intensity and converts it into an electrical pulse.

Each detected pulse can be counted as one IV drop.

### Reliability

High reliability.  
This sensor provides stable drop detection and is less affected by environmental noise compared with simple IR obstacle sensors. It is suitable for continuous drip counting.

### Cost

Relatively high.  
It is more expensive than common IR obstacle sensors because it uses a dedicated through-beam optical detection structure and provides better accuracy.

### Suitability for the System

Very suitable as the main IV drip sensor for the Smart IV & Patient Safety Monitoring System.  
It provides stable real-time detection, fast response, and can be integrated with Silicon Labs xG24/xG26 microcontrollers.

---

## 1.2 Infrared IR Obstacle Sensor

### Structure and Operating Principle

An IR obstacle sensor typically includes an infrared LED transmitter and a reflected-light receiver such as a photodiode or phototransistor.  
When mounted near the IV drip chamber or IV line, the sensor creates an infrared sensing area to detect the presence of drops or flow-related movement.

### Detection Mechanism

When a drop or object passes through the sensing area, the reflected infrared light intensity changes.  
The receiver detects this change and converts it into a signal pulse, which can be sent to the Silicon Labs xG24/xG26 microcontroller for real-time processing.

### Reliability

Medium reliability.  
The sensor can be affected by ambient light, sensor position, chamber transparency, and mounting angle. Therefore, its accuracy is generally lower than a U-shaped optical sensor for continuous IV drop counting.

### Cost

Low cost.  
This module is widely available and easy to use, making it suitable for prototypes and student research projects.

### Suitability for the System

Suitable as a supporting sensor for the Smart IV & Patient Safety Monitoring System.  
It is easy to integrate, has fast response, and can support real-time flow status detection. However, it is less ideal as the primary sensor for accurate drip counting.

---

# 2. IV Bag Weight Measurement

## 2.1 S-Type Load Cell 0–5 kg with HX711

### Structure and Operating Principle

This module combines an S-type load cell with the HX711 amplifier and analog-to-digital converter.  
When the IV bag is suspended from the load cell, the pulling force deforms the strain gauge inside the load cell. This deformation creates a small analog electrical signal, which is amplified and converted into digital data by the HX711.

### Detection Mechanism

During infusion, the weight of the IV bag gradually decreases.  
The Silicon Labs xG24/xG26 microcontroller reads data from the HX711 to estimate the remaining fluid volume, detect near-empty status, and compare the weight-based estimation with drip-count data.

### Reliability

High reliability.  
The S-type load cell is suitable for hanging-load measurement, which matches the real setup of an IV bag.  
When combined with drip detection, it can reduce false alerts and improve system accuracy.

### Cost

Medium cost.  
It is more expensive than common bar-type load cells but still suitable for prototype and smart healthcare research systems.

### Suitability for the System

Very suitable for the Smart IV & Patient Safety Monitoring System.  
It measures the suspended IV bag directly, supports real-time fluid estimation, and can improve the professional quality of the prototype during demonstration.

---

## 2.2 Bar-Type Load Cell with 24-bit HX711 ADC

### Structure and Operating Principle

This module includes a bar-type strain gauge load cell and the HX711 24-bit ADC module.  
When force is applied to the load cell, the strain gauge resistance changes slightly. The HX711 amplifies and converts this analog signal into digital data for the microcontroller.

### Detection Mechanism

As the IV fluid is infused, the mass of the IV bag decreases over time.  
The microcontroller reads HX711 data to calculate the remaining fluid amount, detect near-empty status, and compare the result with drip-rate data.

### Reliability

Fairly high reliability.  
It provides stable weight measurement with relatively small error. It can support abnormal infusion detection when combined with the drip-counting sensor.

### Cost

Low to medium cost.  
The module is common, affordable, and suitable for IoT healthcare and embedded AI prototypes.

### Suitability for the System

Suitable for the Smart IV & Patient Safety Monitoring System, especially for low-cost prototypes.  
However, for a hanging IV bag setup, an S-type load cell is generally more mechanically suitable and easier to justify.

---

# 3. Heart Rate and SpO₂ Measurement

## 3.1 SparkFun Pulse Oximeter MAX30101 / MAX30102 Module

### Structure and Operating Principle

The SparkFun Pulse Oximeter module uses the MAX30101 or MAX30102 sensor, which integrates red LEDs, infrared LEDs, and a photodetector.  
It measures heart rate and blood oxygen saturation using the PPG method, also known as photoplethysmography.

PPG works by sending light into the skin and measuring the reflected light from blood vessels.

### Detection Mechanism

As blood flows through capillaries, the amount of absorbed and reflected light changes according to the heartbeat.  
The module captures these optical changes and estimates heart rate and SpO₂. The data is then sent through I2C to the Silicon Labs xG24/xG26 microcontroller.

### Reliability

Fairly high for wearable healthcare prototypes.  
The measurement is stable when the sensor is placed correctly and when strong motion artifacts are avoided.

### Cost

Low to medium cost.  
It is much cheaper than professional medical reference platforms, making it suitable for student research projects.

### Suitability for the System

Very suitable for the Smart IV & Patient Safety Monitoring System.  
It is compact, easy to integrate with Edge AI and wireless communication, and supports real-time vital sign monitoring.  
The heart rate and SpO₂ data can be combined with IV infusion data for abnormal condition detection.

---

## 3.2 DIYMORE MAX30102 Pulse Oximeter Module

### Structure and Operating Principle

The DIYMORE MAX30102 module integrates red LEDs, infrared LEDs, and a photodetector to measure heart rate and SpO₂.  
It is based on the PPG optical sensing principle, where light is emitted into the skin and the reflected signal is measured.

### Detection Mechanism

When blood volume changes with each heartbeat, the light absorption also changes.  
The module detects these changes and calculates heart rate and SpO₂. The processed data is transmitted to the Silicon Labs xG24/xG26 microcontroller through I2C.

### Reliability

Fairly high for wearable healthcare and prototype applications.  
It performs well in low-motion conditions, has low power consumption, and provides fast real-time response.

### Cost

Low cost.  
It is widely available in DIY and embedded systems markets, making it suitable for cost-sensitive student projects.

### Suitability for the System

Very suitable for the Smart IV & Patient Safety Monitoring System.  
It is compact, easy to integrate with Edge AI and BLE/Zigbee, and supports continuous monitoring of heart rate and SpO₂.

---

# 4. Body Temperature Measurement

## 4.1 MAX30205 Human Body Temperature Sensor Module

### Structure and Operating Principle

The MAX30205 module is a digital body temperature sensor designed for healthcare applications.  
It communicates through I2C and provides high-resolution temperature measurement with low error in the human body temperature range.

### Detection Mechanism

The MAX30205 uses an integrated semiconductor temperature sensing element to measure contact temperature from the body surface.  
The measured temperature is converted internally into digital data and sent through I2C to the Silicon Labs xG24/xG26 microcontroller.

### Reliability

Very high for healthcare-related applications.  
The sensor can reach approximately ±0.1°C accuracy within the human body temperature range. It is stable, low-power, and suitable for continuous monitoring.

### Cost

Medium cost.  
It is more expensive than common temperature sensors such as LM35 or DS18B20, but it is more suitable for healthcare and patient monitoring applications.

### Suitability for the System

Very suitable for the Smart IV & Patient Safety Monitoring System.  
Body temperature data can be combined with heart rate, SpO₂, and IV infusion status to support early detection of fever, infection, or abnormal patient conditions.

---

## 4.2 Pcbfun MAX30205 I2C Human Body Temperature Sensor Module

### Structure and Operating Principle

The Pcbfun MAX30205 module uses the MAX30205 digital temperature sensor, which is designed for accurate human body temperature measurement.  
It communicates through I2C and supports real-time monitoring with low power consumption.

### Detection Mechanism

The sensor detects temperature changes through contact with the skin surface.  
After internal signal processing, the temperature data is transmitted digitally through I2C to the Silicon Labs xG24/xG26 microcontroller.

### Reliability

High reliability.  
It provides stable measurement with low error in the human body temperature range. It is suitable for continuous monitoring and has good noise resistance.

### Cost

Relatively higher than general-purpose temperature sensors.  
However, it is suitable for systems that require medical-oriented accuracy and long-term stability.

### Suitability for the System

Very suitable for the Smart IV & Patient Safety Monitoring System.  
It supports continuous body temperature monitoring and can be used together with heart rate, SpO₂, and infusion data to detect fever, infection, or other abnormal conditions during treatment.

---

# 5. Component Suitability Summary

| Function | Component Option | Reliability | Cost | Suitability |
|---|---|---:|---:|---|
| IV drip detection | Hanyoung PU-30 U-shaped optical sensor | High | High | Very suitable as the main drip sensor |
| IV drip detection | IR obstacle sensor | Medium | Low | Suitable as a supporting/prototype sensor |
| IV bag weight measurement | S-type load cell 0–5 kg + HX711 | High | Medium | Very suitable for hanging IV bag measurement |
| IV bag weight measurement | Bar-type load cell + HX711 | Fairly high | Low–Medium | Suitable for low-cost prototype measurement |
| Heart rate and SpO₂ | SparkFun MAX30101/MAX30102 pulse oximeter | Fairly high | Low–Medium | Very suitable for real-time vital sign monitoring |
| Heart rate and SpO₂ | DIYMORE MAX30102 module | Fairly high | Low | Very suitable for low-cost prototype monitoring |
| Body temperature | MAX30205 human body temperature module | Very high | Medium | Very suitable for patient temperature monitoring |
| Body temperature | Pcbfun MAX30205 I2C module | High | Medium–High | Very suitable for continuous temperature monitoring |

---

