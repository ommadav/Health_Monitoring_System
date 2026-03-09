# 🩺 Arduino Health Monitoring System

![Arduino](https://img.shields.io/badge/Arduino-Uno-blue)
![Language](https://img.shields.io/badge/Language-C++-orange)
![Platform](https://img.shields.io/badge/Platform-Arduino%20IDE-green)
![Status](https://img.shields.io/badge/Project-Completed-success)

A simple **Health Monitoring System** built using **Arduino Uno** that measures **heart rate and body temperature** and displays the readings on an **OLED display**.

This project demonstrates how **biomedical sensors can be interfaced with microcontrollers** for real-time monitoring.

---

# 📌 Project Overview

The system integrates two sensors:

* **KY-039 Heartbeat Sensor** – measures pulse rate
* **DS18B20 Temperature Sensor** – measures body temperature

Both sensors send data to the **Arduino Uno**, which processes the readings and displays them on an **SSD1306 OLED display**.

This project is useful for:

* Embedded systems learning
* Biomedical instrumentation basics
* Arduino sensor interfacing
* Academic mini projects

---

# ⚙️ Features

✔ Real-time **Heart Rate Monitoring**
✔ **Body Temperature Measurement**
✔ **OLED Display Output**
✔ Low-cost hardware components
✔ Beginner-friendly Arduino implementation

---

# 🧰 Hardware Components

| Component                  | Quantity |
| -------------------------- | -------- |
| Arduino Uno                | 1        |
| KY-039 Heartbeat Sensor    | 1        |
| DS18B20 Temperature Sensor | 1        |
| OLED Display (SSD1306 I2C) | 1        |
| 4.7kΩ Resistor             | 1        |
| Breadboard                 | 1        |
| Jumper Wires               | Several  |

---

# 🔌 Circuit Diagram

![Circuit Diagram](circuit/circuit_image.png)

### Pin Connections

#### KY-039 Heartbeat Sensor

| Sensor Pin | Arduino Pin |
| ---------- | ----------- |
| VCC        | 5V          |
| GND        | GND         |
| Signal     | A0          |

---

#### DS18B20 Temperature Sensor

| Sensor Pin | Arduino Pin |
| ---------- | ----------- |
| VCC        | 5V          |
| GND        | GND         |
| DATA       | D2          |

⚠ A **4.7kΩ pull-up resistor** must be connected between **VCC and DATA**.

---

#### OLED Display (I2C)

| OLED Pin | Arduino Pin |
| -------- | ----------- |
| VCC      | 5V          |
| GND      | GND         |
| SDA      | A4          |
| SCL      | A5          |

---

# 🧠 Block Diagram

![Block Diagram](images/Health Monitoring System.pdf)

### System Flow

1. **KY-039 sensor** detects heartbeat using infrared light.
2. **DS18B20 sensor** measures body temperature digitally.
3. **Arduino Uno processes the data** from sensors.
4. Processed data is sent to the **OLED display**.
5. User can monitor **pulse rate and temperature in real time**.

---

# 📊 Example Output

```
Heart Rate : 74 BPM
Temperature : 36.6 °C
```

---

# 📚 Software Requirements

Install the following libraries in **Arduino IDE**:

* Adafruit GFX
* Adafruit SSD1306
* OneWire
* DallasTemperature

Install via:

Arduino IDE → Sketch → Include Library → Manage Libraries

---

# 📂 Project Structure

```
Health-Monitoring-System
│
├── code
│   └── health_monitor.ino
│
├── circuit
│   └── circuit_diagram.png
│
├── images
│   └── block_diagram.png
│
├── images
│   └── project_setup.jpg
│
└── README.md
```

---

# 🚀 How to Run the Project

1. Clone the repository

```
git clone https://github.com/yourusername/Health-Monitoring-System.git
```

2. Open the `.ino` file in **Arduino IDE**

3. Install required libraries

4. Connect the hardware according to the **circuit diagram**

5. Upload the code to **Arduino Uno**

6. Place your finger on the **KY-039 sensor**

7. View heart rate and temperature on the **OLED display**

---

# 💡 Applications

* Patient monitoring systems
* Biomedical electronics projects
* IoT healthcare prototypes
* Embedded systems learning

---

# 🔮 Future Improvements

* Add **WiFi monitoring using ESP8266 / ESP32**
* Send data to **Blynk or IoT dashboard**
* Add **SpO₂ sensor (MAX30102)**
* Add **buzzer alert for abnormal readings**
* Cloud data logging

---

# 👨‍💻 Author

**Om Madav**
---

# 📄 License

This project is licensed under the **MIT License**.
