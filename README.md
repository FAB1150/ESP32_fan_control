# Temperature fan control with an ESP32

A simple way to output a PWM signal based on temperature readings by a thermistor

## What you need:
* An [ESP32][link1]
* A PWM fan (or anything that accepts a PWM signal that you want to control)
* A [voltage regulator][link2] for the ESP32 (if you have a 12V fan)
* An [NTC thermistor][link3] (take one from a 3D printer hotend), preferrably a 100K 3950
* A 100KΩ resistor
* A pull-down resistor (1KΩ is fine)
* A 12V voltage source (or 5V for 5V fans)
* Some free time

#### notes:
* [Check the pinout of your ESP32][link4] to avoid headaches! Not all boards are the same!
* If you want to put everything on a breadboard, the ESP32s without silkscreen legends are thinner
* If you use 12V fans, the voltage regulators linked above are configurable. Get a thin blade and some solder
* If you use small 5V fans you don't need to step down the voltage and can just use a 5V supply for everything
* the thermistor resistor isn't necessarily 100KΩ, use the [voltage divider equation][link5] to find an appropriate resistor (we'll set this up in the code). Usually a resistor with the same value as the thermistor is good

## Getting Started
* download pwm_fan_control.ino from its folder and open it with the arduino IDE
* if you don't know how to flash to the ESP32 from Arduino IDE, follow [this][link6] tutorial to get started!
* Wire everything up
* set up the code and flash it to the ESP32
* you're done!

## Wiring
Wiring everything up is fairly easy, as you can usually do everything on one side of the ESP32. Again, make sure to [check the pinout of your board][link4], as some have the 3V3 output on the other side!

Follow this image for reference:
![Breadboard][img1]

* note that in this image the bottom rail in the breadboard has 12V, and the top rail is 5V. Don't mix them up!
* The PWM pin in the fan is NOT next to the power wires! Put it in the correct position or you'll fry the ESP32!
* By default, the fan PWM output (green wire) is on pin GPIO35, and the thermistor input (brown wire) is on pin GPIO25.
* The pull-down resistor (connected to the green wire) isn't strictly necessary, but if you don't use it some fans might misbehave
* The 100KΩ resistor goes between 3V and the thermistor, as in this image:
![thermistor][img2]
* You need to set up the voltage regulator for 5V. To do this, cut the little trace over the ADJ pad with an xacto knife (circled in the image), and bridge the two pads for 5V. If you plan on modifying the project a bit, remember that these voltage regulators can't boost the voltage, only step it down! If you need to go up, buy a different one :)
![voltage regulator][img3]

## Setup the code
\#todo
define the correct pins
calibrate thermistor (website reference). Decomment #define DEBUG!!
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

[link1]: https://www.amazon.com/HiLetgo-ESP-WROOM-32-Development-Microcontroller-Integrated/dp/B0718T232Z/ref=sr_1_4?crid=1WTN5IH997FW2&keywords=esp32&qid=1655224029&sprefix=esp32%2Caps%2C151&sr=8-4
[link2]: https://www.amazon.com/Weewooday-Regulator-Voltage-Converter-Transformer/dp/B08JZ5FVLC/ref=sr_1_3?crid=3E4K4QAQ63O3Y&keywords=mini+voltage+regulator&qid=1655223958&sprefix=mini+voltage+regulator%2Caps%2C152&sr=8-3
[link3]: https://www.amazon.com/HICTOP-Thermistors-Sensor-Reprap-printer/dp/B0150YLX9C/ref=sr_1_1_sspa?crid=3AMQB4ZI9Y2LA&keywords=ntc%2B3950%2B100k%2Bthermistor&qid=1655224094&sprefix=NTC%2B%2Caps%2C148&sr=8-1-spons&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUExRDk1SURYOVhDVVJZJmVuY3J5cHRlZElkPUEwNTk4NTk5MjVGQ0NJSkJORjNQSiZlbmNyeXB0ZWRBZElkPUEwNDUwMzU1V0c2STZUTUhFUjROJndpZGdldE5hbWU9c3BfYXRmJmFjdGlvbj1jbGlja1JlZGlyZWN0JmRvTm90TG9nQ2xpY2s9dHJ1ZQ&th=1
[link4]: https://www.upesy.com/blogs/tutorials/esp32-pinout-reference-gpio-pins-ultimate-guide
[link5]: https://ohmslawcalculator.com/voltage-divider-calculator
[link6]: https://randomnerdtutorials.com/installing-the-esp32-board-in-arduino-ide-windows-instructions/

[img1]: https://github.com/FAB1150/ESP32_fan_control/blob/main/images/breadboard%20schematic.jpg?raw=true
[img2]: https://github.com/FAB1150/ESP32_fan_control/blob/main/images/thermistor%20wiring.jpg?raw=true
[img3]: https://github.com/FAB1150/ESP32_fan_control/blob/main/images/voltage%20regulator.jpg?raw=true
