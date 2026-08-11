# ESP32 Web-Based LED Lighting System

A simple **web-based LED lighting control system using ESP32**, developed as a project for the **Microprocessor and Interfacing** course.

The ESP32 creates its own Wi-Fi network and runs a lightweight web server. Users can connect to the ESP32 using a smartphone or computer and control two LEDs through a web browser.

## Features

* 📡 ESP32 operates as a standalone Wi-Fi Access Point
* 🌐 Built-in HTTP web server
* 💡 Independent control of two LEDs
* 🔘 Browser-based ON/OFF controls
* 📱 Can be controlled from a smartphone, laptop, or tablet
* 🔌 Uses ESP32 GPIO pins 16 and 17
* 🎓 Designed as an educational Microprocessor and Interfacing project

---

## Project Overview

This project demonstrates a basic **IoT-style wireless lighting control system** using an ESP32.

Instead of connecting the ESP32 to an existing Wi-Fi router, the ESP32 creates its own wireless network using **SoftAP (Software Access Point)** mode.

Once a device connects to the ESP32's Wi-Fi network, the user can access a web page hosted directly by the ESP32. The web page contains buttons for controlling two LEDs.

### System Architecture

```text
                    ┌─────────────────────┐
                    │     Smartphone      │
                    │    / Laptop / PC    │
                    └──────────┬──────────┘
                               │
                         Wi-Fi Connection
                               │
                               ▼
                    ┌─────────────────────┐
                    │        ESP32        │
                    │                     │
                    │   Wi-Fi Access     │
                    │       Point         │
                    │          +          │
                    │    Web Server       │
                    └──────────┬──────────┘
                               │
                        HTTP Requests
                               │
                 ┌─────────────┴─────────────┐
                 │                           │
                 ▼                           ▼
          ┌─────────────┐             ┌─────────────┐
          │   GPIO 16   │             │   GPIO 17   │
          │             │             │             │
          │    LED 1    │             │    LED 2    │
          └─────────────┘             └─────────────┘
```

---

## Hardware Requirements

| Component               |    Quantity |
| ----------------------- | ----------: |
| ESP32 Development Board |           1 |
| LED                     |           2 |
| 220Ω–330Ω Resistor      |           2 |
| Breadboard              |           1 |
| Jumper Wires            | As required |
| USB Cable               |           1 |

---

## Circuit Connections

### LED 1

Connect LED 1 to **GPIO 16**:

```text
ESP32 GPIO 16 ── Resistor ── LED ── GND
```

### LED 2

Connect LED 2 to **GPIO 17**:

```text
ESP32 GPIO 17 ── Resistor ── LED ── GND
```

### Pin Configuration

| Device | ESP32 GPIO | Function       |
| ------ | ---------: | -------------- |
| LED 1  |    GPIO 16 | Digital Output |
| LED 2  |    GPIO 17 | Digital Output |

> **Note:** Always use an appropriate current-limiting resistor with each LED.

---

## Software Requirements

