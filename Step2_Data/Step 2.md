
| **Type/Output** | **Component**               | Type     | **Description**                                            | Constraints                                                                                    | **Operational Parameters**                                                                | **Sample Values**                                        |
| --------------- | --------------------------- | -------- | ---------------------------------------------------------- | ---------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| **Input**       | Real Time Clock (RTC)       | str      | Provides current date and time                             | Must us valid time and date format                                                             | Time format DD/MM/YYYY, HH:MM; checked every 1 second                                     | `11/08/2025, 13:00`                                      |
| **Input**       | Bowl Weight Sensor (BWS)    | int      | Measures the weight of food in the bowl                    | Bowl weight cannot be negative value                                                           | Threshold: >1g indicates food present, sampled after dispensing                           | `0g` (empty), `50g` (food)                               |
| **Input**       | Food Hopper Sensor (FHS)    | int      | Measures remaining food quantity in hopper                 | Hopper cannot be negative value. Must have >=100g to dispense.                                 | Minimum level: 100g to allow dispensing                                                   | `700g`                                                   |
| **Input**       | Manual Override Button      | bool     | Physical button or app feature                             | Press cannot trigger a dispense if hopper is empty or system is in an error state              | Button press detected within 30 seconds window                                            | `False` (not pressed)                                    |
| **Input**       | Configuration via App       | list     | Settings input via Bluetooth or Wi-Fi from an external app | Must be a a complete list, containing; Feeding number, feeding time (24hr) & dispense duration | Feeding times in 24-hour format; dispense time in seconds                                 | `1`, `08:00`, `10` (seconds)                             |
| **Output**      | Servo Motor Control         | bool     | Controls the servo that dispenses food                     | limitations on range of motion, torque, speed, and duty cycle                                  | Rotates to open for configured dispense duration then rotates to close (e.g., 10 seconds) | `True` (Rotate to "open" and Rotate to "close"), `False` |
| **Output**      | Staff Alert System          | bool+str | Sends error alerts via UI and app                          | Must only trigger when device has recorded an error                                            | Alerts triggered on error states with specific error codes                                | `Error with Device 001 "ER01" Hopper Empty               |
| **Output**      | User Interface (UI_Display) | str      | LCD display showing system status and errors               | Must update every cycle to show current state.                                                 | Displays messages, updates on state changes                                               | `Running`, `Error: ER04`                                 |
| **Output**      | Event Log                   | list     | Text log saved locally and accessible via app              | No Constraint                                                                                  | Log entries formatted with date/time, user data, sensor data, event type, error codes     | *See Log Example*                                        |

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






