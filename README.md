Complete Smart Plant Monitor Guide
### Quick note 
at the end of every step are some troubleshoots for problems some things are from my own expiriance wich will have a picture of the error others have some general problem solutions because i did not run into any problems at that stage.

# What You Need
## Hardware
- 1x NodeMCU 1.0 (ESP-12E Module)
-  1x Capacitive Soil Moisture Sensor (3-4 wires)
-   1x BH1750 Light Sensor (I2C module)
-   1x WS2812B LED Strip (8-12 LEDs)
-    Micro USB cable
-    Computer with internet connection
-  Software
-   Arduino IDE (free download)
-   Free OpenWeatherMap account (for weather data)
### Optional
- External 5V power supply (for 12+ LEDs)
-  Breadboard for easier connections


#  STEP 1: Install Required Libraries
1.1 Open Library Manager
Go to Sketch → Include Library → Manage Libraries
Wait for the list to load

<img width="361" height="370" alt="image" src="https://github.com/user-attachments/assets/46b597ad-1b1c-40ab-9ec9-09534cdd1e0d" />
 
 or go to the library directly for easy use

1.2 Install Adafruit NeoPixel Library
In the search box, type: Adafruit NeoPixel
Find "Adafruit NeoPixel by Adafruit"
Click Install
Wait until it says "Installed"

1.3 Install BH1750 Library
In the search box, type: BH1750
Find "BH1750 by Christopher Laws"
Click Install

1.4 Install ArduinoJson Library
In the search box, type: ArduinoJson
Find "ArduinoJson by Benoit Blanchon"
Click Install
Close the Library Manager

✓ Success check: All three libraries show as "INSTALLED" in Library Manager


# General problems

if your ardruino  has problems connecting or gives you an error like this 

<img width="1033" height="98" alt="image" src="https://github.com/user-attachments/assets/4927278e-0d00-463e-bca3-90ea7c1346b9" />

try any of these 4 steps 

Solution 1: Force boot mode (most effective)

Hold down the FLASH button on your NodeMCU
Briefly press the RST button (while still holding FLASH)
Release FLASH
Now click Upload in Arduino IDE

Solution 2: Timing adjustment
If solution 1 doesn't work:

Click Upload in Arduino IDE
As soon as you see "Connecting....." → hold down FLASH
Keep holding until upload starts

Solution 3: Check settings
Go to Tools and verify:

Board: "NodeMCU 1.0 (ESP-12E Module)"
Upload Speed: Lower to 115200 (default is often 921600, which can cause problems)
CPU Frequency: 80 MHz
Flash Size: "4MB (FS:2MB OTA:~1019KB)"
Port: Correct COM port

Solution 4: Driver issue
If nothing works, install the CH340 driver:

Download from: https://sparks.gogo.co.nz/ch340.html
Restart your computer after installation
Try different USB cable

Try Solution 1 first - it works in 90% of cases!



# step 2
##connecting the led strip to the arduino board.


<img width="231" height="126" alt="image" src="https://github.com/user-attachments/assets/5a4e6d1e-e27b-46b7-ba84-ac1c2e993ff6" />

once everything is connected try this code to make sure the led light works 

```ruby


#include <Adafruit_NeoPixel.h>

#define LED_PIN D4        // Data pin
#define NUM_LEDS 12       // Number of LEDs on your strip

Adafruit_NeoPixel strip(NUM_LEDS, LED_PIN, NEO_GRB + NEO_KHZ800);

void setup() {
  Serial.begin(115200);
  Serial.println("\n=== LED Strip Test ===");
  
  strip.begin();           // Initialize strip
  strip.setBrightness(50); // Brightness (0-255)
  strip.show();            // Turn off LEDs
  
  Serial.println("LED strip initialized!");
}

void loop() {
  // Red
  Serial.println("Red...");
  for(int i = 0; i < NUM_LEDS; i++) {
    strip.setPixelColor(i, strip.Color(255, 0, 0));
  }
  strip.show();
  delay(1000);
  
  // Green
  Serial.println("Green...");
  for(int i = 0; i < NUM_LEDS; i++) {
    strip.setPixelColor(i, strip.Color(0, 255, 0));
  }
  strip.show();
  delay(1000);
  
  // Blue
  Serial.println("Blue...");
  for(int i = 0; i < NUM_LEDS; i++) {
    strip.setPixelColor(i, strip.Color(0, 0, 255));
  }
  strip.show();
  delay(1000);
  
  // Off
  Serial.println("Off...");
  for(int i = 0; i < NUM_LEDS; i++) {
    strip.setPixelColor(i, strip.Color(0, 0, 0));
  }
  strip.show();
  delay(1000);
}
```
## troubleshoot
if you have any problems with the led lights try these things

No light? Check if VIN (5V) is connected, not 3V3

Wrong colors? Try NEO_RGB instead of NEO_GRB

First LED works, rest don't? Check if data line is properly connected

# step 3
## connecting the moisture sensor

<img width="223" height="133" alt="image" src="https://github.com/user-attachments/assets/735c4714-b47e-4510-86e6-4ccedbc5897b" />

