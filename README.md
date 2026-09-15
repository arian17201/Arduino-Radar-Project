# Arduino Radar System

A real-time radar system built with **Arduino, HC-SR04 Ultrasonic Sensor, Servo Motor, Buzzer, and Processing IDE**. The ultrasonic sensor scans the surrounding area by rotating through a range of angles, measures the distance of detected objects, and sends the angle-distance data to a Processing application for visualization.

## 📌 Project Overview

The system works like a simple radar:

1. A **Servo Motor** rotates the ultrasonic sensor from **15° to 165°**.
2. The **HC-SR04 Ultrasonic Sensor** measures the distance of objects at each angle.
3. Arduino sends the measured **angle and distance** to the computer through Serial communication.
4. **Processing IDE** receives the serial data and displays a real-time radar interface.
5. A **Buzzer** turns on when an object is detected within **10 cm**.

The radar visualization displays objects up to **40 cm** away.

---

## ✨ Features

- 🔄 Servo-controlled scanning from **15° to 165°**
- 📡 Ultrasonic distance measurement
- 🖥️ Real-time radar visualization using Processing
- 📏 Detection range displayed up to **40 cm**
- 🔔 Buzzer warning for objects within **10 cm**
- 🔌 Serial communication at **9600 baud**
- ↔️ Continuous left-to-right and right-to-left scanning
- 🎯 Displays detected object's angle and distance

---

## 🧰 Hardware Components

| Component | Quantity | Purpose |
|---|---:|---|
| Arduino Uno | 1 | Main microcontroller |
| HC-SR04 Ultrasonic Sensor | 1 | Measures object distance |
| Servo Motor | 1 | Rotates the ultrasonic sensor |
| Buzzer | 1 | Provides close-object warning |
| Jumper Wires | As required | Connections |
| Breadboard | 1 | Prototyping |
| USB Cable | 1 | Arduino programming and serial communication |

---

## 🔌 Pin Configuration

| Component | Arduino Pin | Function |
|---|---:|---|
| HC-SR04 Trig | D8 | Ultrasonic trigger |
| HC-SR04 Echo | D9 | Ultrasonic echo |
| Buzzer | D11 | Warning output |
| Servo Signal | D7 | Servo control |

### HC-SR04 Power

- **VCC → 5V**
- **GND → GND**

### Servo Power

- **Signal → D7**
- **VCC → 5V**
- **GND → GND**

> For larger/high-current servos, use an appropriate external 5V supply and connect the supply ground to Arduino GND.

---

## 🧠 How It Works

### 1. Servo Scanning

The servo continuously rotates between:

```text
15° → 165° → 15° → 165° → ...
```

At each angle, the Arduino waits briefly and takes an ultrasonic distance measurement.

### 2. Distance Measurement

The HC-SR04 sends an ultrasonic pulse and measures the time taken for the echo to return.

The project calculates distance using:

```text
Distance = (Echo Time × 0.034) / 2
```

The division by 2 is required because the ultrasonic wave travels to the object and then returns to the sensor.

### 3. Serial Data

Arduino sends data in the following format:

```text
angle,distance.
```

Example:

```text
90,25.
```

Here:

- `90` = sensor angle in degrees
- `25` = measured distance in centimeters
- `.` = delimiter used by Processing to identify the end of a data packet

### 4. Buzzer Alert

The buzzer activates when:

```text
0 < distance ≤ 10 cm
```

Otherwise, the buzzer remains off.

### 5. Processing Visualization

Processing reads the serial data and draws:

- Radar arcs
- Angle lines
- Green scanning line
- Red detected-object indicator
- Current angle
- Current distance
- In-range/out-of-range status

Objects farther than **40 cm** are treated as out of the displayed radar range.

---

## 📁 Project Structure

```text
Arduino Radar Project/
│
├── rdr/
│   └── rdr.ino
└── processing.pde
```

### Main Files

**`rdr.ino`**

Contains the Arduino program for:

- Servo control
- HC-SR04 distance measurement
- Buzzer control
- Serial data transmission

**`processing.pde`**

Contains the Processing program for:

- Serial communication
- Radar drawing
- Object visualization
- Angle/distance display

---

## 💻 Software Requirements

### Arduino

- Arduino IDE
- Servo library
- Compatible Arduino board
- USB connection to the computer

### Processing

- Processing IDE
- `processing.serial.*` library included with Processing
- Available serial/COM port

---

## 🚀 Installation and Setup

### Step 1 — Assemble the Circuit

Connect the components according to the pin configuration:

```text
HC-SR04 Trig  → Arduino D8
HC-SR04 Echo  → Arduino D9
Buzzer        → Arduino D11
Servo Signal  → Arduino D7
```

Connect all required VCC and GND connections.

### Step 2 — Upload the Arduino Code

1. Open `rdr/rdr.ino` in Arduino IDE.
2. Select your Arduino board.
3. Select the correct COM port.
4. Upload the code.

### Step 3 — Identify the Serial Port

After uploading, check which COM port your Arduino is using.

