
| **Type**   | **Component**               | **Description**                                            | **Purpose**                                                                              | **Operational Parameters**                                                                | **Sample Values**                                        |
| ---------- | --------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| **Input**  | Real Time Clock (RTC)       | Provides current date and time                             | Used to check scheduled feeding times and trigger dispensing                             | Time format DD/MM/YYYY, HH:MM; checked every 1 second                                     | `11/08/2025, 13:00`                                      |
| **Input**  | Bowl Weight Sensor (BWS)    | Measures the weight of food in the bowl                    | Detects presence of food and confirm dispensing success                                  | Threshold: >1g indicates food present, sampled after dispensing                           | `0g` (empty), `50g` (food)                               |
| **Input**  | Food Hopper Sensor (FHS)    | Measures remaining food quantity in hopper                 | Ensures enough food is available before dispensing                                       | Minimum level: 100g to allow dispensing                                                   | `700g`                                                   |
| **Input**  | Manual Override Button      | Physical button or app feature                             | Allows manual activation of dispensing outside scheduled times                           | Button press detected within 30 seconds window                                            | `False` (not pressed)                                    |
| **Input**  | Configuration via App       | Settings input via Bluetooth or Wi-Fi from an external app | Provides user settings such as feeding frequency, scheduled times, and dispense duration | Feeding times in 24-hour format; dispense time in seconds                                 | `1`, `08:00`, `10` (seconds)                             |
| **Output** | Servo Motor Control         | Controls the servo that dispenses food                     | Opens/closes servo to release measured amount of food                                    | rotates to open for configured dispense duration then rotates to close (e.g., 10 seconds) | `True` (Rotate to "open" and Rotate to "close"), `False` |
| **Output** | Staff Alert System          | Sends error alerts via UI and app                          | Notifies staff of errors like empty hopper, no food dispensed, or animal not eating      | Alerts triggered on error states with specific error codes                                | `Error with Device 001 "ER01" Hopper Empty               |
| **Output** | User Interface (UI_Display) | LCD display showing system status and errors               | Provides visual feedback to users/staff                                                  | Displays messages, updates on state changes                                               | `Running`, `Error: ER04`                                 |
| **Output** | Event Log                   | Text log saved locally and accessible via app              | Records timestamped events including errors, feeding actions, and settings               | Log entries formatted with date/time, user data, sensor data, event type, error codes     | *See Log Example*                                        |

Log Example :

	Date & Time: 01/01/2020 08:00
	Device ID: 001
	Config DATA: 
		Amount of times per day to feed : 1
		Feeding Time : 0800
		Dispense Duration: 10secs
		Food in Hopper: 1300g
		Food In Bowl: 0g
	Sensor Data:
		Hopper Weight: 1200g
		Bowl Weight: 0g
	Dispense Type: scheduled
	Error State: FALSE





