🌍 Real-Time GPS Tracker using NodeMCU + Neo-6M (No SIM Card Required)

Track your location in real-time using Wi-Fi instead of mobile networks! This project uses the NodeMCU ESP8266 and Neo-6M GPS module to send live location data to a web interface or Google Maps—no SIM card or GSM module needed.

📦 Features
- 🔄 Real-time GPS tracking (latitude, longitude, speed, accuracy)
- 🌐 Sends data over Wi-Fi (ESP8266)
- 🗺️ Displays location on Google Maps or a custom web interface
- 💸 Low cost and no monthly SIM fees
- 🪫 USB or battery powered

🔧 Hardware Required
| Component           | Quantity |
|--------------------|----------|
| NodeMCU ESP8266    | 1        |
| Neo-6M GPS Module  | 1        |
| Micro USB Cable    | 1        |
| Jumper Wires       | 4        |
| Breadboard (opt.)  | 1        |

🔌 Circuit Diagram
| Neo-6M Pin | NodeMCU Pin |
|------------|--------------|
| VCC        | 3.3V         |
| GND        | GND          |
| TX         | D7 (GPIO13)  |
| RX         | D8 (GPIO15)  |

⚠️ Ensure GPS module gets sufficient voltage—some boards may require 5V.

💻 Software Setup

1. Install Arduino IDE
   Download from: https://www.arduino.cc/en/software

2. Install Required Libraries
   - TinyGPS++
   - SoftwareSerial
   - ESP8266WiFi

3. Add ESP8266 Board to Arduino IDE
   File → Preferences → Additional Board Manager URLs:
   http://arduino.esp8266.com/stable/package_esp8266com_index.json

🚀 Upload Code

```cpp
#include <SoftwareSerial.h>
#include <TinyGPS++.h>
#include <ESP8266WiFi.h>

const char* ssid = "YOUR_WIFI_SSID";
const char* password = "YOUR_WIFI_PASSWORD";

TinyGPSPlus gps;
SoftwareSerial gpsSerial(D7, D8); // RX, TX

WiFiClient client;

void setup() {
  Serial.begin(115200);
  gpsSerial.begin(9600);

  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\nWiFi Connected");
}

void loop() {
  while (gpsSerial.available() > 0) {
    gps.encode(gpsSerial.read());
    if (gps.location.isUpdated()) {
      float lat = gps.location.lat();
      float lng = gps.location.lng();
      Serial.print("Lat: "); Serial.print(lat, 6);
      Serial.print(" | Lng: "); Serial.println(lng, 6);

      // Create Google Maps URL
      Serial.print("Map: https://maps.google.com/?q=");
      Serial.print(lat, 6);
      Serial.print(",");
      Serial.println(lng, 6);
    }
  }
}
```

🗺️ Viewing Location
Paste coordinates in your browser:
https://maps.google.com/?q=12.971598,77.594566

🧠 How It Works
1. Neo-6M gets real-time GPS data via satellite.
2. NodeMCU parses the GPS data using TinyGPS++.
3. Wi-Fi module sends data over the network.
4. Output includes a real-time Google Maps link in the Serial Monitor.

📈 Optional Enhancements
- 🔋 Use a LiPo battery for mobile tracking
- 📲 Send GPS data to Blynk, Firebase, or a custom server
- 📟 Add an OLED display for real-time readings
- 🧠 Add a push-button to log checkpoints
- 📡 Store GPS data locally when offline, upload later

🧪 Troubleshooting
- GPS not working? Move to open sky, wait 30–60 seconds for fix.
- Only 0.00 lat/lon? GPS doesn’t have a fix yet.
- No Serial output? Check baud rates and wiring.

📄 License
MIT License. Feel free to remix and share!

