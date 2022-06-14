# Temperature fan control with an ESP32

A simple way to output a PWM signal based on temperature readings by a thermistor

## What you need:
* An [ESP32](https://www.amazon.com/HiLetgo-ESP-WROOM-32-Development-Microcontroller-Integrated/dp/B0718T232Z/ref=sr_1_4?crid=1WTN5IH997FW2&keywords=esp32&qid=1655224029&sprefix=esp32%2Caps%2C151&sr=8-4)
* A PWM fan (or anything that accepts a PWM signal that you want to control)
* A [voltage regulator](https://www.amazon.com/Weewooday-Regulator-Voltage-Converter-Transformer/dp/B08JZ5FVLC/ref=sr_1_3?crid=3E4K4QAQ63O3Y&keywords=mini+voltage+regulator&qid=1655223958&sprefix=mini+voltage+regulator%2Caps%2C152&sr=8-3) for the ESP32 (if you have a 12V fan)
* An [NTC thermistor](https://www.amazon.com/HICTOP-Thermistors-Sensor-Reprap-printer/dp/B0150YLX9C/ref=sr_1_1_sspa?crid=3AMQB4ZI9Y2LA&keywords=ntc%2B3950%2B100k%2Bthermistor&qid=1655224094&sprefix=NTC%2B%2Caps%2C148&sr=8-1-spons&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUExRDk1SURYOVhDVVJZJmVuY3J5cHRlZElkPUEwNTk4NTk5MjVGQ0NJSkJORjNQSiZlbmNyeXB0ZWRBZElkPUEwNDUwMzU1V0c2STZUTUhFUjROJndpZGdldE5hbWU9c3BfYXRmJmFjdGlvbj1jbGlja1JlZGlyZWN0JmRvTm90TG9nQ2xpY2s9dHJ1ZQ&th=1) (take one from a 3D printer hotend), preferrably a 100K 3950
* A 10KΩ resistor
* A pull-down resistor (1KΩ is fine)
* A 12V voltage source (or 5V for 5V fans)
* Some free time

#### notes:
* [Check the pinout of your ESP32](https://www.upesy.com/blogs/tutorials/esp32-pinout-reference-gpio-pins-ultimate-guide) to avoid headaches! Not all boards are the same!
* If you want to put everything on a breadboard, the ESP32s without silkscreen legends are thinner
* If you use 12V fans, the voltage regulators linked above are configurable. Get a thin blade and some solder
* If you use small 5V fans you don't need to step down the voltage and can just use a 5V supply for everything

## Getting Started
Download 

## Wiring
Wiring everything up is fairly easy, as you can usually do everything on one side of the ESP32. Again, make sure to [check the pinout of your board](https://www.upesy.com/blogs/tutorials/esp32-pinout-reference-gpio-pins-ultimate-guide), as some have the 3V3 output on the other side!

Follow this image for reference:
![Breadboard](https://github.com/FAB1150/ESP32_fan_control/blob/main/images/breadboard%20schematic.jpg?raw=true)

* By default, the fan PWM output (green wire) is on pin GPIO35, and the thermistor input (brown wire) is on pin GPIO25.
* The pull-down resistor (connected to the green wire) isn't strictly necessary, but if you don't use it some fans might misbehave
* The 100KΩ resistor goes between 3V and 

## Setup the code
\#todo
define the correct pins
calibrate thermistor (website reference)
set up the temperatures, max and min PWM, curve shape

``` arduino
#placeholder
```

## Usage
``` arduino
#placeholder
```


## License
[GNU GPLv3.0](https://choosealicense.com/licenses/gpl-3.0/)