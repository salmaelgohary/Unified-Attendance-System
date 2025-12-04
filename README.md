# Unified Attendance System

**ESP32 + MFRC522 RFID + 16×2 I2C LCD**

This system scans RFID cards and logs attendance data directly to a Google Sheet using Wi-Fi and an Apps Script API. User results are displayed on the LCD with buzzer + LED visual feedback.


## Install Requirements

### Arduino Libraries

Install via **Arduino IDE → Tools → Manage Libraries**:

* **MFRC522 by GithubCommunity** (RFID reader)
* **LiquidCrystal I2C by Frank de Brabander** (LCD display) (Note: you may see the following warning when compiling "WARNING: library LiquidCrystal_I2C claims to run on avr architecture(s) and may be incompatible with your current board which runs on esp32 architecture(s).". This can be safely ignored the library works fully with ESP32.

Included by default with ESP32 board support:

* `WiFi.h`
* `WiFiClientSecure.h`
* `SPI.h`
* `Wire.h`
* `time.h`

### ESP32 Board Package

> Tools → Board Manager → Search **“ESP32 by Espressif Systems”** → Install


## Hardware Setup

### RFID Reader → ESP32

| RFID Pin | ESP32 Pin |
| -------- | --------- |
| SDA/SS   | 4         |
| SCK      | 18        |
| MOSI     | 23        |
| MISO     | 19        |
| RST      | 16        |
| 3.3V     | 3.3V      |
| GND      | GND       |

### LCD (I2C) → ESP32

| LCD Pin | ESP32 Pin |
| ------- | --------- |
| SDA     | 21        |
| SCL     | 22        |
| VCC     | 5V        |
| GND     | GND       |

### Status LEDs + Buzzer

| Component | ESP32 Pin |
| --------- | --------- |
| Green LED | 17        |
| Red LED   | 5         |
| Buzzer    | 15        |


## Wi-Fi Configuration

If the network changes, update the credentials in the code:

```cpp
#define WIFI_SSID "Your WiFi Name"
#define WIFI_PASSWORD "Your Password"
```


## Google Sheets Integration

If you happen to modify the apps script in the future, replace your Script Web App URL here to change the upload destination:

```cpp
const String sheet_url = "YOUR_SCRIPT_URL?name=";
```

As long as the main code is uploaded into the ESP32 and it has power and Wi-Fi access, it will **upload attendance logs without needing a computer connected**.

You can simply plug the system into a wall outlet using a USB power adapter.


## USB Driver Fix (Windows)

If your computer does not detect the ESP32 when plugged in via USB, you won’t be able to upload or modify code. This is usually due to a missing USB-to-UART driver. Follow these steps to fix it: 

Install the **CP210x USB-to-UART driver**:

1. Download the **CP210x Windows Drivers** from Silicon Labs [https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers](https://www.silabs.com/software-and-tools/usb-to-uart-bridge-vcp-drivers?tab=downloads)
2. Extract the Zip file
3. Run `CP210xVCPInstaller_x64.exe` (or `_x86.exe` for 32-bit)
4. Unplug and Replug ESP32
5. Navigate to **Device Manager → Ports**, and you should now the USB connection and its corresponding COM port:

```
Silicon Labs CP210x USB to UART Bridge
```

5. In Arduino IDE → Tools → **Port** → select the detected COM port


## Adding / Updating Cardholders

1. Open `RFID-Attendance.cardholder_configure.ino`
2. Edit the **name** in code (max **16 characters**, including spaces)
3. Select the COM port (see Device Manager if unsure)
4. Upload the cardholder configure sketch to the ESP32
5. Open Serial Monitor
6. Scan the RFID tag, the serial monitor should display the fedback updating the name stoed in the card
7. Re-upload the main-code `RFID-Attendance_final.ino` program when done


## Operation

1. Power the ESP32 using:

   * A **USB wall adapter**, **OR**
   * A PC connection if you want to set it up with the PC and live Google Sheet
2. Open the linked Google Sheet to view live logs
3. Scan a card and the sheet will update in real time
5. Delete all rows at end of day for a clean sheet for the next day

