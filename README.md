## ⚡ Core Operational Features

### 1. Smart Gating Control
*   **Entrance Gate (`Servo S1`):** Monitors vehicles approaching the entry point via a 3-pin ultrasonic distance sensor. The gate swings open 90 degrees (`OPEN_ANGLE`) when a vehicle is within **30 cm** (`GATE_THRESHOLD`) and returns to 0 degrees (`CLOSE_ANGLE`) when the vehicle clears the boundary.
*   **Exit Gate (`Servo S2`):** Operated independently with its own 3-pin ultrasonic sensor. It uses a **30 cm** activation threshold to open the exit barrier for outgoing vehicles.

### 2. Real-Time Occupancy Tracking
*   Two **HC-SR04 (4-pin) Ultrasonic Sensors** are mounted directly over the designated parking spots (**Slot 1** and **Slot 2**).
*   If a parked vehicle is detected within a distance of **40 cm** (`SLOT_THRESHOLD`):
    *   The respective spot status is set to **NA** (Not Available) on the LCD display.
    *   The dedicated red/status LED for that slot illuminates to give drivers a quick visual cue.
*   If the sensor returns a distance greater than 40 cm, the slot is marked as **A** (Available) and the indicator LED is deactivated.

### 3. Eco-Power Saving LCD Control (Green Computing)
*   Equipped with a **PIR Motion Sensor** situated near the control console.
*   To prevent unnecessary energy consumption, the system switches off the 16x2 LCD display backlight (`LCD_BACKLIGHT`) when no movement or driver is nearby.
*   Once a driver or vehicle approaches the console, the PIR sensor detects motion and automatically turns the backlight **ON** instantly.

### 4. Hardware Diagnostics Terminal
*   The system broadcasts real-time operations telemetry over the hardware Serial interface at **9600 Baud**.
*   This telemetry allows engineers to monitor values for: PIR sensor detection, entrance sensor readings, exit sensor readings, and exact occupancy distances of Slot 1 & 2.

---

## 🔌 Hardware Wiring & Pin Mapping

Here is the exact pinout connection configuration utilized in the system simulation layout:

| Component | Pin Type | Arduino Pin | Description |
| :--- | :--- | :--- | :--- |
| **Entry Servo (S1)** | PWM | `Pin 10` | Controls entry gate rotation angle (0° - 90°) |
| **Exit Servo (S2)** | PWM | `Pin 9` | Controls exit gate rotation angle (0° - 90°) |
| **Entry Ultrasonic Sensor** | I/O (3-Pin) | `Pin 6` | Detects incoming vehicles (<30cm threshold) |
| **Exit Ultrasonic Sensor** | I/O (3-Pin) | `Pin 13` | Detects outgoing vehicles (<30cm threshold) |
| **Slot 1 Trigger (Trig 1)** | Digital Output | `Pin 7` | Sends ultrasonic pulse for Slot 1 |
| **Slot 1 Echo (Echo 1)** | Digital Input | `Pin 8` | Receives echo pulse for Slot 1 |
| **Slot 2 Trigger (Trig 2)** | Analog Output | `Pin A0` | Sends ultrasonic pulse for Slot 2 |
| **Slot 2 Echo (Echo 2)** | Analog Input | `Pin A1` | Receives echo pulse for Slot 2 |
| **Slot 1 Indicator LED** | Analog Output | `Pin A4` | Indicator LED for Slot 1 occupancy |
| **Slot 2 Indicator LED** | Analog Output | `Pin A5` | Indicator LED for Slot 2 occupancy |
| **PIR Motion Sensor** | Analog Input | `Pin A2` | Triggers power-saving backlight control |
| **LCD Backlight Switch** | Analog Output | `Pin A3` | Connected to LCD pin 15 (LED +) to toggle backlight power |
| **16x2 LCD RS** | Digital I/O | `Pin 12` | Register Select pin for the LCD character display |
| **16x2 LCD Enable (EN)** | Digital I/O | `Pin 11` | Enable write operations on the LCD |
| **16x2 LCD D4 - D7** | Digital I/O | `Pins 5, 4, 3, 2` | 4-bit data communication bus pins |

---

## 🛠️ Installation & Simulation Guide

### Tinkercad Simulation Setup (Virtual Deployment)
1. Navigate to your [Tinkercad Circuits](https://www.tinkercad.com/) dashboard.
2. Construct the layout according to the **Wiring & Pin Mapping** table above or refer to `Parking simulator final final.png` in this repository.
3. Open the **Code editor** panel in Tinkercad, switch the mode to **Text**, and paste the contents of [copy_of_parking_simulator1 finalllll.ino](copy_of_parking_simulator1%20finalllll.ino) into the IDE.
4. Click **Start Simulation** to run.

### Physical Arduino Uno Hardware Setup
1. Assemble all required components listed in `PARKING SYSTEM COMPONENTS.csv`.
2. Wire up the components on a breadboard using the schematics outlined in `Parking simulator (1).pdf` as a reference.
3. Open the [Arduino IDE](https://www.arduino.cc/en/software) on your computer.
4. Connect your physical Arduino Uno board via USB.
5. Create a new sketch, paste the [copy_of_parking_simulator1 finalllll.ino](copy_of_parking_simulator1%20finalllll.ino) code, select the correct Port and Board under **Tools**, and click **Upload**.
6. Open the **Serial Monitor** (Ctrl + Shift + M) at **9600 Baud** to inspect diagnostic outputs.

---

## 📊 Live Diagnostic Outputs

When running, the Serial Console streams live status updates structured as follows:

```text
PIR: 0 | Entry: -1 | Exit: -1 | Slot1: 152 | Slot2: 120
PIR: 1 | Entry: 15  | Exit: -1 | Slot1: 22  | Slot2: 125
PIR: 1 | Entry: 15  | Exit: 12 | Slot1: 22  | Slot2: 24
```
*   `PIR: 1` Indicates driver approach detected (LCD Backlight lights up!).
*   `Entry: 15` Gate opens! (Vehicle is closer than the 30cm boundary).
*   `Slot1: 22` Slot 1 LED is turned ON (Distance is under the 40cm threshold, indicating a vehicle is parked).
```
```