The Processing code currently contains:

```java
myPort = new Serial(this, "COM6", 9600);
```

If your Arduino uses a different port, change `COM6`.

For example:

```java
myPort = new Serial(this, "COM3", 9600);
```

### Step 4 — Close Arduino Serial Monitor

Before running Processing, close the Arduino Serial Monitor/Serial Plotter if it is open. Only one application should normally use the serial port at a time.

### Step 5 — Run Processing

1. Open `processing.pde` in Processing IDE.
2. Set the correct COM port.
3. Make sure the baud rate remains **9600**.
4. Run the Processing sketch.
5. The radar visualization should appear in full-screen mode.

---

## 📊 Radar Display

The Processing interface provides a visual representation similar to:

```text
                 90°
                  |
            \     |     /
             \    |    /
          120°\   |   /60°
               \  |  /
                \ | /
                 \|/
                  ●
                Sensor
```

The green line represents the current scanning angle.

A red line/marker represents a detected object within the displayed range.

---

## ⚙️ Important Settings

### Buzzer Detection Threshold

The Arduino code uses:

```cpp
const int distanceThreshold = 10;
```

This means the buzzer activates for objects at **10 cm or closer**.

To change it to 15 cm:

```cpp
const int distanceThreshold = 15;
```

### Radar Display Range

The Processing application displays objects when:

```cpp
if(iDistance < 40)
```

Therefore, the visualized radar range is approximately **40 cm**.

### Serial Baud Rate

Both Arduino and Processing use:

```text
9600 baud
```

Arduino:

```cpp
Serial.begin(9600);
```

Processing:

```java
myPort = new Serial(this, "COM6", 9600);
```

Both values must match.

---

## 🔧 Troubleshooting

### Processing says the COM port does not exist

Check the Arduino IDE:

**Tools → Port**

Then replace `COM6` in `processing.pde` with the correct port.

### Processing does not receive data

Check:

- Arduino is connected.
- Correct COM port is selected.
- Arduino Serial Monitor is closed.
- Both programs use **9600 baud**.
- Arduino code has been successfully uploaded.

### Servo does not rotate

Check:

- Servo signal wire is connected to **D7**.
- Servo has sufficient power.
- GND is connected correctly.
- The Servo library is available.

### Ultrasonic sensor gives incorrect distances

Check:

- Trig → D8
- Echo → D9
- VCC → 5V
- GND → GND
- Sensor is facing the object directly.
- Objects are within the sensor's practical operating range.

### Buzzer stays on

The current project intentionally activates the buzzer for distances:

```text
0–10 cm
```

Move the object farther away or increase/decrease `distanceThreshold` according to your requirements.

---

## 🔄 System Flow

```text
          START
            │
            ▼
     Initialize Arduino
            │
            ▼
     Initialize Servo
            │
            ▼
    Rotate Servo to Angle
            │
            ▼
   Trigger Ultrasonic Sensor
            │
            ▼
     Calculate Distance
            │
            ▼
     Send Angle + Distance
            │
            ├──────────────► Processing
            │                    │
            │                    ▼
            │             Draw Radar Display
            │
            ▼
    Distance ≤ 10 cm?
        /          \
      YES           NO
       │             │
       ▼             ▼
  Buzzer ON      Buzzer OFF
       │             │
       └──────┬──────┘
              ▼
       Continue Scanning
```

---

## 🔬 Technical Specifications

| Parameter | Value |
|---|---|
| Servo scan range | 15°–165° |
| Ultrasonic sensor | HC-SR04 |
| Serial baud rate | 9600 |
| Buzzer threshold | ≤ 10 cm |
| Processing display range | < 40 cm |
| Servo control pin | D7 |
| Ultrasonic Trig | D8 |
| Ultrasonic Echo | D9 |
| Buzzer pin | D11 |

---

## 🌟 Possible Future Improvements

- Increase the ultrasonic detection range.
- Add multiple ultrasonic sensors for wider coverage.
- Add an LCD/OLED display.
- Store detected-object data.
- Add object tracking.
- Improve radar graphics and animations.
- Add adjustable detection thresholds from the Processing interface.
- Add wireless communication using ESP32, Bluetooth, or Wi-Fi.
- Add a camera for visual object identification.
- Use a rechargeable battery for portable operation.
- Add collision-avoidance functionality for robotics applications.

---

## 🎯 Applications

This project can be adapted for:

- Obstacle detection
- Robotics
- Smart vehicle systems
- Security monitoring
- Proximity warning systems
- Educational radar demonstrations
- Autonomous navigation experiments

---

## 👥 Project Information

**Project:** Radar System with Arduino  
**Group:** Group 07

The project consists of an Arduino-based sensing system and a Processing-based computer visualization interface.

---

## 📜 License

This project is intended for educational and academic purposes. You may modify and extend the source code for learning, experimentation, and non-commercial projects.

---

## 🙏 Acknowledgements

- Arduino community and documentation
- Processing community
- HC-SR04 ultrasonic sensor documentation
- Servo motor and Arduino libraries
