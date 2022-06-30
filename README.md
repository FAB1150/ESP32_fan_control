# THIS README FILE IS A WIP
### the code works, but the instructions aren't finalized. Expect a bunch of activity in the file!

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
* Wire everything up
* set up the code and flash it to the ESP32
* if you don't know how to flash to the ESP32 from Arduino IDE, follow [this tutorial][link6] to get started
* you're done!

## Wiring
Wiring everything up is fairly easy, as you can usually do everything on one side of the ESP32. Again, make sure to [check the pinout of your board][link4], as some have the 3V3 output on the other side!

Follow this image for reference:
![Breadboard][img1]

* note that in this image the bottom rail in the breadboard has 12V, and the top rail is 5V. Don't mix them up!
* the pin next to the 5V input is NOT a ground! it doesn't say GND, but CMD. follow the diagram in the image above!
* By default, the fan PWM output (green wire) is on pin GPIO35, and the thermistor input (brown wire) is on pin GPIO25. Check where the pins are on your board, as some have them scrambled in different places. If you choose differents pins, you can edit the default ones in the next step. For the thermistor input, make sure to [choose a pin][link4] connected to the ADC (ADCx_y in the website's image)
* The pull-down resistor (connected to the green wire) isn't strictly necessary, but if you don't use it some fans might misbehave
* The 100KΩ resistor goes between 3V and the thermistor, as in this image:
![thermistor][img2]
* You need to set up the voltage regulator for 5V. To do this, cut the little trace over the ADJ pad with an xacto knife (circled in the image), and bridge the two pads for 5V. If you plan on modifying the project a bit, remember that these voltage regulators can't boost the voltage, only step it down! If you need to go up, buy a different one.
* The Enable pin of the regulator will stay discconnected, as the regulator can stay on all of the time.
![voltage regulator][img3]
* Be careful with connecting the fan wires correctly! the PWM signal wire is NOT next to the power wires, but the one on the other side of the connector! be careful or you will fry your esp32!
* If you're following these instructions in order and plan on calibrating the thermistor, no need to plug the fan in yet. Do it at the end, as it's quite clumsy

## Set up the code
We're almost done! Now you need to set up the code.
#### It is well documented, but make sure to follow this readme at least for the thermistor calibration!

* After downloading it, open pwm_fan_control.ino with the Arduino IDE
* If you have never used an ESP32 with the Arduino IDE yet, follow [this tutorial][link6] to get started. Now that you see the esp32 code correctly, we can go on editing our settings.
* define the correct pins by editing these two variables, if you used the same pins as me you don't have to modify this

  ``` c++
  //setting the I/O pins
    //the fan PWM output pin
      const int fanOutput = 32;
    //thermistor pin
      const int thrm = 25;
  ```
### Now you need to define the temperatures for the fan curve!
  * Keep this line uncommented if you want the fan to turn off when below the minimum temperature, or comment it if you want the fan to always spin
    ``` c++
    #define FAN_OFF
    ```
  * This variable sets the minimum temperature that makes the fan spin (if you commented FAN_OFF, below 
this temperature the fan just spins at the minimum speed you set later)
    ``` c++
    const int MINTEMP = 35;
    ```
  * This variable sets the maximum fan speed, it goes from 0 to 255.
    ``` c++
    const int maxDutyCycle = 190; 
    ```
  * Now you have a few different fan curves you can experiment with. I recommend just using the default one, as it's the most customizeable.
  * At this temperature the fan curve reaches maxDutyCycle, and caps off
    ``` c++
    const int MAXTEMP = 60;
    ```
  * You set up your fan curve! To aid in visualizing it, here's how the default fan curve looks like:
    ![fan curve][img4]
  * Note that if FAN_OFF is defined, when the temperature reaches MINTEMP the fan will turn off instead of staying at minDutyCycle.

### TODO calibrate the voltage readings

### Now you have to calibrate the thermistor!
This is an important step, as as I said a few times already the ESP32 ADC is not precise. The default value works, but don't expect to get a precise temperature reading from it!
A laptop here is pretty handy, as you'll have to move around a bit while getting some readings. A kitchen thermometer is pretty handy too, but not strictly necessary if you don't have one.

* Uncomment DEBUG, in line 4, as we'll need to know the readings of the thermistor
  ``` c++
  #define DEBUG
  ```
* Now flash the code to the ESP32. This also lets you know if you made any mistakes in the fan curve configuration! If you have difficulty flashing to the ESP32 and you did everything follow [this tutorial][link6] tells you to do, try holding the BOOT button on the ESP32 while it is flashing. Your finger might hate you ;)
* With the ESP32 still connected, in the IDE go to tools > Serial Monitor. Select Baud 115200 and you'll see a flow of numbers!

  ![serial monitor][img7]

