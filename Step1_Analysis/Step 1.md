
### Problem Statement 

A shelter needs an affordable, programmable pet feeder that automatically dispenses food for animals at scheduled times, tracks food consumption, and alerts staff of any error or issues.

### Inputs and outputs 

| **INPUT/OUTPUT** | **Source**                   |
| ---------------- | ---------------------------- |
| Input            | Real time clock              |
| Input            | Bowl Weight Sensor           |
| Input            | Food Hopper Sensor           |
| Input            | Manual Override button       |
| Input & Output   | An app                       |
| Output           | Dispensing Servo             |
| Output           | Staff Alert System           |
| Output           | User Interface (LCD Display) |
| Output           | Digital Log                  |

### Assumptions & Limitations

##### Assumptions
- The current logic assumes perfect world conditions, all hardware components operate without delays or faults, and all inputs/outputs are processed instantly and accurately.
- Limited to 1 animal per feeder.  Though it can easily be reprogramed if 1 feeder is being used for different animal. 
- The animal eats all food within 10 mins, value could be including in the user config file but in current iteration is hard coded. 
- The feeder does not monitor the amount of food being dispensed in weight. It is assumed that the animal may start eating before the dispensing cycle is finished making it impossible to get an accurate measurement. Instead they feeder uses a timer to control how much food is being dispensed. 
- The current iteration uses an app or external software for initial setup and as part of the notification/alert system, the system could be adapted to be configured locally to save costs though it would be harder to scale and would required staff to actively monitor each feeder for errors, where an app allows remote monitoring and controls
- Assumed that the staff will undertake basic training on programing the device include how to properly calibrate the timer based on how much food they want dispensed. 

##### Limitations 
- hardware limitations would need to be accounted for (accuracy of weight sensors and capacity of servo), for this project they have been disregarded assuming perfect world conditions. 