* [Arduino IDE](https://www.arduino.cc/en/software)
* ESP32 board support package for Arduino IDE
* ESP32 development board
* USB cable
* Web browser

The project uses the ESP32 `WiFi.h` library.

---

## Project Structure

```text
ESP32-Web-Based-LED-Lighting-System/
│
├── ESP32_Web_LED_Control/
│   └── ESP32_Web_LED_Control.ino
│
├── images/
│   ├── circuit-diagram.png
│   └── web-interface.png
│
└── README.md
```

The `images` folder is optional. You can add photographs of your actual circuit and screenshots of the web interface there.

---

## How It Works

### 1. ESP32 Creates a Wi-Fi Network

The ESP32 is configured as a Wi-Fi Access Point using:

```cpp
WiFi.softAP(ssid, password);
```

The default network credentials are:

```text
SSID:      ESP32-Network
Password:  Esp32-Password
```

---

### 2. Web Server Starts

The ESP32 creates a web server on port `80`:

```cpp
WiFiServer server(80);
```

After starting the server, the ESP32 prints its IP address to the Serial Monitor:

```cpp
Serial.println(WiFi.softAPIP());
```

The default SoftAP address is commonly:

```text
192.168.4.1
```

However, always use the IP address printed by your ESP32.

---

### 3. Connect to the ESP32

From a smartphone or computer, open the Wi-Fi settings and connect to:

```text
ESP32-Network
```

Use the password:

```text
Esp32-Password
```

---

### 4. Open the Web Interface

Open a web browser and enter:

```text
http://192.168.4.1
```

or use the IP address displayed in the Serial Monitor.

You should see a web page similar to:

```text
       ESP32 Web Server

      Control LED State

          [ ON ]

          [ ON ]
```

Each button independently controls one LED.

---

## HTTP Control Mechanism

The web interface sends simple HTTP GET requests to the ESP32.

### LED 1 — GPIO 16

Turn ON:

```text
GET /16/on
```

Turn OFF:

```text
GET /16/off
```

The ESP32 handles these requests using:

```cpp
if (header.indexOf("GET /16/on") >= 0) {
    statePin16 = "on";
    digitalWrite(ledPin16, HIGH);
}
else if (header.indexOf("GET /16/off") >= 0) {
    statePin16 = "off";
    digitalWrite(ledPin16, LOW);
}
```

### LED 2 — GPIO 17

Turn ON:

```text
GET /17/on
```

Turn OFF:

```text
GET /17/off
```

The ESP32 then controls GPIO 17 accordingly.

---

## Getting Started

### Step 1 — Install Arduino IDE

Download and install the Arduino IDE:

https://www.arduino.cc/en/software

### Step 2 — Install ESP32 Board Support

Add ESP32 board support to the Arduino IDE and select your ESP32 development board.

For example:

```text
Tools → Board → ESP32 Arduino → ESP32 Dev Module
```

### Step 3 — Connect the ESP32

Connect the ESP32 to your computer using a USB cable.

Select the appropriate COM/serial port:

```text
Tools → Port
```

### Step 4 — Upload the Code

Open:

```text
ESP32_Web_LED_Control.ino
```

Compile and upload the program to the ESP32.

### Step 5 — Open Serial Monitor

Open:

```text
Tools → Serial Monitor
```

Set the baud rate to:

```text
115200
```

You should see something similar to:

```text
IP address:
192.168.4.1
```

### Step 6 — Connect to the ESP32 Network

Connect your phone or computer to:

```text
Wi-Fi: ESP32-Network
Password: Esp32-Password
```

### Step 7 — Control the LEDs

Open the ESP32 IP address in your browser:

```text
http://192.168.4.1
```

Use the buttons to turn the LEDs ON and OFF.

---

## Program Flow

```text
             START
               │
               ▼
      Initialize Serial
               │
               ▼
      Configure GPIO 16/17
               │
               ▼
      Create Wi-Fi Access Point
               │
               ▼
        Start Web Server
               │
               ▼
       Wait for Web Client
               │
               ▼
       Receive HTTP Request
               │
        ┌──────┴──────┐
        │             │
        ▼             ▼
   GPIO 16         GPIO 17
        │             │
    ┌───┴───┐     ┌───┴───┐
    ▼       ▼     ▼       ▼
   ON      OFF   ON      OFF
    │       │     │       │
    └───┬───┘     └───┬───┘
        │             │
        └──────┬──────┘
               ▼
       Generate Web Page
               │
               ▼
       Send Response
               │
               ▼
          Close Client
               │
               ▼
            Repeat
```

---

## Code Highlights

### Wi-Fi Access Point

```cpp
const char* ssid = "ESP32-Network";
const char* password = "Esp32-Password";

WiFi.softAP(ssid, password);
```

This allows the ESP32 to create its own wireless network.

### Web Server

```cpp
WiFiServer server(80);
```

The ESP32 listens for HTTP requests on port `80`.

### GPIO Configuration

```cpp
const int ledPin16 = 16;
const int ledPin17 = 17;

pinMode(ledPin16, OUTPUT);
pinMode(ledPin17, OUTPUT);
```

### LED Control

```cpp
digitalWrite(ledPin16, HIGH);
digitalWrite(ledPin16, LOW);

digitalWrite(ledPin17, HIGH);
digitalWrite(ledPin17, LOW);
```

---

## Demonstration

### Circuit


```markdown
![ESP32 LED Circuit](images/circuit-diagram.jpg)
```

### Web Interface

Add a screenshot of the web interface here:

```markdown
![Web Interface](images/web-interface.jpg)
```

---

## Technologies Used

| Technology  | Purpose                 |
| ----------- | ----------------------- |
| ESP32       | Microcontroller         |
| C/C++       | Firmware development    |
| Arduino IDE | Programming environment |
| Wi-Fi       | Wireless communication  |
| HTTP        | Web communication       |
| HTML        | Web interface           |
| CSS         | Web interface styling   |

---

## Course Information

**Course:** Microprocessor and Interfacing
**Project:** ESP32 Web-Based LED Lighting System
**Platform:** ESP32
**Programming Language:** C/C++
**Development Environment:** Arduino IDE
**Communication:** Wi-Fi + HTTP

---

## License

This project was developed for **educational purposes** as part of a Microprocessor and Interfacing course.

Feel free to use, modify, and extend the project for learning and academic purposes.
