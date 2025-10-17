Complete Smart Plant Monitor Guide


 What You Need
Hardware
1x NodeMCU 1.0 (ESP-12E Module)
1x Capacitive Soil Moisture Sensor (3-4 wires)
1x BH1750 Light Sensor (I2C module)
1x WS2812B LED Strip (8-12 LEDs)
Micro USB cable
Computer with internet connection
Software
Arduino IDE (free download)
Free OpenWeatherMap account (for weather data)
Optional
External 5V power supply (for 12+ LEDs)
Breadboard for easier connections

 STEP 1: Install Arduino IDE
1.1 Download Arduino IDE
Go to https://www.arduino.cc/en/software
Download the version for your operating system (Windows/Mac/Linux)
Install and open Arduino IDE
✓ Success check: You should see the Arduino IDE window open


 STEP 2: Set Up Arduino for ESP8266
2.1 Add ESP8266 Board Support
In Arduino IDE, go to File → Preferences
Find the field: "Additional Board Manager URLs"
Copy and paste this URL:
 http://arduino.esp8266.com/stable/package_esp8266com_index.json


Click OK
2.2 Install ESP8266 Boards
Go to Tools → Board → Boards Manager
Wait for the list to load
In the search box, type: esp8266
Find "esp8266 by ESP8266 Community"
Click Install (this may take a few minutes)
Wait until it says "Installed"
Close the Boards Manager
✓ Success check: Go to Tools → Board and you should see "NodeMCU 1.0" in the list
2.3 Configure Board Settings
Go to Tools → Board
Scroll down and select "NodeMCU 1.0 (ESP-12E Module)"
Set Upload Speed to 115200
Connect your NodeMCU with USB cable
Select your Port (COM3, COM5, etc. on Windows; /dev/tty.* on Mac)
✓ Success check: The port shows up in Tools → Port menu

 STEP 3: Install Required Libraries
3.1 Open Library Manager
Go to Sketch → Include Library → Manage Libraries
Wait for the list to load
3.2 Install Adafruit NeoPixel Library
In the search box, type: Adafruit NeoPixel
Find "Adafruit NeoPixel by Adafruit"
Click Install
Wait until it says "Installed"
3.3 Install BH1750 Library
In the search box, type: BH1750
Find "BH1750 by Christopher Laws"
Click Install
3.4 Install ArduinoJson Library
In the search box, type: ArduinoJson
Find "ArduinoJson by Benoit Blanchon"
Click Install
Close the Library Manager
✓ Success check: All three libraries show as "INSTALLED" in Library Manager

 STEP 4: Get Weather API Key
4.1 Create OpenWeatherMap Account
Go to https://openweathermap.org/api
Click "Sign Up" (it's FREE!)
Fill in your details:
Email address
Username
Password
Check your email and confirm your account
4.2 Get Your API Key
Log in to OpenWeatherMap
Go to your profile (top right)
Click "My API Keys"
You'll see a key like: a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6
Copy this key - you'll need it soon!
✓ Success check: You have copied your API key
⚠️ Important: It may take 10-15 minutes for your API key to activate. If you get errors later, wait a bit and try again.