* This is your friend. The columns of numbers you see are respectively the temperature reading, the voltage reading and the resistance of the thermistor, and the PWM output (from 0 to 255) to the fan.
* Open [this website][link7] and keep it open in a tab, it's the calculator for the values we'll put in the code.
* Get some water in a pot on a stove, and some water from the fridge with ice in it. We want three reference points at 100C, 0C and... 36C, your body temperature!
* Now you want to submerge the probe in the boiling water, and read the resistance value in the serial monitor. Don't put the probe too far down or you'll read the temperature of the metal and your reading won't be accurate! Wait for the resistance value to stabilize but be quick, you don't want to ruin your thermistor. Take note of the value somewhere.
* Do the same in the ice water, after you're fairly sure the temperature stabilized at 0C, note that value down too, and now stick the probe into your mouth. If you have a thermometer here use it, to get a more precise reading. put the probe in the same spot where you took your temperature with the thermometer!
* As you might have guessed, this is more of an art than a science (if you don't have calibrated temperature references, of course). After some messing around, you'll know you got it right!
* Note that the resistance is given to you in kiloOhms, while the website we're about to use asks for Ohms. Just multiply by 1000 the numbers you noted down!
* Go back to [the calculator][link7], and focus on the "Please input resistance-temperature pairs" section.
  * Under "T (°C)", put 0, 36.5, 100, or the values you got with your thermometer (T1 is the cold water, T2, is your body temperature, T3 is the boiling water)
  * Under "R (Ω)", put the readings you got from the serial monitor respectively for icy water, your body temperature, and the boiling water (R1, R2, R3).

    ![Values input][img5]
  * A rule of thumb to see if you got readings that make sense is to see if the blue and the yellow curves in the graph are close. They'll never be exactly the same, but they shouldn't be too far apart.
  * Now, go down to the "Calculated β model coefficients" section. Keep note of the two numbers you see.

    ![Beta coefficients][img6]
* We can go back to the code! Under the variables for the thermistor resistance calculation section, you need to edit R_Tk and BCoeff with the first and second value we took from the calculator respectively. (R_Tk is R(25°C)=[ ]Ω and BCoeff isβ=[ ]K). If they happen to be whole numbers, still use the decimal number! (write x.0)
  ``` c++
  //variables for the thermistor resistance calculation
    const float R_Tk = 115025.41;
    const float BCoeff = 3433.3;
  ```
* Flash the code to the ESP32, and try to measure various things looking at the first value on the left. That's your temperature!
* If you're getting non-sensical readings, you'll have to do everything again (if you didn't use a thermometer try  to use one, and experiment with the water temperatures. Try water from the fridge instead of icy water, and water that's not quite boiling yet, for example.
* If everything looks good, you're done! Go back to line 4, comment DEBUG back and flash the code to the ESP once again. The hard part is over!

\#todo
Recomment #define DEBUG in the end.

``` c++
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
[link7]: https://www.thinksrs.com/downloads/programs/Therm%20Calc/NTCCalibrator/NTCcalculator.htm

[img1]: https://github.com/FAB1150/ESP32_fan_control/blob/main/images/breadboard%20schematic.jpg?raw=true
[img2]: https://github.com/FAB1150/ESP32_fan_control/blob/main/images/thermistor%20wiring.jpg?raw=true
[img3]: https://github.com/FAB1150/ESP32_fan_control/blob/main/images/voltage%20regulator.jpg?raw=true
[img4]: https://github.com/FAB1150/ESP32_fan_control/blob/main/images/FanCurve.jpeg?raw=true
[img5]: https://github.com/FAB1150/ESP32_fan_control/blob/main/images/values%20input.jpg?raw=true
[img6]: https://github.com/FAB1150/ESP32_fan_control/blob/main/images/Beta%20coefficient.jpg?raw=true
[img7]: https://github.com/FAB1150/ESP32_fan_control/blob/main/images/serial%20monitor.jpg?raw=true
