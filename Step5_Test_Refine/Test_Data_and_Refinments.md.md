### Sample Test Data

1)  **Animal Eat as expected and device operates as expected**
*Inputs*
 - Time: `11/08/2025, 08:00`
 - BWS: `0g`
 - FHS :`1300g'
 - Manual Override: `False`
 - Config 
	- Number of scheduled times: `1`
	- Specific times(24hr): `0800`
	- Dispense Durations(Seconds): `10`

*Expected Outputs*
- Servo Controls:  open for 10 seconds 
- Staff Alert system: no alert sent
- UI_Display: **"Running"**
- Log: 
	Date & Time: 11/08/2025, 08:00
	Device ID: 001
	Config DATA: 
		Amount of times per day to feed : 1
		Feeding Time : 0800
		Dispense Duration: 10secs
	Sensor Data:
		Hopper Weight: 1150g
		Bowl Weight: 0g
	Dispense Type: scheduled
	Error State: FALSE
	
*Logic Discussion:*
Device operated as expected, Scheduled feeding at 8am only, servo opened for 10 seconds, bowl weight was 0grams after 10 mins confirming the animal ate all food. No errors confirm that the food dispensed properly and that the was nothing in the bowl and enough food in the hooper at feeding time. 
*Refinement*
No refinements needed, logic performs as intended in this scenario.

---

2) **Something in animal bowl **
*Inputs*
 - Time: `10/08/2025, 16:00`
 - BWS: `70g`
 - FHS : `1300g`
 - Manual Override: `False`
 - Config 
	- Number of scheduled times: `2`
	- Specific times(24hr): `0800, 1600`
	- Dispense Durations(Seconds): `10`

*Expected Outputs*
- Servo Controls: Doesn't Operate
- Staff Alert system: Send Error "ER02"
- UI_Display: **"Error ER02"**
- Log: 
	Date & Time: 10/08/2025, 08:00
	Device ID***: 001
	Config DATA: 
		Amount of times per day to feed : 1
		Feeding Time : 0800 & 1600
		Dispense Duration: 10secs
	Sensor Data:
		Hopper Weight: 1300g
		Bowl Weight: 70g
	Dispense Type: scheduled
	Error State: TRUE, ER02
	
*Logic Discussion:*
2nd Scheduled feeding time was triggered correctly but there is something remaining in the animals bowl causing the bowl sensor to still read 70g, system triggered Error State ER02 alerting staff and not dispensing any more food to avoid over feeding 
*Refinement*
Adding a retry function to the system to allow it to try a scheduled dispense again when in ER02, currently anytime there is an error the system doesn't attempt to feed the animal again until the next scheduled feeding time. A retry function would allow the animal time to eat any leftover while still ensuring they are receiving the intended amount of food. 

---

3) **Animal Doesn't Eat**
*Inputs*
 - Time: `11/08/2025, 08:00`
 - BWS: `0g`
 - FHS :`1300g`
 - Manual Override: `False`
 - Config 
	- Number of scheduled times: `1`
	- Specific times(24hr): `0800`
	- Dispense Durations(Seconds): `10`

*Expected Outputs*
- Servo Controls:  open for 10 seconds 
- Staff Alert system: Send Error "ER04"
- UI_Display: **"Error ER04"**
- Log: 
	Date & Time: 11/08/2025, 08:00
	Device ID: 001
	Config DATA: 
		Amount of times per day to feed : 1
		Feeding Time : 0800
		Dispense Duration: 10secs
	Sensor Data:
		Hopper Weight: 1150g
		Bowl Weight: 150g
	Dispense Type: scheduled
	Error State: TRUE, ER04
	
*Logic Discussion:*
System operated as intended, successfully dispensing food as requested but the animal didn't eat all of the food within the current 10 mins waiting time triggering error ER04 and alerting staff.  

*Refinement*
Currently the eating window is hard codes as 10 mins, its possible some animals may take longer then 10 mins to eat, leading to unnecessary staff alerts. To reduce these alerts the "Eating window variable" could be included in part of the config files.  

---

4) **Food Hopper Empty**
*Inputs*
 - Time: `11/08/2025, 08:00`
 - BWS: `0g`
 - FHS : `60g`
 - Manual Override: `False`
 - Config 
	- Number of scheduled times: `1`
	- Specific times(24hr): `0800`
	- Dispense Durations(Seconds): `10`

*Expected Outputs*
- Servo Controls:  Doesn't Operate
- Staff Alert system: Send Error "ER01"
- UI_Display: **Error "ER01"**
- Log: 
	Date & Time: 11/08/2025, 08:00
	Device ID: 001
	Config DATA: 
		Amount of times per day to feed : 1
		Feeding Time : 0800
		Dispense Duration: 10secs
	Sensor Data:
		Hopper Weight: 60g
		Bowl Weight: 0g
	Dispense Type: scheduled
	Error State: TRUE, ER01
	
*Logic Discussion:*
Scheduled feeding time triggered successfully but when attempted to dispenses food the system noted the hopper didn't have enough food remaining (<100gram), triggering Error ER02, alerting staff.  
*Refinement*
Including monitoring system to notify staff before the device runs out of food, preventing animals from missing a scheduled feed and allowing staff time to refill hoppers. 
	
---

5) **Food Doesn't Dispense (Device Jammed Closed)
*Inputs*
 - Time: `11/08/2025, 07:00`
 - BWS: `0g`
 - FHS :`1300g`
 - Manual Override: `True`
 - Config 
	- Number of scheduled times: `1`
	- Specific times(24hr): `0800`
	- Dispense Durations(Seconds): `10`

*Expected Outputs*
- Servo Controls:  "open for 10 seconds" (But didn't dispense properly) 
- Staff Alert system: Send Error "ER03"
- UI_Display: **"Error :ER03"**
- Log: 
	Date & Time: 11/08/2025, 08:00
	Device ID: 001
	Config DATA: 
		Amount of times per day to feed : 1
		Feeding Time : 0800
		Dispense Duration: 10secs
	Sensor Data:
		Hopper Weight: 1100g
		Bowl Weight: 0g
	Dispense Type: Manuel
	Error State: TRUE "ER03"
	
*Logic Discussion:*
Scheduled feeding time triggered successfully and the servo attempted to opened for defined time but internal logic never recorded the bowls weight increase during the dispensing cycle, meaning the food didn't dispense properly, triggering Error ER03 notifying staff. 
*Refinement*
It also possible for the servo to jam open allowing the hopper to constantly empty into the animals bowl. A redesign of the servo system would likely be needed to avoid these errors, it would also be possible to include a more complex monitoring tools like servo amp draw.

---

6) **Multiple Errors**
*Inputs*
 - Time: `11/08/2025, 07:00`
 - BWS: `0g`
 - FHS :`1300g`
 - Manual Override: `True`
 - Config 
	- Number of scheduled times: `1`
	- Specific times(24hr): `0800`
	- Dispense Durations(Seconds): `10`

*Expected Outputs*
- Servo Controls:  "open for 10 seconds"
- Staff Alert system: Send Error "ER04"
- UI_Display: "**Error Limit Reach**"
- Log: 
	Date & Time: 11/08/2025, 07:00
	Device ID: 001
	Config DATA: 
		Amount of times per day to feed : 1
		Feeding Time : 0800
		Dispense Duration: 10secs
	Sensor Data:
		Hopper Weight: 1100g
		Bowl Weight: 0g
	Dispense Type: Manuel
	Error State: Error "ER04"
	
*Logic Discussion*
In this case the system operated in the same way to example 3 but when internally the system has been tracking the number errors and has entered a "Error Limit mode" as there have been more then 3 errors since the last time the device was started and is waiting for staff intervention to fix the errors.

---

**Other refinements**

After a review I've noted a oversite in the assumption around monitoring how much food is being dispensed, The assumption was it would be impossible to measure the amount of food being dispensed using the bowl sensors, the oversite being that it would in fact be possible to measure the change in weight in the food hopper to measure how much food was dispensed using a simple function like.

```
hopper weight = read hopper sensor
set servo to open 

while run is true 
	if read hopper_sensor <= hopper_weight - dispense_amount
		set servo to closed
		run = false

```

Explanation

```
reads hopper sensor and store it as "hopper weight"
Open servo to start dispensing food

While run is true(loop)
	if hopper sensor weight is less then or equal to hopper_weight minus dispense_amount 
		Closes servo
		sets run to false ending the loop
```




