### BME280 atmospheric sensor breakout

[Specifications](#specifications)\
[Interfacing with your microcontroller](#interfacing-with-your-microcontroller)\
[Power consumption](#power-consumption)\
[How to optimize for battery operation](#how-to-optimize-for-battery-operation)

At some stage or another all electronics enthusiasts build something with a temperature sensor, because it is cool (and sometimes hot) and we want to know exactly how cool or hot it is and then display it on a screen, or send it to the cloud. What no one tells us is that not all sensors are made equal. Some are inaccurate, others make it difficult by giving you a 3V-only or 5V-only option having you running around trying to find logic shifters and voltage regulators to get it working with your specific development board.

This version of the BME280 atmospheric sensor breakout solves all that. Not only does it contain one of the most precise environmental sensors around, designed by Bosch Sensortech, but it also solves the problem of not having to worry about logic levels and incompatible voltages. Whether you have an Arduino or an ESP32 (or any other dev board that normally operates between 3V and 5V) with which you want to build your next weather station, this will do it for you in a "Plug & Play" fashion. 

Simply power the board, hook up the two data lines (I2C), install your favorite BME280 library, and get going.

<img width="533" alt="image" src="https://github.com/BillyGriffiths/BME280/assets/6160978/39d51cf5-d1b0-41b2-b100-9d70bb42ced5">

## Specifications

- Input voltage: 2.5V ~ 5V 
- Communication protocol: I2C
- I2C Addressing: 0x77 (default), 0x76 by soldering the jumper
- I2C pull-ups: Onboard (10k ohm)
- Logic level shifting: Automatic
- Sensor: Bosch Sensortec BME280
- Function: Temperature, pressure, and humidity sensor
- Dimensions: 12 x 18mm
- Breadboard compatible: Yes

## Interfacing with your microcontroller

1. Connect **+** (Vin) to the power supply. Use the same voltage as the logic of your microcontroller.
2. Connect **-** (GND) to the common power/data ground.
3. Connect **SDA** to the I2C data pin (SDA) of your microcontroller. 
4. Connect **SCL** to the I2C clock pin (SCL) of your microcontroller.

The default I2C address is `0x77`. To make use of I2C address `0x76` create a solder bridge between the two pads marked "76" in the top left corner of the board (See "Power consumption" below for considerations when doing so).

<img width="557" alt="image" src="https://github.com/BillyGriffiths/BME280/assets/6160978/0900b0a7-4c47-4d41-b020-66e63b995fd4">

## Power consumption

At 2.62uC (Microcoulomb), the module is most power efficient in its default configuration, making use of the BME280's forced mode and powered using 3.3V. To make address selection more convenient an additional 10K resistor acts as a pull-up for the address-select feature. This, unfortunately, comes at a cost when it is configured to make use of I2C address `0x76`. (See "How to optimize for battery operation" below to reduce consumption).

**Sleep mode:** When the BME280 is in power save mode\
**Normal mode:** When the BME280 remains in an active state\
**Forced mode:** The BME280 remains in sleep mode until a sensor reading is requested. After taking the reading it automatically returns to sleep.

Zoom your browser window to view the results more clearly.
  
![power_consumption_0x77](https://github.com/BillyGriffiths/BME280/assets/6160978/bcac3173-1f85-4ca3-a239-3da7cef9d36e)

![power_consumption_0x76](https://github.com/BillyGriffiths/BME280/assets/6160978/ecd1ad5c-9034-46e3-b3b8-c72dac6b31b6)
 
## How to optimize for battery operation

### Option A: Turn the module on/off using a GPIO

The best way to save power is to only power the module when a reading is required. Due to the low power consumption of the module, power can be sourced safely from a microcontroller GPIO. Most microcontroller GPIO's support 20-30mA (Check the microcontroller datasheet for your specific model) allowing you to control power to the module via code.

1. Connect + (Vin) of the module to a GPIO of your choice
2. Before taking a sensor reading, set the GPIO state `HIGH` to power the module
3. Allow a couple of milliseconds delay (e.g 20ms) for the sensor to initialize
4. Read the sensor values
5. Turn the module off by changing the GPIO state to `LOW`.

❗If you will be placing your microcontroller in deep sleep, make sure the GPIO you select defaults to a `LOW` state during sleep. Some GPIOs default to `HIGH` when entering deep sleep even if you set it `LOW` in code, which will cause the module to be powered during sleep and you will not be saving any power. 

### Option B: Modify the PCB

⚠️ **Warning:** Modifying the PCB carries risk. Any soldering should be performed quickly and accurately. Do not allow your soldering iron to come near or touch the BME280 sensor as this might affect its calibration and cause inaccurate sensor readings.

- *When using I2C address 0x77:* No power saving is achieved by modifying the module.

- *When using I2C address 0x76:* Using a soldering iron, carefully remove resistor R1. This eliminates the pull-up current draw of resistor R1 and will reduce consumption by ~324uA (3.3V) and ~333uA (5.0V) respectively.

<img width="388" alt="image" src="https://github.com/BillyGriffiths/BME280/assets/6160978/e2da045e-5136-4779-b4e7-3a46e5137b0d">





