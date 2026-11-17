# Cybersecurity : CSN150

## Name of Project

ESP32-CAM Access Point (AP) Web Server

## Purpose

Set up the ESP32-CAM as a Wi-Fi Access Point so devices can connect directly to it without a router, and control the board from a web page.

## Equipment

* ESP32-CAM (AI Thinker) with ESP32-CAM-MB programmer board
* OV2640 camera module
* USB Micro Data Cable
* MacBook (Apple Silicon)
* iPhone (to connect to the Access Point)

## Links to documentation and tools

**Other Links:**
* https://randomnerdtutorials.com/esp32-cam-access-point-ap-web-server/

**AI GPTs used:** Claude

## Steps I followed

1. Opened File → Examples → ESP32 → Camera → CameraWebServer in Arduino IDE.
2. In board_config.h, turned on `CAMERA_MODEL_AI_THINKER` and turned off the other camera models.
3. Set the network name and password:
   `const char *ssid = "ESP32-CAM-ISHMAEL";`
   `const char *password = "123456789";`
4. Replaced `WiFi.begin()` (which connects to a router) with `WiFi.softAP(ssid, password)`, which makes the ESP32 its own Access Point.
5. Uploaded the sketch, but the camera failed to start (see Problems and Solutions).
6. Wrote a new sketch, `esp32_ap_webserver`, that creates the Access Point and hosts a web page without the camera. The page has buttons to turn the flash LED (GPIO 4) on and off.
7. Uploaded it and opened the Serial Monitor at 115200 baud. It printed: `Access Point ready! Connect to 'ESP32-CAM-ISHMAEL' and open http://192.168.4.1`
8. On my iPhone, joined the ESP32-CAM-ISHMAEL Wi-Fi network with the password.
9. Opened Safari, went to 192.168.4.1, and used the buttons to turn the LED on and off.

## Problems and Solutions

**Problem:** board_config.h had ESP_EYE selected by default, and while editing I accidentally turned on TTGO_T_JOURNAL instead of AI_THINKER. **Solution:** I made sure only `#define CAMERA_MODEL_AI_THINKER` had no `//` in front of it.

**Problem:** After pasting the Access Point code, the `}` that closes `setup()` was deleted. **Solution:** I added the `}` back, and the sketch compiled.

**Problem:** The camera came off the board and the ribbon cable was torn. The Serial Monitor showed `Camera probe failed with error 0x106 (ESP_ERR_NOT_SUPPORTED)` and `Camera init failed with error 0x106`. Because the camera check failed, the code stopped before creating the Access Point. **Solution:** The camera module needs to be replaced. To still complete the Access Point, I wrote a sketch that doesn't use the camera. It creates the same Access Point and hosts a web page that controls the flash LED.

## Final Report

I set up the ESP32-CAM as a Wi-Fi Access Point. In Access Point mode, the ESP32 creates its own Wi-Fi network instead of joining a router (Station mode). My iPhone found ESP32-CAM-ISHMAEL in its Wi-Fi list, connected with the password, and opened the ESP32's web page at 192.168.4.1. The page showed how many devices were connected and let me turn the flash LED on and off from my phone. The phone showed "No Internet" because the ESP32 is a standalone network that isn't connected to the internet.

The camera stream didn't work because the camera ribbon cable was torn, causing "Camera init failed with error 0x106." This taught me how to read Serial Monitor errors and how a hardware problem can stop code from running even when the code is correct. Once the OV2640 camera is replaced, the CameraWebServer Access Point code should stream video too.