once everything is connected try this code 
```ruby
const int moistureSensorPin = A0;

// Calibration values (adjust these!)
int dryValue = 850;  // Sensor in dry air
int wetValue = 450;  // Sensor in water

void setup() {
  Serial.begin(115200);
  Serial.println("\n=== Moisture Sensor Test ===");
  Serial.println("Tip: Calibrate your sensor first!");
  Serial.println();
}

void loop() {
  // Read raw value
  int sensorValue = analogRead(moistureSensorPin);
  
  // Convert to percentage (0-100%)
  int moisturePercent = map(sensorValue, dryValue, wetValue, 0, 100);
  moisturePercent = constrain(moisturePercent, 0, 100);
  
  // Print results
  Serial.print("Raw value: ");
  Serial.print(sensorValue);
  Serial.print(" | Moisture: ");
  Serial.print(moisturePercent);
  Serial.print("% | Status: ");
  
  if (moisturePercent >= 60) {
    Serial.println("WET");
  } else if (moisturePercent >= 30) {
    Serial.println("NORMAL");
  } else {
    Serial.println("DRY");
  }
  
  delay(1000);
}
```
### calibration
now we will callibrate the sensor 
- Upload the code
- Open Serial Monitor (115200 baud)
<img width="1919" height="813" alt="image" src="https://github.com/user-attachments/assets/7bccadc2-832b-445d-acfd-be9b05178a64" />


- Hold sensor in dry air → note value
- Put sensor in glass of water → note value
- update the values in the code
- upload the new code and test it


# step 4
## Adding BH1750 Light Sensor
<img width="229" height="158" alt="image" src="https://github.com/user-attachments/assets/befa4023-5a5e-4da3-9d22-c835dcc182b7" />

now try this test code 
```ruby
#include <Wire.h>
#include <BH1750.h>

BH1750 lightMeter;

void setup() {
  Serial.begin(115200);
  Serial.println("\n=== BH1750 Light Sensor Test ===");
  
  // Start I2C bus
  Wire.begin(D2, D5);  // SDA = D2, SCL = D5
  
  // Initialize sensor
  if (lightMeter.begin(BH1750::CONTINUOUS_HIGH_RES_MODE)) {
    Serial.println("✓ BH1750 successfully connected!");
  } else {
    Serial.println("✗ BH1750 not found!");
    Serial.println("Check your wiring:");
    Serial.println("  VCC → 3V3");
    Serial.println("  GND → GND");
    Serial.println("  SDA → D2");
    Serial.println("  SCL → D5");
  }
}

void loop() {
  float lux = lightMeter.readLightLevel();
  
  Serial.print("Light intensity: ");
  Serial.print(lux);
  Serial.print(" lux | ");
  
  // Interpretation
  if (lux < 50) {
    Serial.println("Very dark");
  } else if (lux < 200) {
    Serial.println("Dark");
  } else if (lux < 500) {
    Serial.println("Indoor lighting");
  } else if (lux < 1000) {
    Serial.println("Overcast");
  } else if (lux < 10000) {
    Serial.println("Bright daylight");
  } else {
    Serial.println("Direct sunlight");
  }
  
  delay(1000);
}
```

if it works you should see the value chance in the serial moniter.

while making this guide my light sensor stopped working 
<img width="307" height="17" alt="image" src="https://github.com/user-attachments/assets/1d732d0a-3199-41c3-bbdd-a0dc48699641" />
if you get the same error it might be some of these things go ahead and try them all

- make sure its in the right pin
- check if the pins are really connected and not lose
- try a diffrent sensor to see if that is the problem


  # step 5
  ## Wifi connection

  now we will try and connect the arduino to wifi. before we begin some important notes make sure that the wifi or hotspot you are gonna use is on 2.4 ghz otherwise this wil not work because the arduino only works at 2.4ghz.
```ruby
  #include <ESP8266WiFi.h>

const char* ssid = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";

void setup() {
  Serial.begin(115200);
  delay(100);
  Serial.println("\n=== WiFi Connection Test ===");
  
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  
  Serial.print("Connecting to ");
  Serial.println(ssid);
  
  int attempts = 0;
  while (WiFi.status() != WL_CONNECTED && attempts < 20) {
    delay(500);
    Serial.print(".");
    attempts++;
  }
  
  if (WiFi.status() == WL_CONNECTED) {
    Serial.println("\n✓ WiFi connected!");
    Serial.print("IP address: ");
    Serial.println(WiFi.localIP());
    Serial.print("Signal strength: ");
    Serial.print(WiFi.RSSI());
    Serial.println(" dBm");
  } else {
    Serial.println("\n✗ Connection failed!");
    Serial.println("Check your SSID and password");
  }
}

void loop() {
  if (WiFi.status() == WL_CONNECTED) {
    Serial.println("WiFi still connected ✓");
  } else {
    Serial.println("WiFi connection lost!");
  }
  delay(5000);
}
```
<img width="657" height="182" alt="image" src="https://github.com/user-attachments/assets/ab24be93-1658-4a5d-b4bb-188964d97a53" />

if you get this error make sure that you correctly typed your wifi and pasword as it is really sensitive for cappital letters.


<img width="297" height="65" alt="image" src="https://github.com/user-attachments/assets/328e35f8-18e5-43ea-99aa-c6a52c62a9ba" />

if it works you should see this in the serial moniter
