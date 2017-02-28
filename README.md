# BETA: NOT READY FOR PRODUCTION

SeismoCloud project: http://www.seismocloud.com

# Supported boards

* NodeMCU 1.0 devkit (with ESP-12E module - ESP8266)
* Arduino/Genuino Uno (with Ethernet Shield)
* Arduino Ethernet
* Arduino MEGA2560 (with Ethernet Shield) - untested

Please mind that Arduino cannot update itself, so its use is discouraged in non-lab projects. If you plan to build place-and-forget devices please use **NodeMCU** or **Raspberry PI/Galileo** version: https://github.com/sapienzaapps/galileo-terremoti

## Hardware: assemble a NodeMCU

### Requirements for NodeMCU

* Arduino/Genuino IDE with ESP8266 sdk installed. If you haven't ESP8266 sdk:
	* Open *Preferences* window (from *File* menu)
	* Enter `http://arduino.esp8266.com/stable/package_esp8266com_index.json` into *Additional Board Manager URLs* field. You can add multiple URLs, separating them with commas.
	* Close with "OK", open *Boards Manager* from *Tools* > *Board* menu and install *esp8266* platform (and don't forget to select *NodeMCU 1.0 (ESP-12E)* board from *Tools* > *Board* menu after installation).
* NodeMCU 1.0 devkit board with ESP-12E module
* MPU6050 Accelerometer
* (optional) 3 LEDs (red-green-yellow) with 3 resistors

### Accelerometer MPU6050

Link these pins from Accelerometer MPU6050 to NodeMCU board:

* 3v3: 3v3
* GND: GND
* SDA: D1
* SCL: D2

### (optional) LEDs

Remember to put a resistor with LED (after/before is not really important) to limit
current flowing, otherwise you may damage the NodeMCU board.

By default, LED pins are:

* Pin D5 : Green
* Pin D6 : Yellow
* Pin D7 : Red

## Hardware: assemble an Arduino/Genuino

### Requirements for Arduino/Genuino

* Arduino/Genuino IDE
* Arduino/Genuino board (see *Supported board* chapter)
* WizNet-compatible Ethernet shield (with SD reader)
* MMA7361 Accelerometer
* (optional) 3 LEDs (red-green-yellow) with 3 resistors
* (optional; strongly recommended) Arduino ISP and SD-card (any size >= 10 MB)

We recommend to buy Arduinos on official website https://store.arduino.cc

### Accelerometer MMA7361

Link these pins from Accelerometer MMA7361 to Arduino board:

* Vin: 5v
* GND: GND
* SEL: GND
* X: A0
* Y: A1
* Z: A2

Loop back **3v3** pin to **SLP** on Accelerometer.

### (optional) LEDs

Remember to put a resistor with LED (after/before is not really important) to limit
current flowing, otherwise you may damage the Arduino board.

By default, LED pins are:

* Pin 2 : Yellow
* Pin 5 : Green
* Pin 3 : Red

### Burning a new bootloader with Arduino ISP for self-upgrade

TODO: avr_boot

## Software

### Network requirements

If you have any firewall in your network, please allow these ports:

* TCP: 8883 (outgoing)

### Build and upload code

For NodeMCU, you need to download `WifiManager` library

1. Download the source code (for stable releases, please checkout latest git tag)
2. Open project in Arduino IDE
3. Choose the right **Port** and **Board** into **Tools** menu
4. Compile and upload (2nd button below menus) in your board
5. Unplug the board from PC and plug Accelerometer and Leds, then power on the board
6. For NodeMCU boards, connect to `SeismoCloud` Wi-Fi network and configure Wi-Fi client network parameters. On save, the board reboots and try to connect to Wi-Fi network. If it fails, you can reconnect to `SeismoCloud` network and modify/fix network parameters.
7. Open SeismoCloud app, connect to the same network of the board and register your device. Enjoy!

### LED status description

LEDs can be in these different states:

* **Green**: device is ready
* **Green + Yellow**: device is ready but there is an issue connecting to SeismoCloud APIs
* **Green + Red (only for about 5 seconds)**: shake detected
* **Yellow ONLY - blinking**: no position available - initialize Seismometer with Android/iOS App
* **Green + Yellow + Red**: software is loading
* **Green + Yellow + Red - ALL blinking fast**: software is loaded, starting accelerometer
* **Green + Yellow + Red - ALL blinking slow**: network init failed
* **Yellow + Red - ALL blinking**: EEPROM failed
