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


 STEP 1: Install Required Libraries
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

