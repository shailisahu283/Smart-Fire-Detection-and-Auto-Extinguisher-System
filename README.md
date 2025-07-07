# 🔥 Smart Fire Detection and Auto-Extinguisher System 🔧🧯

An **Arduino Uno**-based real-time safety project that senses **temperature**, **gas**, and **light** levels to **automatically detect and extinguish fires** using a water pump and alert system. It is ideal for use in homes, labs, and industries where **real-time fire response is critical**.

🔗 **[Tinkercad Simulation Project Link](https://www.tinkercad.com/things/6RlD1wMv3Bi-smart-fire-detection-and-auto-extinguisher-system?sharecode=u-PO-R5tO7RxkcP3zWa5F9RO92UQc3yLnriXb1KtBdQ)** 
---

## 📚 Table of Contents

1. [🎯 Project Objective](#-project-objective)
2. [⚙️ Components Used](#-components-used)
3. [🔌 Circuit Overview](#-circuit-overview)
4. [🧠 Working Principle](#-working-principle)
5. [💻 Arduino Code](#-arduino-code)
6. [📊 Real-Time Display](#-real-time-display)
7. [🌱 Applications & Use Cases](#-applications--use-cases)
8. [🌟 Future Enhancements](#-future-enhancements)
9. [📸 Project Screenshots](#-project-screenshots)
10. [🙌 Acknowledgement](#-acknowledgement)
11. [📎 Project Link](#-project-link)

---

## 🎯 Project Objective

To build a **real-time fire detection and suppression system** using sensors and Arduino Uno that can:

* Detect early signs of fire (🔥heat, 💨gas, 💡light)
* 🔊 Trigger an audible buzzer alarm
* 💧 Automatically activate a water pump to suppress fire
* 🌈 Indicate danger/safety visually with NeoPixel
* 📟 Display system status messages on LCD

---

## ⚙️ Components Used

| Component                 | Quantity | Purpose                            |
| ------------------------- | -------- | ---------------------------------- |
| 🔌 Arduino UNO            | 1        | Main microcontroller               |
| 🌡️ LM35 Temp Sensor      | 1        | Measures surrounding temperature   |
| 🧪 MQ2 Gas Sensor         | 1        | Detects smoke/gas leaks            |
| 💡 Phototransistor        | 1        | Detects sudden brightness (flame)  |
| 🔔 Piezo Buzzer           | 1        | Alarm sound when fire is detected  |
| 💧 Water Pump             | 1        | Used to simulate fire suppression  |
| 📟 16x2 LCD               | 1        | Displays real-time system messages |
| 🌈 NeoPixel Ring (7 LEDs) | 1        | Red for danger, Green for safe     |
| 🔗 Breadboard + Jumpers   | Many     | Circuit connections                |

---

## 🔌 Circuit Overview

* Sensors connected to analog pins:

  * A0 → LM35
  * A1 → MQ2
  * A2 → Phototransistor

* Digital Output Pins:

  * Pin 2 → NeoPixel Ring
  * Pin 3 → Buzzer
  * Pin 10 → Pump
  * Pins 4–9 → LCD Display

🧩 Refer to the circuit diagram or Tinkercad simulation to wire the components accurately.

---

## 🧠 Working Principle

📡 The Arduino constantly monitors:

* **Temperature** via LM35
* **Gas** levels via MQ2
* **Light** intensity via Phototransistor

🧠 If the following condition(s) are met:

* Gas > 70
* Temperature > 80
* Light > 5

📣 It performs these actions:

* 🔔 Buzzer ON
* 💧 Pump ON
* 🖥️ LCD displays "ALERT" and "EVACUATE"
* 🔴 NeoPixel turns RED

✅ Else, system displays "SAFE", turns NeoPixel GREEN, and all outputs remain OFF.

---

## 💻 Arduino Code

```cpp
#include <LiquidCrystal.h>
#define D4 7
#define D5 6
#define D6 5
#define D7 4
#define E 8
#define RS 9
#include <Adafruit_NeoPixel.h>
#define PIN 2
#define N_LEDS 7

LiquidCrystal LCD(9,8,7,6,5,4);
int piezo =3;
int pump = 10; 
int temp = 0;
int light = 0;
Adafruit_NeoPixel strip = Adafruit_NeoPixel(N_LEDS, PIN, NEO_GRB + NEO_KHZ800);

void setup() {
    Serial.begin(9600);
    pinMode(A0, INPUT); // Temperature
    pinMode(A1, INPUT); // Gas
    pinMode(A2, INPUT); // Light
    pinMode(piezo, OUTPUT);
    pinMode(pump, OUTPUT);
    LCD.begin(16,2); 
    strip.begin();
}

void loop() {
    int temp = analogRead(A0);
    int light = analogRead(A2);
    int gasVal = analogRead(A1);
    int gasMapped = map(gasVal, 0, 1023, 0, 255);

    Serial.print("Temperature: "); Serial.println(temp);
    Serial.print("Light Level: "); Serial.println(light);
    Serial.print("Gas Intensity: "); Serial.println(gasMapped);
    delay(2000);

    if ((gasMapped > 70 && light > 5) || (temp > 80 && light > 5) || (gasMapped > 70 && temp > 80)) {
        digitalWrite(piezo, HIGH);
        digitalWrite(pump, HIGH);
        LCD.clear();
        LCD.setCursor(0,0); LCD.print("ALERT");
        delay(1000);
        LCD.clear();
        LCD.setCursor(0,1); LCD.print("EVACUATE");
        delay(1000);
        chase(strip.Color(255, 0, 0)); // Red alert
        delay(100);
    } else {
        digitalWrite(piezo, LOW);
        digitalWrite(pump, LOW);
        LCD.clear();
        LCD.setCursor(0,0); LCD.print("SAFE");
        delay(1000);
        LCD.clear();
        LCD.setCursor(0,1); LCD.print("ALL CLEAR");
        delay(1000);
        chase(strip.Color(0, 255, 0)); // Green safe
        delay(100);
    }
}

static void chase(uint32_t c) {
    for(uint8_t i=0; i<strip.numPixels()+8; i++) {
        strip.setPixelColor(i  , c);
        strip.setPixelColor(i-8, 0);
        strip.show();
        delay(50);
    }
}
```

---

## 📊 Real-Time Display

* LCD shows:

  * 🔐 `SAFE` / `ALL CLEAR`
  * 🚨 `ALERT` / `EVACUATE`

* Serial Monitor logs:

  * 🌡️ Temperature
  * 💨 Gas intensity
  * 💡 Light level

* NeoPixel Status:

  * 🔴 Red: Danger
  * 🟢 Green: Safe

---

## 🌱 Applications & Use Cases

* 🏠 Home safety automation
* 🧪 Gas lab leakage detection
* 🏭 Industrial fire safety system
* 🚗 Car engine fire alert
* 🌲 Smart forest fire detector (expandable)

---

## 🌟 Future Enhancements

* 🌐 **IoT integration** (ESP8266, mobile app notifications)
* 🔋 Battery-powered operation
* 🎥 Add camera for real-time visual alert
* 🧠 AI-based fire risk prediction
* 📱 Touch interface or voice alerts

---

## 📸 Project Screenshots

📌 Upload your screenshots below to showcase working stages of your project:

1. **System in Safe State**

   > ![Safe State Screenshot](https://github.com/user-attachments/assets/17b6a2a3-1216-4444-8d24-9fd8adbf9855)

2. **Fire Detected – Buzzer and Pump Activated**

   > ![Alert State Screenshot](https://github.com/user-attachments/assets/4cacedfa-ce90-45a4-8b78-d6806a374950)

3. **TinkerCad Full Circuit Layout**

   > ![Circuit Diagram](https://github.com/user-attachments/assets/95bd3a7d-1a6d-4245-b46e-3f671d245519)


---

## 🙌 Acknowledgement

I would like to express my heartfelt gratitude to:

* 👨‍🏫 My professors and mentors who guided me through the logic
* 💻 The creators of **Tinkercad**, **Arduino IDE**, and **open-source libraries**
* 👨‍💻 GitHub and open-source contributors for documentation support

---

## 📎 Project Link

🔗 **[Click here to run the TinkerCad Simulation](https://www.tinkercad.com/things/6RlD1wMv3Bi-smart-fire-detection-and-auto-extinguisher-system?sharecode=u-PO-R5tO7RxkcP3zWa5F9RO92UQc3yLnriXb1KtBdQ)** 
