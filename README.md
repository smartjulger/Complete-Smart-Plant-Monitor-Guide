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


<img width="1852" height="196" alt="image" src="https://github.com/user-attachments/assets/7057ca2f-e6c7-433b-886c-297d01035e4e" />

make sure that it is set to 115200 baud if needed chance it



- Hold sensor in dry air → note value

  <img width="496" height="112" alt="image" src="https://github.com/user-attachments/assets/513f7e29-5c40-4604-a4c2-7b0ec1a36fe9" />

  this is wat it will look like the values these are my dry values they might be diffrent from yours

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

<img width="301" height="89" alt="image" src="https://github.com/user-attachments/assets/49365c61-ab2c-4c44-85f0-15cb333e4f62" />


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


# step 6 
## basic webserver

now we will make a basic webserver so you can see your data on your own netwerk. this wil be a local webserver so only devices on the same network can work
```ruby
#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>

const char* ssid = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";

ESP8266WebServer server(80);

void handleRoot() {
  String html = "<html><body>";
  html += "<h1>Hello from ESP8266!</h1>";
  html += "<p>The webserver works!</p>";
  html += "</body></html>";
  
  server.send(200, "text/html", html);
}

void setup() {
  Serial.begin(115200);
  Serial.println("\n=== Webserver Test ===");
  
  WiFi.begin(ssid, password);
  
  Serial.print("Connecting");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  
  Serial.println("\n✓ WiFi connected!");
  Serial.print("Open in browser: http://");
  Serial.println(WiFi.localIP());
  
  server.on("/", handleRoot);
  server.begin();
  Serial.println("Webserver started!");
}

void loop() {
  server.handleClient();
}
```
<img width="400" height="56" alt="image" src="https://github.com/user-attachments/assets/4a4784de-9cd5-4140-8476-fed0e17656e7" />

if everything works you should see this in your serial moniter

after that open the ip you see in your browser and you should see something like this
<img width="732" height="217" alt="image" src="https://github.com/user-attachments/assets/2b67d143-1ef8-4069-b25c-da2d3a9ea8ae" />

if you do not see anything like this make sure that you have the right wifi since it only works on the same network.

# step 7 
## weather api

now we will get the weather api key so we can get data from it witch will tell us what the weather will be like so you know when it is the best time 
to put your plant into the sun.

1. Go to openweathermap.org
2. Create free account
   once logged in go to your profile where you can sellect your api key
   <img width="2536" height="1160" alt="image" src="https://github.com/user-attachments/assets/e5600f54-3c43-41d0-8762-98ecac061922" />


4. Go to API Keys section
once inside you should see something like this copy that key

<img width="1125" height="318" alt="image" src="https://github.com/user-attachments/assets/08bea5c4-ac79-4457-9676-45695dad9c25" />

6. Copy your API key
7. put the API key into the provide code


```ruby
#include <ESP8266WiFi.h>
#include <ESP8266HTTPClient.h>
#include <WiFiClient.h>
#include <ArduinoJson.h>

const char* ssid = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";
const char* apiKey = "YOUR_API_KEY";
const char* city = "Amsterdam";
const char* countryCode = "NL";

void setup() {
  Serial.begin(115200);
  Serial.println("\n=== Weather API Test ===");
  
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\n✓ WiFi connected!");
  
  // Get weather data
  getWeather();
}

void loop() {
  // Get weather every 10 minutes
  delay(600000);
  getWeather();
}

void getWeather() {
  WiFiClient client;
  HTTPClient http;
  
  String url = "http://api.openweathermap.org/data/2.5/weather?q=" + 
               String(city) + "," + String(countryCode) + 
               "&appid=" + String(apiKey) + "&units=metric";
  
  Serial.println("Fetching weather data...");
  http.begin(client, url);
  int httpCode = http.GET();
  
  if (httpCode == 200) {
    String payload = http.getString();
    
    DynamicJsonDocument doc(1024);
    deserializeJson(doc, payload);
    
    String weather = doc["weather"][0]["main"].as<String>();
    float temp = doc["main"]["temp"];
    float humidity = doc["main"]["humidity"];
    float clouds = doc["clouds"]["all"];
    
    Serial.println("\n✓ Weather data received:");
    Serial.print("  Weather: ");
    Serial.println(weather);
    Serial.print("  Temperature: ");
    Serial.print(temp);
    Serial.println("°C");
    Serial.print("  Humidity: ");
    Serial.print(humidity);
    Serial.println("%");
    Serial.print("  Cloudiness: ");
    Serial.print(clouds);
    Serial.println("%");
    
  } else if (httpCode == 401) {
    Serial.println("✗ API Key invalid!");
    Serial.println("Check your API key and wait 10-20 min after creation");
  } else {
    Serial.print("✗ HTTP Error: ");
    Serial.println(httpCode);
  }
  
  http.end();
}
```
if everything works you should see something like this in your serial moniter

