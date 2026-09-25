<div align="center">

# 🦯 Smart Walker — Fall Detection & Safety Monitoring System

### Arduino-Based Embedded Safety Assistant for Elderly Mobility Aids

*Bilateral IMU fall detection · weight distribution monitoring · obstacle alerts · real-time LCD feedback*

[![Arduino](https://img.shields.io/badge/Arduino-Uno-00979D?style=for-the-badge&logo=arduino&logoColor=white)](https://www.arduino.cc/)
[![C++](https://img.shields.io/badge/C++-Embedded-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)](https://en.wikipedia.org/wiki/C%2B%2B)
[![Standalone](https://img.shields.io/badge/Cloud-Independent-brightgreen?style=for-the-badge)]()
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](#-license)

[![Cost](https://img.shields.io/badge/Est._Hardware_Cost-%3C%20%2430-blue?style=flat-square)]()
[![Sensors](https://img.shields.io/badge/Dual--IMU-Bilateral_Sensing-orange?style=flat-square)]()

</div>

---

## 📖 Overview

This project is an embedded safety-monitoring system for walkers and rollators, built entirely on an **Arduino Uno**. It runs standalone — no cloud, no smartphone, no internet — and continuously watches for fall risk, uneven weight distribution, and nearby obstacles, giving the user immediate audio and visual feedback through a buzzer and LCD.

The goal is simple: turn a passive mechanical walker into a device that can notice when something's going wrong and say so, right when it happens.

> 🩺 **Note:** This is an assistive prototype, not a certified medical device. It's intended to support — not replace — proper clinical guidance.

---

## ✨ Features

| | |
|---|---|
| 🧭 **Bilateral Tilt & Fall Detection** | Two independent IMUs (left + right) track tilt in real time |
| ⚖️ **Weight Distribution Monitoring** | Dual load cells detect overload or left/right imbalance |
| 🚧 **Obstacle Detection** | Dual ultrasonic sensors watch both walker legs for nearby obstacles |
| 🔊 **Pattern-Coded Buzzer Alerts** | Distinct buzzer patterns for obstacles vs. weight issues vs. falls |
| 📟 **Live LCD Readout** | Step count, walking speed, and live left/right weight display |
| 🔁 **Self-Healing I2C Bus** | Automatically recovers from sensor bus lockups without a manual reset |
| 🚶 **Step Counting & Speed Estimation** | Derived from smoothed accelerometer magnitude |
| 🧮 **EMA-Smoothed Angle Tracking** | Reduces noise/jitter in tilt readings before triggering alerts |

---

## 🔌 System Diagram

<div align="center">
<img src="screenshots/wiring_diagram.svg" alt="Smart Walker system block diagram" width="850">
</div>

---

## 🏗️ Hardware Components

| Component | Role | Connection |
|---|---|---|
| Arduino Uno | Main controller | — |
| MPU6050 (×2) | Tilt / fall sensing (right + left) | I2C — 0x68 (right), 0x69 (left) |
| HX711 + Load Cell (×2) | Weight sensing (right + left) | DT/SCK pins 2–5 |
| HC-SR04 (×2) | Ultrasonic obstacle detection (right + left) | Trig/Echo pins 6–9 |
| Passive Buzzer | Audio alerts | Pin 11 |
| 16×2 I2C LCD | Status display | I2C — 0x27 |

**Estimated hardware cost:** under **USD 30**, using widely available components.

---

## ⚙️ How It Works

1. **Startup** — initializes the I2C bus, LCD, both IMUs, and both load cells (with tare-zeroing to remove the walker's own structural weight from readings).
2. **Main loop** — on every cycle, the system:
   - Checks I2C bus health and self-recovers if a sensor stops responding
   - Measures distance on both sides via ultrasonic sensors
   - Reads and smooths weight from both load cells
   - Reads and smooths tilt angle from both IMUs (exponential moving average)
   - Detects steps from accelerometer magnitude and estimates walking speed
   - Checks for a fall (sustained tilt beyond threshold, debounced over multiple readings)
   - Prioritizes alerts: **fall > obstacle > weight imbalance**
   - Refreshes the LCD every 500ms with live stats
   - Logs detailed sensor data to Serial for debugging/tuning

---

## 🚀 Getting Started

### Requirements
- Arduino Uno (or compatible ATmega328P board)
- Arduino IDE
- Libraries: `Wire.h`, `LiquidCrystal_I2C`, `MPU6050`, `HX711`

### Wiring
Refer to the pin definitions at the top of the sketch — I2C devices share `A4`/`A5`, load cells use pins 2–5, ultrasonic sensors use pins 6–9, and the buzzer is on pin 11.

### Upload & Calibrate
1. Install the required libraries via the Arduino Library Manager
2. Upload the sketch to your board
3. **Keep the walker flat and still** during the initial calibration phase (the system will prompt you via LCD/Serial)
4. Tune `STEP_THRESHOLD_HIGH/LOW` and `TILT_THRESHOLD` in the code if needed, based on your specific sensor mounting and hardware

### Monitor
Open the Serial Monitor at **9600 baud** to see live sensor readings, step/speed tracking, and alert states while testing.

---

## 🔧 Key Parameters (Tunable)

| Parameter | Default | Purpose |
|---|---|---|
| `TILT_THRESHOLD` | 12° (EMA) | Fall-detection sensitivity |
| `DIST_THRESHOLD` | 30 cm | Obstacle-alert distance |
| `WEIGHT_MAX_THRESHOLD` | 500 g | Total weight overload alert |
| `WEIGHT_UNEVEN_DIFF` | 150 g | Left/right imbalance alert |
| `FALL_CONFIRM_COUNT` | 2 | Debounce cycles before confirming a fall |
| `EMA_ALPHA` | 0.5 | Smoothing factor for tilt angle |

---

## 🗺️ Roadmap

- [ ] Wireless alert notification to a caregiver's phone
- [ ] SD card logging for offline gait history
- [ ] Support for additional walker/rollator form factors
- [ ] Refined calibration workflow for varied mounting positions

---

## ⚠️ Status

This project is an active prototype tied to an ongoing patent filing. Implementation details in this repository (the code itself) are shared here; broader invention documentation is not included.

---

## 📄 License

This project's source code is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**⭐ If you found this project useful, consider giving it a star!**

</div>
