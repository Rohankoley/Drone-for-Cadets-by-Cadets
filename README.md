# 🛩️ Drone for Cadets by CADETS  
### ESP32-Based Flight Controller for Educational Drone Training

---

## 📖 Overview
**Drone for Cadets by CADETS** is an educational quadcopter flight controller designed to help students and cadets understand the fundamentals of drone control systems, flight dynamics, and embedded programming.  

It uses an **ESP32 microcontroller**, an **MPU6050 IMU**, and **ESC-controlled motors** to perform stable flight through a dual **PID control loop** — maintaining balance, orientation, and control responsiveness.

This project is part of the **CADETS initiative** to promote practical UAV learning and technical awareness among students.

---

## 🧩 Features
- ESP32-based flight controller with 32-bit dual-core processor  
- MPU6050 IMU (accelerometer + gyroscope) for real-time orientation tracking  
- 6-channel RC receiver input for manual control  
- Dual-loop PID stabilization (Angle + Rate) for Roll, Pitch, and Yaw  
- Complementary filter for smooth and stable angle estimation  
- ESC control via PWM using the `ESP32Servo` library  
- Built-in motor disarm safety system  
- LED-based startup indicator  
- Simple educational structure for learning PID and flight control logic  

---

## 🛠️ Hardware Requirements
| Component | Quantity | Description |
|------------|-----------|-------------|
| ESP32 Dev Board | 1 | Main microcontroller |
| MPU6050 Sensor | 1 | IMU (Gyro + Accelerometer) |
| Brushless Motors | 4 | Drone propulsion system |
| ESCs | 4 | Motor controllers (500 Hz PWM input) |
| Li-Po Battery | 1 | Power source |
| RC Receiver | 1 | 6-Channel radio receiver (Roll, Pitch, Yaw, Throttle, Aux) |
| 36kΩ + 100kΩ resistors | 1 each | Battery voltage divider |
| LED + 300Ω resistor | 1 | Status indicator |

---

## ⚡ Circuit Overview
### Step 1: Build the Brain
- Connect **ESP32** with the **MPU6050** via I²C (`SDA`, `SCL`).
- Add an **LED + 300Ω resistor** to a GPIO pin (example: GPIO15) as a status light.
- Use a **36kΩ and 100kΩ** resistor voltage divider to safely measure battery voltage.

### Step 2: Connect the Muscles
- Connect each **motor** → **ESC** → **ESP32 PWM pin**.  
- Power ESCs using the Li-Po battery.  
- ESC signal pins go to GPIO 13, 12, 14, and 27.

### Step 3: Connect the Eyes (Receiver)
- Attach RC receiver channel outputs to GPIOs 34, 35, 32, 33, 25, and 26.  
- These correspond to Roll, Pitch, Throttle, Yaw, and Aux inputs.

### Step 4: Teach the Brain (Programming)
1. Connect ESP32 to your computer via USB.  
2. Upload **PID tuning code** to find calibration values.  
3. Copy the tuned values into the main code.  
4. Upload the **main quadcopter control code**.

> ⚠️ **Note:** Disconnect all external connections from the ESP32 GPIO pins while uploading code to prevent damage.

---

## ⚙️ Software Details

### Libraries Used
- **Wire.h** – I²C communication with MPU6050  
- **ESP32Servo.h** – PWM signal generation for ESCs  

### Main Control Logic
1. **IMU Reading:**  
   Reads acceleration and gyro data from MPU6050.  
2. **Angle Calculation:**  
   Uses a **complementary filter** to combine gyro and accelerometer readings for stable Roll and Pitch angles.  
3. **Outer PID Loop (Angle):**  
   Calculates the desired rotation rate based on how far the drone tilts from level.  
4. **Inner PID Loop (Rate):**  
   Adjusts motor speeds to achieve that rate, stabilizing the drone.  
5. **Motor Mixing:**  
   Combines throttle and PID outputs to generate motor commands.  
6. **Failsafe:**  
   If throttle is below 1030 µs → motors stop and PID controllers reset.

---

## ⚙️ PID Parameters
| Loop | P | I | D |
|------|---|---|---|
| Angle (Roll/Pitch) | 2.0 | 0.5 | 0.007 |
| Rate (Roll/Pitch) | 0.625 | 2.1 | 0.0088 |
| Rate (Yaw) | 4.0 | 3.0 | 0.0 |

---

## 🧠 How It Works
1. The RC receiver sends stick positions as PWM signals.  
2. The ESP32 translates them into **desired angles** and **rotation rates**.  
3. The MPU6050 provides actual tilt and rotation data.  
4. Two PID controllers calculate corrections.  
5. The controller adjusts motor PWM signals to maintain stability.  
6. The process repeats every 4 milliseconds (~250 Hz).

---

## 🛑 Safety Instructions
- Always **test without propellers first**.  
- Ensure **ESCs are properly calibrated**.  
- Perform first tests at low throttle in a wide open area.  
- Keep a **safe distance** while arming and testing.  
- Power down immediately if instability is observed.

---

## 🧰 Future Improvements
- Replace complementary filter with a **Kalman filter** for better precision.  
- Add **barometer or ultrasonic sensor** for altitude hold.  
- Implement **auto-level and GPS-assisted flight**.  
- Add **Bluetooth or Wi-Fi telemetry** for in-flight tuning.

---

## ⚖️ License
```
THERE IS NO WARRANTY FOR THE SOFTWARE, TO THE EXTENT PERMITTED BY APPLICABLE LAW.  
THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESSED OR IMPLIED,  
INCLUDING, BUT NOT LIMITED TO, WARRANTIES OF MERCHANTABILITY OR FITNESS FOR A PARTICULAR PURPOSE.  
USE AT YOUR OWN RISK.
```

---

## ✍️ Credits
Project Name: **Drone for Cadets by CADETS**  
Developed by: **Rohan Koley**  
Mentored under: **CADETS Initiative**  
Version: **Beta v1**

---
