Copilot_Attempt_README.md

# Programmable Pet Feeder System

A smart, scalable, and affordable automated pet feeder designed for shelters and multi-animal facilities. This system dispenses food at scheduled times, monitors food levels, detects errors, and alerts staff — all while being configurable via a central server or app.

---

## Project Overview

This project was developed for an *Intro to IT* assignment to demonstrate embedded systems design, logic development, and networked device integration.  
The feeder unit is powered by a **Raspberry Pi Zero 2 W** and communicates with a central server for configuration, logging, and alerts.

---

## Problem Statement

Animal shelters require a reliable feeder system that:

- Dispenses food at scheduled times.
- Detects and logs feeding events and errors.
- Alerts staff when issues occur (e.g., empty hopper, animal not eating).
- Can be configured remotely via a central dashboard or app.

---

## System Architecture

### Feeder Unit (Raspberry Pi Zero 2 W)
- Runs the core logic and controls sensors and actuators.
- Displays status and errors via LCD.
- Sends logs and error codes to a central server.
- Receives configuration updates from the server.

---

## Hardware Components

| Component                 | Purpose                                           |
|---------------------------|---------------------------------------------------|
| **Raspberry Pi Zero 2 W** | Main controller (logic + communication)           |
| **Load Cell + HX711**     | Bowl and hopper weight sensing                     |
| **Servo Motor (SG90)**    | Dispenses food                                    |
| **Push Button**           | Manual override                                   |
| **I2C LCD Display**       | Displays system status and errors                 |
| **Buzzer/LED**            | Optional local alert system                       |
| **Wi-Fi**                 | Built-in (Pi Zero 2 W) for server communication   |

---

## Core Features

- **Scheduled Feeding** – Dispenses food based on user-defined times.  
- **Manual Override** – Allows staff to trigger feeding manually.  
- **Sensor Monitoring** – Detects food presence in the bowl and hopper.  
- **Error Detection** – Identifies issues like jammed servo, uneaten food, or empty hopper.  
- **Logging** – Records all events with timestamps and sensor data.  
- **Alerts** – Sends error codes to staff via app/server.  
- **Remote Configuration** – Settings can be updated via app or server.  

---

## Sample Test Cases

The system has been tested under scenarios including:

1. Normal operation with successful feeding.  
2. Bowl not empty at feeding time.  
3. Animal not eating within a set window.  
4. Hopper empty or servo jammed.  
5. Multiple errors triggering system shutdown.  

Each test includes expected outputs, logic discussion, and refinement suggestions.

---

## Future Enhancements

- **Retry Logic** – Allow reattempts after certain errors (e.g., bowl not empty).  
- **Configurable Eating Window** – Adjustable time allowed for an animal to eat.  
- **Hopper Weight-Based Dispensing** – More accurate food measurement.  
- **Camera Integration** – Monitor animal behavior.  
- **Battery Backup** – Prevent data loss during power outages.  

---

## File Structure

**/feeder-firmware/**  
├── **main.py** – Core logic for Raspberry Pi Zero 2 W  
├── **config.json** – Device configuration  
├── **log.txt** – Local event log  
└── **README.md** – Project documentation  

---

## Communication Protocol

- **HTTP / MQTT** – Used to send logs and receive config from the server.  
- **JSON** – Standard format for data exchange.  

---

## Tools & Languages

- **Python 3** – Device logic  
- **Flask / Node.js** – Optional server-side  
- **MQTT / HTTP** – Communication protocols  
- **Draw.io** – Flowcharts and diagrams  

---

## Contact

For questions or collaboration, please contact:  

**Ryan Xuereb** 
u3293430@uni.canberra.edu.au
*Intro to IT – Student Developer*
