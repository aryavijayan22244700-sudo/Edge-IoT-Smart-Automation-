# 🔌 Edge-IoT-Smart-Automation

> A hands-on learning journey through Edge Computing, IoT systems, and Smart Automation — from development environment setup to AI/ML integration.

---

## 📋 Table of Contents

- [About the Project](#about-the-project)
- [Learning Progress](#learning-progress)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)

---

## About the Project

This repository documents a structured, day-by-day progression through the foundations of Edge IoT and Smart Automation — covering hardware tooling, firmware development, version control, and machine learning concepts applied to embedded/edge systems.

---

## Learning Progress

### Day 1 — Version Control & GitHub Basics
- Installed **Git** and configured the local environment
- Learned GitHub fundamentals: repositories, commits, push/pull
- Set up GitHub account and connected it with the local Git installation

---

### Day 2 — Portfolio Deployment
- Built and deployed a **personal portfolio website**
- Learned the deployment workflow using GitHub Pages / hosting platform
- Practiced end-to-end: code → commit → deploy → live site

---

### Day 3 — Hardware & Firmware Tooling Setup
- Installed **KiCad** — open-source EDA tool for PCB schematic design
- Installed and completed setup of **PlatformIO IDE** (VSCode Extension)
  - Configured for embedded development workflow
  - Verified board support and build toolchain

---

### Day 4 — Introduction to Machine Learning
Attended a session covering core ML concepts relevant to intelligent IoT and edge systems:

- **What is ML** — Computers learn patterns from data without being explicitly programmed with rules
- **Three Types of ML:**
  - 🔵 **Supervised Learning** — Labeled training data; model learns input-output mappings
  - 🔵 **Unsupervised Learning** — Finds hidden patterns in unlabeled data
  - 🔵 **Reinforcement Learning** — Agent learns through reward/penalty feedback loops
- **ML Pipeline:**
  ```
  Collect → Clean → Train → Evaluate → Deploy
  ```
- **Evaluation** — Always test on unseen (held-out) test data to measure real-world performance; avoid data leakage
- **Ethics & Responsibility** — Fairness, transparency, and responsible AI matter as much as model accuracy

---

### Day 5 — Applied Machine Learning: Loan Prediction Classifier

Completed a hands-on workshop on Applied ML — built and trained a **Decision Tree Classifier** in Google Colab to predict loan approval outcomes.

#### 🧠 Core Concepts Learned

**How Decision Trees Work**
- Instead of writing explicit formulas, the model asks a series of yes/no questions to divide data into clean groups
- **Root Node** — the very first question the tree asks to split the data
- **Leaf Nodes** — the final endpoints that provide the prediction (e.g. Approved / Denied)

**Entropy & Node Purity**
- **High Entropy** = a 50/50 mix of outcomes → the model is uncertain
- **Zero Entropy** = a group with only one outcome type → the model is fully certain
- The tree always picks the question that creates the **cleanest, most sorted groups** (maximum information gain)

---

#### 🛠️ Workshop Pipeline

```
Load & Clean → Encode & Split → Train & Plot → Interactive Test
```

**Step 1 — Import & Load Data**
```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier

url = "https://raw.githubusercontent.com/shrikant-temburwar/Loan-Prediction-Dataset/master/train.csv"
df = pd.read_csv(url)
df = df.dropna()
```

**Step 2 — Encode & Split (80% train / 20% test)**
```python
df['Gender'] = df['Gender'].map({'Male': 1, 'Female': 0})
df['Married'] = df['Married'].map({'Yes': 1, 'No': 0})
df['Education'] = df['Education'].map({'Graduate': 1, 'Not Graduate': 0})
df['Loan_Status'] = df['Loan_Status'].map({'Y': 1, 'N': 0})
# ... (other categorical encodings)

features = ['Gender', 'Married', 'Education', 'Self_Employed',
            'ApplicantIncome', 'LoanAmount', 'Credit_History', 'Property_Area']
X = df[features]
y = df['Loan_Status']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

**Step 3 — Train & Evaluate**
```python
model = DecisionTreeClassifier(max_depth=4, random_state=42)
model.fit(X_train, y_train)
accuracy = model.score(X_test, y_test)
print(f"📈 Test Accuracy: {accuracy * 100:.2f}%")
```

**Step 4 — Custom Prediction**
```python
my_custom_profile = [[1, 0, 1, 1, 6000, 150, 1.0, 1]]
my_custom_df = pd.DataFrame(my_custom_profile, columns=features)
prediction = model.predict(my_custom_df)
# Output: 🎉 AI Decision: Loan APPROVED!
```

**Optional — Visualize the Decision Tree**
```python
from sklearn.tree import plot_tree
import matplotlib.pyplot as plt

plt.figure(figsize=(20, 12))
plot_tree(model, feature_names=features,
          class_names=['Denied', 'Approved'],
          filled=True, rounded=True, fontsize=12)
plt.show()
```

---

#### 📌 Key Takeaways

| Concept | Insight |
|---------|---------|
| Decision Trees | Learn by splitting data with yes/no questions |
| Entropy | Measures disorder; lower = purer, better split |
| `train_test_split` | Keeps test data unseen to evaluate real performance |
| `max_depth` | Limits tree complexity to prevent overfitting |
| Categorical Encoding | Text labels must be mapped to numbers before training |
| Cell Execution Order | Jupyter notebooks must be run top-to-bottom (NameError otherwise) |

> 🔗 Workshop Reference: [Applied ML — Loan Prediction Classifier](https://edge-iot-intern.web.app/loan%20prediction.html)

---

### Day 6 — Microcontrollers, ESP32 Architecture & Hands-on Hardware

Completed the **USIZO Academy: Introduction to Microcontrollers & Embedded Systems** interactive lab, and had a first hands-on session physically connecting an **ESP32** with a sensor module and programming it via PlatformIO in VSCode.

#### 🔩 Hardware Used
| Component | Description |
|-----------|-------------|
| **ESP32-WROOM-32** | Dual-core 240MHz MCU with onboard Wi-Fi & Bluetooth |
| **MPU6050** | 6-axis IMU sensor module (Accelerometer + Gyroscope) via I2C |
| Jumper Wires (Yellow) | SDA / SCL connections between ESP32 and MPU6050 |
| USB-C Cable | Power & serial flashing connection to laptop |
| PlatformIO (VSCode) | Firmware development, build & upload environment |

---

#### 🧠 Concepts Covered

**MCU vs MPU**
- **Microcontroller (MCU)** — All-in-one single chip: CPU + RAM + Flash + I/O peripherals. Examples: ESP32, Arduino ATMega328, STM32
- **Microprocessor (MPU)** — Raw CPU; depends on external chips for RAM and storage. Examples: Intel Core i5, Apple M-Series
- MCUs are optimized for **low-power, low-cost, dedicated control**; MPUs for raw computational throughput

| Feature | MCU | MPU |
|---------|-----|-----|
| Integration | Single chip (all-in-one) | System of chips |
| Avg. Cost | $1 – $5 | $50 – $400 |
| Power Draw | Microwatts – Milliwatts | 10W – 150W |
| Application | Thermostat, sensor node | Laptop, desktop |

---

**Cores, Threads & FreeRTOS**
- **CPU Core** — Physical engine that executes instructions; ESP32 is a **dual-core** chip (Core 0 + Core 1)
- **Thread / Task** — A self-contained software function running concurrently (e.g. reading a sensor, running Wi-Fi, blinking an LED — all as separate tasks)
- **FreeRTOS** — Real-Time OS kernel that guarantees critical tasks run **deterministically** with strict priority deadlines
- **Preemption** — FreeRTOS instantly halts low-priority tasks when a high-priority task needs to execute

```
Task Priority Example:
  Priority 3 (High)   → Task_Temp_Sensor()
  Priority 2 (Medium) → Task_WiFi_Transmit()
  Priority 1 (Low)    → Task_Blink_Status_LED()
```

---

**GPIO — General Purpose Input/Output**
- Physical pins on the MCU configurable at runtime via code
- **Output mode** — Write HIGH (3.3V) or LOW (0V) to drive LEDs, relays, displays
- **Input mode** — Read voltage levels to detect button presses or sensor states
- **Pull-up / Pull-down resistors** — Prevent floating pin noise (an unconnected input pin acts like an antenna, randomly fluctuating between 0 and 1)

```cpp
// Configuring Pin 2
pinMode(2, OUTPUT);
digitalWrite(2, HIGH);  // LED ON

pinMode(4, INPUT_PULLUP);
int state = digitalRead(4);  // Read button
```

---

**ADC & DAC Conversion**
- **ADC (Analog-to-Digital)** — Converts real-world continuous voltage into binary numbers; ESP32 has **12-bit ADC** (0 to 4095 range)
- **DAC (Digital-to-Analog)** — Converts binary code back to analog voltage output
- **Quantization Error** — Step-like digital approximation of a smooth analog signal; higher bit depth = smaller error

---

**Serial Communication Protocols**

| Protocol | Clock | Wires | Devices | Speed |
|----------|-------|-------|---------|-------|
| **UART** | Async (baud rate) | 2 (TX, RX) | Point-to-point (2) | Medium |
| **I2C** | Sync (SCL) | 2 (SDA, SCL) | Up to 127 (addressed) | Medium |
| **SPI** | Sync (SCK) | 4 (MOSI, MISO, SCK, CS) | Multiple (CS pins) | High |

> The **MPU6050** sensor communicates over **I2C** using SDA and SCL lines — connected to the ESP32 via yellow jumper wires as seen in the session.

---

**ESP32-WROOM Architecture**
- **Core**: Tensilica Xtensa Dual-Core 32-bit LX6 @ up to 240 MHz
- **Wireless**: 2.4 GHz Wi-Fi (802.11 b/g/n) + Bluetooth v4.2 / BLE — all on-chip
- **Memory**: 520 KB internal SRAM + 4MB/8MB external SPI Flash
- **Peripherals**: Capacitive touch, 12-bit ADCs, 8-bit DACs, PWM, SPI, I2C, UART
- **ULP co-processor**: Handles sensor polling during deep sleep to minimize power draw
- **Pin Multiplexing**: Each GPIO pin supports multiple hardware functions configurable in firmware

---

#### ⚙️ Hands-on Activity
- Physically connected **ESP32** to laptop via USB-C for power and serial flashing
- Wired **MPU6050** (IMU sensor) to ESP32 using **I2C** jumper connections (SDA + SCL)
- Opened firmware project in **PlatformIO + VSCode** — observed `setup()` and `loop()` structure in C++
- Explored ESP32 pin multiplexing — understanding which pins support ADC, I2C, SPI, and UART simultaneously

---

#### 📌 Key Takeaways

| Concept | Insight |
|---------|---------|
| MCU vs MPU | MCU = all-in-one low-power chip; MPU = high-performance multi-chip system |
| FreeRTOS | Enables true concurrent task execution with deterministic priority scheduling |
| GPIO | Fully software-configurable pins; always use pull-up/down to avoid floating noise |
| ADC Resolution | 12-bit on ESP32 gives 4096 discrete steps across 0–3.3V range |
| I2C Protocol | 2-wire addressed bus — ideal for sensors like MPU6050 on shared lines |
| Pin Multiplexing | Each ESP32 pin can serve multiple hardware roles defined in firmware |
| Clock Speed | Higher MHz = faster execution but higher power draw and heat |

> 🔗 Session Reference: [USIZO Academy — Microcontrollers & ESP32 Introduction](https://edge-iot-intern.web.app/)

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| Git & GitHub | Version control and collaboration |
| KiCad | PCB / schematic design |
| PlatformIO (VSCode) | Embedded firmware development |
| Python / ML Frameworks | Machine learning and data processing |
| ESP32-WROOM-32 | Target MCU for embedded programming |
| MPU6050 | IMU sensor (I2C — accelerometer + gyroscope) |
| FreeRTOS | Real-time OS for concurrent task scheduling on ESP32 |

---

## Getting Started

```bash
# Clone this repository
git clone https://github.com/<your-username>/Edge-IoT-Smart-Automation.git

# Navigate into the project
cd Edge-IoT-Smart-Automation
```

> PlatformIO projects can be opened directly in VSCode with the PlatformIO extension installed.

---

Day 7 — Local IoT Integration & Web Server Architecture
Configured the ESP32 to host a localized web server, enabling real-time bidirectional data flow between an embedded system and an interactive web dashboard over a shared local area network (LAN).

🔩 Hardware Used
* **ESP32-WROOM-32** (Acting as Wi-Fi Access Point/Station & Web Server)
* **MPU6050** (6-axis IMU tracking live motion data)
* **LED & Current-Limiting Resistor** (Target actuator for hardware control)
* **Mobile Hotspot** (Local network gateway)

🧠 Concepts Covered
### Local Web Servers on Microcontrollers
* **HTTP Protocol in Embedded Systems:** The ESP32 listens for incoming HTTP requests on Port 80. When a client (e.g., a smartphone browser) hits the ESP32’s local IP address, the MCU serves static HTML/CSS/JS resources directly from its internal memory.
* **Pulse-Width Modulation (PWM):** Instead of simple binary states (ON/OFF), the ESP32 utilizes hardware PWM timers to rapidly oscillate digital output pins. By adjusting the **Duty Cycle** via an interactive browser slider, we smoothly modulated the analog brightness of a physical LED.

### Asynchronous Data Flow
* **AJAX & Fetch API:** To prevent web page reloads every time a hardware state updates, asynchronous HTTP requests handle background telemetry (MPU6050 data stream) and control endpoints dynamically.

⚙️ Hands-on Activity
* Connected the ESP32 to a local mobile hotspot network and exposed its assigned local IP address via the Serial Monitor.
* Served a custom, responsive web dashboard containing an interactive slider element directly from the microcontroller.
* Mapped the slider UI value to the ESP32's PWM driver to control external LED brightness in real-time without latency.

📌 Key Takeaways
| Concept | Insight |
| :--- | :--- |
| **Embedded Web Server** | Allows any device with a browser on the same network to interact with hardware without specialized apps. |
| **PWM Duty Cycle** | Controls power delivery; higher duty cycle = higher average voltage = brighter LED. |
| **Asynchronous Control** | Avoids blocking the main execution loop, keeping sensor polling active while serving web pages. |

---

Day 8 — Global Cloud Deployment & Industrial Automation via Relays
Scaled the localized network prototype into a production-grade, globally accessible Internet of Things (IoT) infrastructure capable of driving high-voltage home/industrial appliances from anywhere in the world.



🔩 Hardware Used
* **ESP32-WROOM-32** (Connected to global cloud network)
* **Relay Module** (Optocoupler-isolated mechanical switch acting as a bridge between low-power logic and high-voltage loads)
* **Utility Light Bulb & AC Mains** (High-voltage electrical load)

🧠 Concepts Covered
### Scaling from Local to Cloud (Global Architecture)
* **Decoupled Architecture:** Moving past network boundary limitations by hosting the web application interface on a production-ready cloud deployment platform instead of hosting it locally inside the ESP32's limited SRAM.
* **Global Pipeline Security:** Establishing a secure, cloud-routed data pathway between the globally hosted client application and the physical ESP32 endpoint.

### High-Voltage Isolation and Actuation
* **Relay Physics:** Microcontrollers operate at fragile 3.3V/5V logic levels which would immediately destroy themselves if exposed to household AC mains (110V/230V). 
* **Optoisolation:** The relay module isolates the sensitive ESP32 logic pins from high-voltage spikes by translating digital commands into internal infrared light signals that flip the physical mechanical switch.
* Here is the polished and formatted content for **Day 7** and **Day 8**, structured to match the design, tone, tables, and markdown styling of your existing `README.md` file.

You can copy and paste the markdown block below directly into your file right under the Day 6 section, or update your complete repository document with it.

---

```markdown
Day 7 — Local IoT Integration & Web Server Architecture
Configured the ESP32 to host a localized web server, enabling real-time bidirectional data flow between an embedded system and an interactive web dashboard over a shared local area network (LAN).

🔩 Hardware Used
* **ESP32-WROOM-32** (Acting as Wi-Fi Access Point/Station & Web Server)
* **MPU6050** (6-axis IMU tracking live motion data)
* **LED & Current-Limiting Resistor** (Target actuator for hardware control)
* **Mobile Hotspot** (Local network gateway)

🧠 Concepts Covered
### Local Web Servers on Microcontrollers
* **HTTP Protocol in Embedded Systems:** The ESP32 listens for incoming HTTP requests on Port 80. When a client (e.g., a smartphone browser) hits the ESP32’s local IP address, the MCU serves static HTML/CSS/JS resources directly from its internal memory.
* **Pulse-Width Modulation (PWM):** Instead of simple binary states (ON/OFF), the ESP32 utilizes hardware PWM timers to rapidly oscillate digital output pins. By adjusting the **Duty Cycle** via an interactive browser slider, we smoothly modulated the analog brightness of a physical LED.

### Asynchronous Data Flow
* **AJAX & Fetch API:** To prevent web page reloads every time a hardware state updates, asynchronous HTTP requests handle background telemetry (MPU6050 data stream) and control endpoints dynamically.

⚙️ Hands-on Activity
* Connected the ESP32 to a local mobile hotspot network and exposed its assigned local IP address via the Serial Monitor.
* Served a custom, responsive web dashboard containing an interactive slider element directly from the microcontroller.
* Mapped the slider UI value to the ESP32's PWM driver to control external LED brightness in real-time without latency.

📌 Key Takeaways
| Concept | Insight |
| :--- | :--- |
| **Embedded Web Server** | Allows any device with a browser on the same network to interact with hardware without specialized apps. |
| **PWM Duty Cycle** | Controls power delivery; higher duty cycle = higher average voltage = brighter LED. |
| **Asynchronous Control** | Avoids blocking the main execution loop, keeping sensor polling active while serving web pages. |

---

Day 8 — Global Cloud Deployment & Industrial Automation via Relays
Scaled the localized network prototype into a production-grade, globally accessible Internet of Things (IoT) infrastructure capable of driving high-voltage home/industrial appliances from anywhere in the world.



🔩 Hardware Used
* **ESP32-WROOM-32** (Connected to global cloud network)
* **Relay Module** (Optocoupler-isolated mechanical switch acting as a bridge between low-power logic and high-voltage loads)
* **Utility Light Bulb & AC Mains** (High-voltage electrical load)

🧠 Concepts Covered
### Scaling from Local to Cloud (Global Architecture)
* **Decoupled Architecture:** Moving past network boundary limitations by hosting the web application interface on a production-ready cloud deployment platform instead of hosting it locally inside the ESP32's limited SRAM.
* **Global Pipeline Security:** Establishing a secure, cloud-routed data pathway between the globally hosted client application and the physical ESP32 endpoint.

### High-Voltage Isolation and Actuation
* **Relay Physics:** Microcontrollers operate at fragile 3.3V/5V logic levels which would immediately destroy themselves if exposed to household AC mains (110V/230V). 
* **Optoisolation:** The relay module isolates the sensitive ESP32 logic pins from high-voltage spikes by translating digital commands into internal infrared light signals that flip the physical mechanical switch.


```

```
              [ 3.3V Low-Power Logic ]
                        │
                        ▼
          ┌───────────────────────────┐
          │   ESP32 Microcontroller   │
          └─────────────┬─────────────┘
                        │ (Digital Signal)
                        ▼
          ┌───────────────────────────┐
          │   Optocoupler Isolation   │  ◀─── Protects MCU from high voltage
          └─────────────┬─────────────┘
                        │ (Internal Light Signal)
                        ▼
          ┌───────────────────────────┐
          │     Mechanical Relay      │
          └─────────────┬─────────────┘
                        │ (Physical Switch Connection)
                        ▼
             [ 230V AC Mains Utility ]

```

```

⚙️ Hands-on Activity
* Migrated the local web dashboard code, deploying a permanent production-grade frontend website accessible globally via the public internet.
* Integrated a Relay Module with the ESP32 development board using proper pin mapping configurations.
* Wired a standard utility light bulb through the relay's **Normally Open (NO)** and **Common (COM)** terminals.
* Executed end-to-end cloud actuation: Clicking a digital toggle button on the globally deployed live site instantly triggered the physical mechanical relay switch miles away.

📌 Key Takeaways
| Concept | Insight |
| :--- | :--- |
| **Global Deployment** | Decoupling the frontend from local hardware eliminates network boundaries, allowing true remote management. |
| **Relay Actuation** | Allows low-power microcontrollers to safely switch heavy AC/DC industrial loads. |
| **Asynchronous Protocols** | Managing network latencies is vital to ensure real-time responsiveness across global server routes. |

```

## 📌 Notes

This repository is actively updated as the program progresses. Each day's work is documented with key learnings and setup steps for reproducibility.

---

*Last updated: Day 6 — Microcontrollers, ESP32 Architecture & Hands-on Hardware*


