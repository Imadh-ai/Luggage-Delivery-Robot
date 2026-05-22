<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1b2a,50:0e4d8a,100:00c8ff&height=200&section=header&text=Smart%20Airport%20Luggage%20Robot&fontSize=36&fontColor=ffffff&fontAlignY=38&desc=Autonomous%20Navigation%20%7C%20RFID%20%7C%20Load%20Sensing&descAlignY=58&descSize=16" width="100%"/>

<br/>

![Arduino](https://img.shields.io/badge/Arduino-UNO-00979D?style=for-the-badge&logo=arduino&logoColor=white)
![ESP8266](https://img.shields.io/badge/NodeMCU-ESP8266-E7352C?style=for-the-badge&logo=espressif&logoColor=white)
![C++](https://img.shields.io/badge/Embedded-C%2FC%2B%2B-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)
![RFID](https://img.shields.io/badge/RFID-RC522-6A0DAD?style=for-the-badge&logo=nfc&logoColor=white)
![Status](https://img.shields.io/badge/Status-Prototype%20Complete-2ea44f?style=for-the-badge)

<br/><br/>

> **An intelligent embedded robotic system that autonomously carries passenger luggage and guides users to predefined airport destinations using IR line-following, RFID authentication, and real-time load monitoring.**

<br/>

</div>

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [System Workflow](#%EF%B8%8F-system-workflow)
- [Hardware Components](#%EF%B8%8F-hardware-components)
- [System Architecture](#-system-architecture)
- [Software & Technologies](#-software--technologies)
- [Project Images](#-project-images)
- [Objectives](#-objectives)
- [Future Improvements](#-future-improvements)
- [Keywords](#-keywords)

---

## 🌐 Overview

This project presents a **Smart Airport Assistance Robot** that reduces passenger effort by transporting luggage and navigating autonomously inside airport environments.

The robot follows predefined paths using **IR sensor-based line tracking**, identifies destinations using **RFID authentication**, and measures luggage weight using a **Load Cell + HX711** system.

It demonstrates practical implementation of **embedded systems, sensor fusion, and autonomous navigation** in real-world public transport applications.

---

## 🚀 Key Features

| # | Feature | Description |
|---|---------|-------------|
| ✅ | **Autonomous Navigation** | IR sensor array–based line-following with real-time correction |
| ✅ | **RFID Destination Selection** | Passenger taps card → robot maps UID to route |
| ✅ | **Real-Time Weight Sensing** | Load Cell + HX711 24-bit ADC for accurate luggage measurement |
| ✅ | **Modular Embedded Design** | Clean separation of controller, sensing, and actuation units |
| ✅ | **Predefined Route Guidance** | Multi-destination support (gates, counters, terminals) |
| ✅ | **Battery-Powered Platform** | Li-ion pack with BMS for safe portable operation |
| ✅ | **IoT Expansion Ready** | NodeMCU ESP8266 for optional cloud/mobile integration |
| ✅ | **Low-Cost Prototype** | Affordable components, scalable for real deployment |

---

## ⚙️ System Workflow

```
┌─────────────────────────────────────────────────────────────┐
│                    SYSTEM STARTUP                           │
│         Initialize sensors · UART · Motor drivers          │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                  LUGGAGE PLACEMENT                          │
│          Passenger places luggage on robot tray             │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                 WEIGHT MEASUREMENT                          │
│       Load Cell + HX711 samples & filters weight data      │
│             ⚠️  Overload? → Safety halt triggered           │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                RFID DESTINATION SCAN                        │
│        RC522 reads UID → Maps to Gate / Terminal           │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                  ROUTE PROCESSING                           │
│    Arduino loads navigation instruction set for route       │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│              LINE-FOLLOWING NAVIGATION                      │
│    IR array detects track → L298N drives DC motors         │
│         PID correction keeps robot on path                 │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│               ✅ DESTINATION REACHED                        │
│      Robot halts · Signal issued · System resets           │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Hardware Components

| Component | Model | Role |
|-----------|-------|------|
| 🧠 **Microcontroller** | Arduino UNO | Main logic, motor control & sensor management |
| 📶 **IoT Module** | NodeMCU ESP8266 | WiFi communication & cloud expansion |
| ⚡ **Motor Driver** | L298N | Dual H-bridge — motor direction & speed control |
| 👁️ **Line Sensor** | IR Sensor Array | Multi-channel line detection for navigation |
| 🪪 **RFID Module** | RC522 (13.56 MHz) | Destination selection via card authentication |
| ⚖️ **Weight Sensor** | Load Cell + HX711 | Real-time 1kg precision luggage weight sensing |
| 🔧 **Drive Motors** | DC Geared Motors | High-torque platform movement & maneuvering |
| 🔋 **Power Supply** | Li-ion Pack + BMS | Portable battery with protection management |
| 🔌 **Regulator** | DC-DC Buck Converter | Stable voltage for all subsystems |
| 🏗️ **Platform** | Robot Chassis | Structural base with luggage tray mount |

---

## 📡 System Architecture

```
╔══════════════════════════════════════════════════════════════════╗
║                     SYSTEM ARCHITECTURE                         ║
╠══════════════╦═══════════════╦═══════════════╦══════════════════╣
║  CONTROLLER  ║ COMMUNICATION ║    SENSING    ║    ACTUATION     ║
║─────────────║───────────────║───────────────║──────────────────║
║ Arduino UNO  ║ NodeMCU       ║ IR Sensor     ║ L298N Driver     ║
║              ║ ESP8266       ║ Array         ║                  ║
║ • Main Logic ║               ║               ║ • Motor Speed    ║
║ • PID Nav    ║ • UART Link   ║ • Load Cell   ║ • Direction Ctrl ║
║ • Route Map  ║ • WiFi / IoT  ║ • HX711 ADC   ║ • DC Geared      ║
║ • Safety     ║ • Firebase    ║ • RFID RC522  ║   Motors x2      ║
╚══════════════╩═══════════════╩═══════════════╩══════════════════╝
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
       UART / SPI Bus         PWM / GPIO
     (Sensor Comms)         (Motor Control)
```

---

## 💻 Software & Technologies

![Embedded C](https://img.shields.io/badge/Embedded_C-C%2FC%2B%2B-00599C?style=flat-square&logo=c%2B%2B)
![Arduino IDE](https://img.shields.io/badge/IDE-Arduino_IDE-00979D?style=flat-square&logo=arduino)
![UART](https://img.shields.io/badge/Protocol-UART_Communication-555555?style=flat-square)
![RFID](https://img.shields.io/badge/Library-MFRC522_RFID-6A0DAD?style=flat-square)
![HX711](https://img.shields.io/badge/Library-HX711_ADC-orange?style=flat-square)
![SensorFusion](https://img.shields.io/badge/Logic-Sensor_Fusion-2ea44f?style=flat-square)

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Language** | Embedded C / C++ | Core firmware for all modules |
| **IDE** | Arduino IDE | Development, compilation & flashing |
| **Protocol** | UART | Arduino ↔ ESP8266 communication |
| **Library** | MFRC522 | RFID read/write and UID handling |
| **Library** | HX711 | Load cell amplifier & ADC interface |
| **Algorithm** | Line-follow + PID | Smooth real-time navigation correction |
| **Expansion** | Firebase / REST | Optional IoT cloud data logging |

---

## 📷 Project Images

<div align="center">

| Robot View 01 | Robot View 02 |
|:---:|:---:|
| ![Robot Image 1](https://github.com/user-attachments/assets/725b8044-759d-4ec6-9864-8ac4600b3850) | ![Robot Image 2](https://github.com/user-attachments/assets/6b3ac88d-7877-4ce6-9088-a6bfd29c37d1) |
| *Front assembly with IR array & chassis* | *Top view showing load cell tray & wiring* |

</div>

---

## 🎯 Objectives

- 🔹 Reduce physical effort of passengers navigating large airport environments
- 🔹 Provide reliable autonomous indoor navigation assistance
- 🔹 Develop a low-cost, replicable smart robotic prototype
- 🔹 Demonstrate embedded robotics in real-world public transport environments
- 🔹 Improve automation efficiency in airport facilities
- 🔹 Establish a foundation for AI-enhanced passenger mobility systems

---

## 🔮 Future Improvements

| Enhancement | Description |
|-------------|-------------|
| 🔊 **Ultrasonic Obstacle Avoidance** | Real-time collision detection and dynamic path adjustment |
| 🖥️ **LCD / OLED Display** | Live navigation status, weight readout & destination display |
| ☁️ **Firebase IoT Monitoring** | Cloud dashboard for fleet tracking and data logging |
| 📱 **Mobile App Integration** | Remote control, live status and route selection via app |
| 🎙️ **Voice Navigation** | Onboard speech recognition for hands-free destination input |
| 🤖 **AI Human Following** | Computer vision–based passenger tracking using ML models |

---

## 🧠 Keywords

`#Arduino` `#ESP8266` `#RFID` `#LineFollowingRobot` `#EmbeddedSystems` `#IoT` `#Robotics` `#AutonomousNavigation` `#SensorFusion` `#SmartAirport` `#Automation` `#HX711` `#L298N` `#MFRC522` `#LoadCell`

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:00c8ff,50:0e4d8a,100:0d1b2a&height=120&section=footer" width="100%"/>

**Embedded Systems & Robotics Project**
*Autonomous Navigation · RFID Destination Selection · Real-Time Load Monitoring*

Built with ❤️ using **Arduino UNO + NodeMCU ESP8266** · **Embedded C/C++**

</div>
