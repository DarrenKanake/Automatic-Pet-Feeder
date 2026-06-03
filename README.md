# Smart Pet Feeder (ESP32 + Web Control)
Team
---
Kamau Faith Wambui
---
Kyalo Emmanuel Kavemba
---
Kanake Darren Rene
---
This is a smart pet feeding system we built using an **ESP32**.
It automatically detects when a pet is near, notifies the owner, and allows feeding either manually or remotely through a web interface.

The system also tracks food weight in real-time and ensures safe feeding with cooldowns and checks.

---

## 🚀 Features

* **Automatic Pet Detection**

  * Uses an IR sensor to detect when the pet is near the bowl

* **Remote Feeding Approval**

  * Sends a notification through WebSocket
  * Owner approves feeding from a web dashboard

* **Manual Controls**

  * Button to feed manually
  * Button to tare/reset the scale

* **Accurate Weight Measurement**

  * Uses HX711 load cell
  * Applies filtering for stable readings

* **Cooldown System**

  * Prevents continuous feeding (8-second delay between feeds)

* **OLED Display**

  * Shows:

    * Bowl weight
    * Food level
    * Pet presence
    * Last feed time

* **Sound Feedback**

  * Buzzer alerts during feeding

* **Built-in Wi-Fi Access Point**

  * ESP32 creates its own network
  * No internet needed

---

## 🧠 How It Works

1. System continuously reads weight and checks for pet presence
2. If a pet is detected and food is low:

   * A message is sent to the web dashboard
3. Owner clicks **"Approve Feeding"**
4. Servo opens and dispenses food
5. System waits for cooldown before next feeding

---

## 🔌 Hardware Components

| Component                | ESP32 Pin |
| ------------------------ | --------- |
| Ultrasonic Sensor (TRIG) | 16        |
| Ultrasonic Sensor (ECHO) | 17        |
| HX711 DT                 | 34        |
| HX711 SCK                | 18        |
| IR Sensor                | 27        |
| Servo Motor              | 26        |
| Buzzer                   | 25        |
| Manual Feed Button       | 23        |
| Tare Button              | 19        |
| OLED SDA                 | 21        |
| OLED SCL                 | 22        |

---

## 📶 Wi-Fi Access

The ESP32 creates its own Wi-Fi network:

* **SSID:** `PetFeeder`
* **Password:** `12345678`

Open your browser and go to:

```
http://192.168.4.1
```

---

## 💻 Web Interface

The web page allows you to:

* See pet detection messages
* Approve feeding remotely

### Button:

* **Approve Feeding** → sends command to ESP32 via WebSocket

---

## 🛠️ Setup

### Requirements

Install these libraries:

* HX711
* ESP32Servo
* Adafruit SSD1306
* Adafruit GFX
* ESPAsyncWebServer
* AsyncTCP

---

### Steps

1. Connect all components using the pin table
2. Upload code to ESP32
3. Power the system
4. Connect to `PetFeeder` Wi-Fi
5. Open `192.168.4.1` in browser

---

## ⚙️ Important Notes

* Calibration factor may need adjustment depending on your load cell
* If weight readings are unstable, check wiring and filtering
* Ensure servo has enough power supply

---

## 💡 Summary

This project combines embedded systems, IoT, and real-time control to create a smart and reliable pet feeder.
It’s simple to use, works offline, and gives full control to the owner.

---

