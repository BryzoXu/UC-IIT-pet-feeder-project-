# Automated Feeder Firmware (Pseudo Code Overview)

This project defines the logic for an automated animal feeder system.  
It handles scheduled and manual feedings, monitors food levels, detects errors, and alerts staff when intervention is needed.

---

## Features

- **Scheduled Feeding**: Dispenses food at predefined times using the real-time clock.
- **Manual Feeding**: Allows staff to trigger feedings via a manual override button.
- **Food Level Monitoring**: Uses sensors to measure food in the hopper and bowl.
- **Error Detection**:
  - `ER01`: Hopper empty
  - `ER02`: Bowl already contains food
  - `ER03`: Food failed to dispense
  - `ER04`: Animal did not eat within set time
- **Alerts**:
  - Displays error messages on the device LCD.
  - Sends notifications via the connected app.
- **Data Logging**:
  - Records timestamp, device ID, settings, sensor data, dispense type, and error codes to `log.txt`.

---

## Program Flow

### 1. Startup
- On boot, prompts the user to enter **Setup Mode**.
- Waits up to 5 seconds for a manual trigger press to connect to the app.
- If connected, downloads updated settings from the app; otherwise, uses existing configuration.
- Begins normal operation.

### 2. Main Loop
- Continuously runs while `Is_Working = True`.
- Checks for scheduled feeding times using the real-time clock.
- If not feeding time, allows a 30-second window for manual feeding requests.
- Executes **Dispense_FOOD** routine if triggered.

### 3. Dispense Process
- Reads configuration and sensor data.
- Verifies hopper has enough food (`> 100g`).
- Confirms bowl is empty before dispensing.
- Operates the servo to dispense food for the configured duration.
- Checks if bowl weight increases; if not, records an error.

### 4. Post-Feeding Check
- Waits 10 minutes after dispensing.
- If food remains in the bowl, logs `ER04` and alerts staff.

### 5. Error Handling
- Alerts staff through both the LCD display and the connected app.
- Increments `error_count` for each detected issue.
- If `error_count >= 3`, stops operation and displays **"ERROR LIMIT REACHED!"**.

### 6. Logging
- After each cycle, records:
  - Timestamp
  - Device ID
  - Current settings
  - Sensor readings
  - Dispense type (scheduled/manual)
  - Error state and code
- Saves the data to `log.txt`.

---

## Variables

| Variable        | Description |
|-----------------|-------------|
| `Device_id`     | Unique feeder ID |
| `error_code`    | Current error code |
| `error_state`   | Boolean indicating if an error exists |
| `Is_Working`    | Controls main loop operation |
| `error_count`   | Counts consecutive errors before shutdown |

---

## Subprocesses

- **`Start_Up`** — Handles setup mode and configuration loading.
- **`Dispense_FOOD`** — Manages dispensing and error detection.
- **`Alert_Staff`** — Sends alerts to LCD and app.
- **`Write_LOG`** — Records operational data to a log file.

---

## Notes

- This pseudo code outlines the **control logic** only; hardware-specific code (e.g., sensor reads, servo control) is implementation-dependent.
- Designed for **reliability** — stops automatically after multiple failures to prevent further errors.

