
- An SD card for logging, terrain data base, scripting (if desired)
- Sufficient outputs for the number of motors and servos to be used
- Sufficient number of UARTs for GPS, and telemetry radios, if desired
- Vibration isolated IMU(s) is very desirable simplifying mounting considerations.
- Be sure the autopilot includes a barometer
- I2C for external compass

#### Choice of autopilot
Below is a list of consideration when it comes to selecting a pilot board. 

1. Sensor Redundancy: ArduPilot supports redundant IMUS, GPS, etc. Many controllers have multiple IMUs integrated on board for applications requiring this level of reliability.
2. Number of Servo/Motor Outputs.
3. Number of UARTs: Telemetry radios, GPS’s, Companion Computers, etc can be attached via these ports.
4. External Buses: I2C, CAN, etc. allow many types of devices, such as airspeed sensors, LED controllers, etc. to be attached to the autopilot.
5. Number of Analog I/O: Some controllers have analog I/O available for such features as inputting receiver signal strength (RSSI) or battery voltage/current or other analog sensors.
6. Integrated Features: Such as on-board OSD (On Screen Display), integrated battery monitoring sensors.
7. Vibration Isolation: Internal mechanical vibration isolators for IMUs for high vibration applications.
8. IMU Heaters: On board temperature control of IMUs for applications in harsh environments or widely varying temperatures during a flight to provide the highest possible precision.
9. Size: Many vehicles have limited space for the autopilot.
10. Expense: Controller prices range from ~$25 to much more, depending on feature set.