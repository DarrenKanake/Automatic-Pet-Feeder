# Smart Pet Feeding and Monitoring System

This is a smart IoT-based pet feeder I built as part of my instrumentation project. It’s powered by an **ESP32-WROOM-32** and designed to automate feeding, track food levels, and make sure only the right pet gets fed.

The system runs on its own local Wi-Fi network and has a web dashboard where you can monitor everything in real time, trigger feeding, and manage RFID access.

---

## 🚀 Key Features

* **State Machine Design:**
  Instead of messy loops, the system uses a clean state-based approach (`STATE_IDLE` → `STATE_ADD_RFID_MODE`) which makes it faster and more efficient.

* **Dual Authentication (RFID + IR):**
  Feeding only happens if:

  * The pet is physically present (IR sensor)
  * The RFID tag is recognized
    This prevents wrong or accidental feeding.

* **Accurate Weight Measurement:**
  Uses an **HX711 load cell** with a moving average filter (15 samples) to give stable and accurate food weight readings in grams.

* **Safety Checks:**

  * Won’t feed if bowl already has enough food (>40g)
  * Won’t feed if storage is almost empty (<10%)
  * Clears unauthorized attempts automatically

* **RFID Storage (Persistent):**
  Stores up to 5 RFID tags using ESP32 flash memory, so data is not lost even after power off.

* **Built-in Web Dashboard:**
  The ESP32 hosts a simple web server where you can:

  * See live data
  * Feed remotely
  * Calibrate the scale
  * Add/remove RFID tags

---

## 🗺️ System Flow (State Machine)

Here’s how the system runs step by step:

1. **`STATE_IDLE`**
   Monitors weight, food level, and checks if a pet is nearby.

2. **`STATE_PET_DETECTED`**
   Confirms the pet is actually present (filters noise).

3. **`STATE_SCANNING_RFID`**
   Starts scanning for RFID tags.

4. **`STATE_AUTHORIZED / UNAUTHORIZED`**

   * If valid → proceed
   * If not → buzzer alert

5. **`STATE_FEEDING`**
   Opens the food gate using a servo motor and temporarily pauses weight readings to avoid noise.

6. **`STATE_ADD_RFID_MODE`**
   30-second mode to register new RFID tags.

---

## 🔌 Hardware Setup

| Component         | Module         | ESP32 Pin                | Interface |
| ----------------- | -------------- | ------------------------ | --------- |
| Controller        | ESP32-WROOM-32 | –                        | Core      |
| OLED Display      | SSD1306        | IO21 (SDA), IO22 (SCL)   | I2C       |
| Ultrasonic Sensor | HC-SR04        | IO16 (TRIG), IO17 (ECHO) | Digital   |
| Load Cell         | HX711          | IO34 (DT), IO18 (SCK)    | Serial    |
| IR Sensor         | FC-51          | IO27                     | Digital   |
| Servo Motor       | SG90           | IO26                     | PWM       |
| Buzzer            | Passive        | IO25                     | Digital   |
| Feed Button       | Push Button    | IO23                     | Input     |
| Tare Button       | Push Button    | IO19                     | Input     |
| RFID (SS)         | MFRC522        | IO5                      | SPI       |
| RFID (RST)        | MFRC522        | IO4                      | SPI       |
| RFID (SCK)        | MFRC522        | IO14                     | SPI       |
| RFID (MOSI)       | MFRC522        | IO13                     | SPI       |
| RFID (MISO)       | MFRC522        | IO12                     | SPI       |

---

## 💻 Web Dashboard

Connect to Wi-Fi:

* **SSID:** `PetFeeder_WiFi`
* **Password:** `feedthepet`
* **IP Address:** `192.168.4.1`

### Available Endpoints:

* **`/`**
  Main dashboard (auto-refresh every 3 seconds)

* **`/feed`**
  Manually trigger feeding (checks safety first)

* **`/tare`**
  Reset the scale

* **`/addrfid`**
  Add a new RFID tag (30-second window)

* **`/removerfid`**
  Clear all saved RFID tags

---

## 🛠️ Setup & Upload

### Requirements

* VS Code + PlatformIO (recommended) or Arduino IDE
* Install these libraries:

  * HX711
  * ESP32Servo
  * Adafruit SSD1306
  * Adafruit GFX
  * MFRC522

### Steps

1. **Connect Hardware**
   Follow the pin configuration table above.

2. **Calibrate the Load Cell**
   Adjust `CALIBRATION_FACTOR` (default: 6748.75) if readings are off.

3. **Upload Code**

   * Connect ESP32 via USB
   * Select board: `esp32dev`
   * Upload

4. **Access Dashboard**

   * Connect to Wi-Fi
   * Open browser → `http://192.168.4.1`

---

## 💡 Summary

This system combines embedded systems, IoT, and sensor integration to create a reliable and secure automated pet feeder. It’s efficient, accurate, and designed with real-world safety and usability in mind.

