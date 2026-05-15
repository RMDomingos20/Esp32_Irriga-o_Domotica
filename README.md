# Automated Irrigation System with ESP32

<p align="center">
  <img src="https://img.shields.io/badge/Platform-ESP32-blue?style=for-the-badge&logo=espressif" />
  <img src="https://img.shields.io/badge/Language-C%2B%2B-00599C?style=for-the-badge&logo=cplusplus" />
  <img src="https://img.shields.io/badge/IDE-Arduino_IDE-00979D?style=for-the-badge&logo=arduino" />
  <img src="https://img.shields.io/badge/WebServer-ESPAsyncWebServer-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Architecture-IoT-success?style=for-the-badge" />
</p>

<p align="center">
  Academic project developed for the Home Automation course<br>
  Control and Automation Engineering — IFSP Bragança Paulista
</p>

---

## Overview

This project is an automated irrigation system based on the ESP32 microcontroller. The system monitors soil moisture using a resistive sensor and automatically controls irrigation through a relay module connected to a 127V solenoid valve.

A local web server running directly on the ESP32 allows real-time monitoring from any device connected to the same Wi-Fi network. The interface displays soil moisture percentage, ADC readings, irrigation status, and configured control thresholds.

The project was developed as part of the Home Automation discipline in the 5th semester of the Control and Automation Engineering course at IFSP — Bragança Paulista campus.

---

## Features

- Automatic irrigation control
- Soil moisture monitoring
- Hysteresis-based switching logic
- Embedded web interface hosted on ESP32
- Real-time updates via Wi-Fi
- Responsive interface for desktop and mobile devices
- Manual calibration of moisture thresholds
- Display of ADC readings and control parameters

---

## Technologies

| Technology | Description |
|---|---|
| C++ | Main programming language |
| ESP32 | Main microcontroller |
| Arduino IDE | Development environment |
| HTML / JavaScript | Embedded web interface |
| ESPAsyncWebServer | Asynchronous web server |
| AsyncTCP | TCP communication library |

---

## Hardware Architecture

The system uses an ESP32 DevKit V1 as the central controller. Soil moisture is measured through an analog resistive sensor connected to GPIO 35, while irrigation control is performed using a 5V relay module connected to GPIO 5.

The relay switches a normally closed 127V AC solenoid valve responsible for controlling water flow.

---

## Connections

### Soil Moisture Sensor → ESP32

| Sensor | ESP32 |
|---|---|
| VCC | 3.3V |
| GND | GND |
| A0 | GPIO 35 |

> If the sensor is powered with 5V, a voltage divider is recommended on the analog output.

---

### Relay Module → ESP32

| Relay | ESP32 |
|---|---|
| VCC | 5V |
| GND | GND |
| IN | GPIO 5 |

---

### Solenoid Valve → Relay → AC Power

| Valve | Relay | AC Line |
|---|---|---|
| Terminal 1 | COM | Phase |
| Terminal 2 | NO | Neutral |

---

## Control Logic

The irrigation system uses hysteresis to avoid rapid switching caused by small oscillations in soil moisture.

```cpp
float LIMIAR_LIGAR = 30.0;
float LIMIAR_DESLIGAR = 40.0;

int seco = 4095;
int molhado = 1400;
```

The relay is activated when moisture falls below the minimum threshold and deactivated when the upper threshold is reached.

---

## Required Libraries

```cpp
#include <WiFi.h>
#include <AsyncTCP.h>
#include <ESPAsyncWebServer.h>
```

---

## ESPAsyncWebServer Compatibility Fix

When using ESP32 Core 3.2.0 or newer, the following error may occur:

```cpp
error: call of overloaded 'IPAddress(unsigned int)' is ambiguous
```

To fix it, edit:

```txt
/Arduino/libraries/ESP_Async_WebServer/src/AsyncWebSocket.cpp
```

Replace:

```cpp
return IPAddress(0U);
```

With:

```cpp
return IPAddress((uint32_t)0);
```

---

## Project Structure

```txt
Esp32_Irrigacao_Domotica
├── irrigacao_esp32_sem_wifi.ino
├── irrigacao_esp32_wifi.ino
├── drivers
│   └── [...]
├── Trabalho Teorico
│   └── Trabalho Irrigacao
└── README.md
```

---

## Running the Project

1. Install the required libraries in Arduino IDE:
   - ESPAsyncWebServer
   - AsyncTCP

2. Configure your Wi-Fi credentials:

```cpp
const char* ssid = "YOUR_WIFI";
const char* password = "YOUR_PASSWORD";
```

3. Select the board:

```txt
ESP32 Dev Module
```

4. Compile and upload the code to the ESP32.

5. Open the Serial Monitor and access the IP address displayed by the board.

---

## Applications

This project can be adapted for residential irrigation systems, greenhouses, smart gardens, and general IoT automation studies involving embedded systems and wireless monitoring.

---

## Authors

- Rafael Domingos Siqueira Magalhães
- Guilherme Gabriel Franco de Souza
- Jonathan Alexandre de Moraes Cândido

---

## Institution

Federal Institute of Education, Science and Technology of São Paulo — IFSP  
Bragança Paulista Campus  
Control and Automation Engineering

---

## Repository

```txt
https://github.com/RMDomingos20/Esp32_Irriga-o_Domotica
```