<img width="250" height="154" alt="image" src="https://github.com/user-attachments/assets/a0c16113-b628-4005-a4e6-def1a8be9635" />


 # final step

 now we will put everthing together with this code

```ruby

 #include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>
#include <ESP8266HTTPClient.h>
#include <WiFiClient.h>
#include <ArduinoJson.h>
#include <Adafruit_NeoPixel.h>
#include <Wire.h>
#include <BH1750.h>

// ====== WiFi Settings ======
const char* ssid = "Name";
const char* password = "Password";

// ====== OpenWeatherMap API ======
const char* weatherApiKey = "your_API_KEY";  // Get free key from openweathermap.org
const char* city = "Amsterdam";
const char* countryCode = "NL";

// ====== Sensor Settings ======
const int moistureSensorPin = A0;
int dryValue = 850;
int wetValue = 450;

// ====== LED Strip Settings ======
#define LED_PIN D4
#define NUM_LEDS 12
Adafruit_NeoPixel strip(NUM_LEDS, LED_PIN, NEO_GRB + NEO_KHZ800);

// ====== Light Sensor BH1750 ======
BH1750 lightMeter;

// ====== Web Server ======
ESP8266WebServer server(80);

// ====== Global Variables ======
int lastMoisturePercent = 0;
float currentLight = 0;
float totalLightToday = 0;
int lightReadings = 0;
String weatherCondition = "Unknown";
String sunAdvice = "Gathering data...";
unsigned long lastWeatherUpdate = 0;
unsigned long lastLightUpdate = 0;
const unsigned long WEATHER_UPDATE_INTERVAL = 1800000; // 30 minutes
const unsigned long LIGHT_UPDATE_INTERVAL = 300000;    // 5 minutes

// Plant light requirements (Daily Light Integral in mol/m²/day)
// Low light plants: 5-10 mol/m²/day
// Medium light: 10-20 mol/m²/day  
// High light: 20-40 mol/m²/day
const float REQUIRED_DAILY_LIGHT = 15.0; // For medium-light plants

void setup() {
  Serial.begin(115200);
  delay(1000);
  Serial.println("\n=== Smart Plant Monitor with Light Sensor ===");

  // Initialize I2C - SDA on D2, SCL on D5
  Wire.begin(D2, D5);
  
  // Initialize light sensor
  if (lightMeter.begin(BH1750::CONTINUOUS_HIGH_RES_MODE)) {
    Serial.println("✓ BH1750 light sensor detected");
  } else {
    Serial.println("✗ BH1750 not found! Check wiring:");
    Serial.println("  VCC → 3V3");
    Serial.println("  GND → GND");
    Serial.println("  SDA → D2");
    Serial.println("  SCL → D5");
  }

  // Initialize LED strip
  strip.begin();
  strip.setBrightness(100);
  strip.show();

  // Connect to WiFi
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  
  Serial.print("Connecting to WiFi");
  int attempts = 0;
  while (WiFi.status() != WL_CONNECTED && attempts < 30) {
    delay(500);
    Serial.print(".");
    attempts++;
  }
  
  if (WiFi.status() == WL_CONNECTED) {
    Serial.println("\n✓ Connected!");
    Serial.print("IP address: ");
    Serial.println(WiFi.localIP());
    Serial.println("Open this address in your browser!\n");
    
    // Get weather data immediately
    updateWeatherData();
  } else {
    Serial.println("\n✗ WiFi connection failed!");
    Serial.println("Check SSID and password in the code");
  }

  // Web server routes
  server.on("/", handleRoot);
  server.on("/data", handleData);
  server.begin();
  Serial.println("Web server started\n");
}

void loop() {
  // Check WiFi connection
  if (WiFi.status() != WL_CONNECTED) {
    Serial.println("WiFi connection lost, reconnecting...");
    WiFi.reconnect();
    delay(5000);
    return;
  }

  // Update sensor data every second
  updateSensors();

  // Update weather data every 30 minutes
  if (millis() - lastWeatherUpdate > WEATHER_UPDATE_INTERVAL) {
    updateWeatherData();
  }

  // Calculate light advice every 5 minutes
  if (millis() - lastLightUpdate > LIGHT_UPDATE_INTERVAL) {
    calculateLightAdvice();
    lastLightUpdate = millis();
  }

  // Handle web server
  server.handleClient();
  delay(10);
}

void updateSensors() {
  static unsigned long lastUpdate = 0;
  if (millis() - lastUpdate < 1000) return;
  
  // Read moisture sensor
  int sensorValue = analogRead(moistureSensorPin);
  lastMoisturePercent = map(sensorValue, dryValue, wetValue, 0, 100);
  lastMoisturePercent = constrain(lastMoisturePercent, 0, 100);

  // Read light sensor (in lux)
  currentLight = lightMeter.readLightLevel();
  
  // Accumulate light for daily calculation
  // 1 lux for 1 second = ~0.0000046 mol/m²
  totalLightToday += (currentLight * 0.0000046);
  lightReadings++;

  // Set LED color based on moisture
  uint32_t color;
  String moistureStatus;
  
  if (lastMoisturePercent >= 60) {
    color = strip.Color(0, 255, 0);
    moistureStatus = "GOOD (Green)";
  } else if (lastMoisturePercent >= 30) {
    color = strip.Color(255, 165, 0);
    moistureStatus = "MODERATE (Orange)";
  } else {
    color = strip.Color(255, 0, 0);
    moistureStatus = "DRY (Red)";
  }

  for (int i = 0; i < NUM_LEDS; i++) {
    strip.setPixelColor(i, color);
  }
  strip.show();

  // Print to Serial Monitor
  Serial.print("Moisture: ");
  Serial.print(lastMoisturePercent);
  Serial.print("% (");
  Serial.print(moistureStatus);
  Serial.print(") | Light: ");
  Serial.print(currentLight);
  Serial.print(" lux | Total: ");
  Serial.print(totalLightToday, 2);
  Serial.println(" mol/m²");

  lastUpdate = millis();
}

void updateWeatherData() {
  if (WiFi.status() != WL_CONNECTED) return;
  
  Serial.println("\n--- Fetching weather data ---");
  
  WiFiClient client;
  HTTPClient http;
  
  String url = "http://api.openweathermap.org/data/2.5/weather?q=" + 
               String(city) + "," + String(countryCode) + 
               "&appid=" + String(weatherApiKey) + "&units=metric";
  
  http.begin(client, url);
  int httpCode = http.GET();
  
  if (httpCode == 200) {
    String payload = http.getString();
    
    DynamicJsonDocument doc(1024);
    DeserializationError error = deserializeJson(doc, payload);
    
    if (!error) {
      weatherCondition = doc["weather"][0]["main"].as<String>();
      float cloudiness = doc["clouds"]["all"].as<float>();
      float temp = doc["main"]["temp"].as<float>();
      
      Serial.print("✓ Weather: ");
      Serial.print(weatherCondition);
      Serial.print(" | Temp: ");
      Serial.print(temp);
      Serial.print("°C | Clouds: ");
      Serial.print(cloudiness);
      Serial.println("%");
      
      lastWeatherUpdate = millis();
      
      // Calculate new light advice immediately
      calculateLightAdvice();
    } else {
      Serial.println("✗ JSON parse error");
    }
  } else if (httpCode == 401) {
    Serial.println("✗ API Key is invalid! Check your weatherApiKey");
  } else {
    Serial.print("✗ HTTP Error: ");
    Serial.println(httpCode);
    Serial.println("Check your internet connection and API key");
  }
  
  http.end();
  Serial.println("--- Weather update complete ---\n");
}

void calculateLightAdvice() {
  float estimatedDailyLight = totalLightToday;
  
  // Calculate how much light the plant has received as percentage
  float lightPercentage = (estimatedDailyLight / REQUIRED_DAILY_LIGHT) * 100;
  
  // Generate advice
  if (lightPercentage >= 100) {
    sunAdvice = "✓ Plant has received enough sun today! (" + String((int)lightPercentage) + "%)";
  } else if (lightPercentage >= 70) {
    sunAdvice = "⚠ Plant almost has enough sun (" + String((int)lightPercentage) + "%). ";
    if (weatherCondition == "Clear") {
      sunAdvice += "It's sunny now - place near window!";
    } else if (weatherCondition == "Clouds") {
      sunAdvice += "It's cloudy now but there's still some light.";
    } else {
      sunAdvice += "Weather: " + weatherCondition;
    }
  } else if (lightPercentage >= 40) {
    sunAdvice = "⚠ Plant needs more sun (" + String((int)lightPercentage) + "%). ";
    if (weatherCondition == "Clear") {
      sunAdvice += "It's sunny outside - move to window!";
    } else {
      sunAdvice += "Weather: " + weatherCondition + ". Find a brighter spot!";
    }
  } else {
    sunAdvice = "✗ Plant is getting too little light! (" + String((int)lightPercentage) + "%). ";
    sunAdvice += "Move to a location with more sunlight or use grow light.";
  }
  
  Serial.println("Light advice: " + sunAdvice);
}

void handleRoot() {
  String status, statusColor;
  
  if (lastMoisturePercent >= 60) {
    status = "GOOD";
    statusColor = "#4CAF50";
  } else if (lastMoisturePercent >= 30) {
    status = "MODERATE";
    statusColor = "#FF9800";
  } else {
    status = "DRY";
    statusColor = "#F44336";
  }
  
  // Light status color
  String lightColor = "#4CAF50";
  if (currentLight < 200) lightColor = "#F44336";
  else if (currentLight < 1000) lightColor = "#FF9800";

  String html = "<!DOCTYPE html><html>";
  html += "<head><meta charset='UTF-8'>";
  html += "<meta name='viewport' content='width=device-width, initial-scale=1'>";
  html += "<title>Smart Plant Monitor</title>";
  html += "<style>";
  html += "* { margin: 0; padding: 0; box-sizing: border-box; }";
  html += "body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; min-height: 100vh; }";
  html += ".container { max-width: 800px; margin: 0 auto; }";
  html += ".header { text-align: center; color: white; margin-bottom: 30px; }";
  html += ".header h1 { font-size: 2.5em; margin-bottom: 10px; }";
  html += ".grid { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 20px; }";
  html += ".card { background: white; padding: 25px; border-radius: 15px; box-shadow: 0 10px 30px rgba(0,0,0,0.3); }";
  html += ".card.full { grid-column: 1 / -1; }";
  html += ".card-title { font-size: 1.2em; color: #666; margin-bottom: 15px; display: flex; align-items: center; gap: 10px; }";
  html += ".circle { width: 120px; height: 120px; border-radius: 50%; display: flex; align-items: center; justify-content: center; margin: 15px auto; font-size: 2em; font-weight: bold; color: white; box-shadow: 0 5px 15px rgba(0,0,0,0.2); }";
  html += ".status-text { text-align: center; font-size: 1.3em; font-weight: bold; margin-top: 10px; }";
  html += ".advice-box { background: #f8f9fa; border-left: 4px solid " + statusColor + "; padding: 15px; border-radius: 5px; margin-top: 15px; }";
  html += ".advice-box.light { border-left-color: " + lightColor + "; }";
  html += ".info-row { display: flex; justify-content: space-between; padding: 10px 0; border-bottom: 1px solid #eee; }";
  html += ".info-label { color: #666; }";
  html += ".info-value { font-weight: bold; color: #333; }";
  html += ".icon { font-size: 1.5em; }";
  html += ".progress-bar { width: 100%; height: 20px; background: #e0e0e0; border-radius: 10px; overflow: hidden; margin-top: 10px; }";
  html += ".progress-fill { height: 100%; background: " + lightColor + "; transition: width 0.3s; }";
  html += "@media (max-width: 600px) { .grid { grid-template-columns: 1fr; } }";
  html += "</style>";
  html += "<script>setTimeout(function(){ location.reload(); }, 5000);</script>";
  html += "</head><body>";
  
  html += "<div class='container'>";
  html += "<div class='header'>";
  html += "<h1>Smart Plant Monitor</h1>";
  html += "<p>Real-time monitoring with light sensor</p>";
  html += "</div>";
  
  html += "<div class='grid'>";
  
  // Moisture card
  html += "<div class='card'>";
  html += "<div class='card-title'><span class='icon'></span> Soil Moisture</div>";
  html += "<div class='circle' style='background:" + statusColor + "'>" + String(lastMoisturePercent) + "%</div>";
  html += "<div class='status-text' style='color:" + statusColor + "'>" + status + "</div>";
  html += "</div>";
  
  // Light card
  html += "<div class='card'>";
  html += "<div class='card-title'><span class='icon'></span> Light Intensity</div>";
  html += "<div class='circle' style='background:" + lightColor + "'>" + String((int)currentLight) + "</div>";
  html += "<div class='status-text' style='color:" + lightColor + "'>lux</div>";
  html += "</div>";
  
  // Light advice card
  html += "<div class='card full'>";
  html += "<div class='card-title'><span class='icon'></span> Sunlight Advice</div>";
  html += "<div class='advice-box light'>" + sunAdvice + "</div>";
  
  // Progress bar
  float lightProgress = (totalLightToday / REQUIRED_DAILY_LIGHT) * 100;
  lightProgress = constrain(lightProgress, 0, 100);
  html += "<div class='progress-bar'>";
  html += "<div class='progress-fill' style='width:" + String((int)lightProgress) + "%'></div>";
  html += "</div>";
  
  html += "<div class='info-row'>";
  html += "<span class='info-label'>Current weather:</span>";
  html += "<span class='info-value'>" + weatherCondition + "</span>";
  html += "</div>";
  html += "<div class='info-row'>";
  html += "<span class='info-label'>Daily light received:</span>";
  html += "<span class='info-value'>" + String(totalLightToday, 2) + " / " + String(REQUIRED_DAILY_LIGHT, 1) + " mol/m²</span>";
  html += "</div>";
  html += "<div class='info-row'>";
  html += "<span class='info-label'>Progress:</span>";
  html += "<span class='info-value'>" + String((int)lightProgress) + "%</span>";
  html += "</div>";
  html += "</div>";
  
  // System info card
  html += "<div class='card full'>";
  html += "<div class='card-title'><span class='icon'>ℹ️</span> System Info</div>";
  html += "<div class='info-row'>";
  html += "<span class='info-label'>WiFi:</span>";
  html += "<span class='info-value'>" + String(ssid) + "</span>";
  html += "</div>";
  html += "<div class='info-row'>";
  html += "<span class='info-label'>IP Address:</span>";
  html += "<span class='info-value'>" + WiFi.localIP().toString() + "</span>";
  html += "</div>";
  html += "<div class='info-row'>";
  html += "<span class='info-label'>Location:</span>";
  html += "<span class='info-value'>" + String(city) + ", " + String(countryCode) + "</span>";
  html += "</div>";
  html += "<div class='info-row'>";
  html += "<span class='info-label'>Light readings:</span>";
  html += "<span class='info-value'>" + String(lightReadings) + "</span>";
  html += "</div>";
  html += "</div>";
  
  html += "</div></div>";
  html += "</body></html>";

  server.send(200, "text/html", html);
}

void handleData() {
  DynamicJsonDocument doc(512);
  doc["moisture"] = lastMoisturePercent;
  doc["light"] = currentLight;
  doc["dailyLight"] = totalLightToday;
  doc["weather"] = weatherCondition;
  doc["advice"] = sunAdvice;
  
  String json;
  serializeJson(doc, json);
  server.send(200, "application/json", json);
}
```
if everything works you should see something like this

<img width="540" height="1776" alt="guidepicture" src="https://github.com/user-attachments/assets/64680484-b63d-43c8-834f-bc412323dee8" />

inside the serial moniter it will also display some messages.
<img width="923" height="15" alt="image" src="https://github.com/user-attachments/assets/1a8f3195-3d85-47a2-b7b8-4a52a2713f2d" />

feel free to add anything else to your own project you can add a message that will be send to your phone or give your plant a voice if you like.
