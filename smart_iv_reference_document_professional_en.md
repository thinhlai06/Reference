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

