02-05-2025 15-33
Status: #idea
Tags: [[3D Printing]]

# 3D printer Calibration

The calibration of the Voron 2.4 can be quite complex so this can be a great reference point to ensure that it can be done consistently when needed. 

### PID Tune Heated Bed
Move the nozzle to the centre of the bed and approximately 5-10mm about the bed surface, the run:

`PID_CALIBRATE HEATER=heater_bed TARGET=100`

This will last around 10 minutes and once finished save the parameters with `SAVE_CONFIG`

### PID Tune Hotend
Set the part cooling fan to 25% (`M016 S64`) and the run:

`PID_CALIBRATE HEATER=extruder TARGET=245`

This will last around 5 minutes and then you can run `SAVE_CONFIG`


---
# References