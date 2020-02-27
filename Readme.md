# Seaport Tide and Sea Level Rise Monitor
## The Basics
### Getting Started
This project comprises a sensor package designed to make distance measurements and display output. It utilizes an Arduino-based Adafruit Bluefruit Feather nRF52832 or similar microcontroller with BLE, a Maxbotix LV-EZ4 ultrasonic rangefinder, an Adafruit FeatherWing OLED screen, a rechargeable lithium ion battery, and a custom laser cut enclosure.

### Very Important Notes
1. The monitor is not waterproof. Not even a little. Do not dunk it, rinse it, let it get rained on, etc. or it will likely be permanently ruined. Waterproofing the instrument is very feasible but not in the scope of this project.
2. The monitor is not designed to be used for anything other than educational purposes. The sensor should be a great representation of more accurate and robust methodologies but limitations in sensor resolution, accuracy, and reliability make this embodiment a poor choice for other applications without significant modification.

## The Fun Stuff
### Deployment
Deployment is actually the last step but it's probably the most exciting, so we'll start our procedural notes here. The Seaport Tide and Sea Level Rise Monitor has a ~ 6.5 m or 21' range with ~ 1" resolution. Deployment location must be chosen with these values in mind. If you are deploying the monitor at a location reasonably close to an existing NOAA tide station, start by examining the known tidal range at your location: [NOAA tide predictions](https://tidesandcurrents.noaa.gov/tide_predictions.html). You will want to deploy the monitor at location where it can aim straight down to the water's surface and at a height that is slightly higher than the highest high tide that the location is expected to experience.

### Assembly
If your monitor arrived unassembled or you were feeling adventurous enough to disassemble it, you may want to follow these directions to put it (back) together.

**Step 1:** Stack the OLED FeatherWing on top of the Feather microcontroller.

If either or both of those units arrived without their stacking headers, you will first need to solder the headers in place. [Here](https://www.makerspaces.com/how-to-solder/) is a nice soldering description and tutorial. The OLED screen is an Adafruit FeatherWing, a design specifically meant to be stacked on top of an Adafruit Feather microcontroller, such as the Bluefruit nRF52832 which we are using here (note that there are many microcontrollers that use the Feather form factor and they are almost interchangeable; only a few minor changes need to be made in the setup of the Arduino software and possibly firmware). Adafruit also makes FeatherWing "doublers" and "triplers" which are designed to do the same thing (electronically speaking) as stacking Feathers/FeatherWings on top of each other but with a side-by-side layout.

**Step 2:** Choose the sensor communication protocol that you will use to get the rangefinder's data from the rangefinder to the microcontroller. Options include UART, PWM, and analog, according to the datasheet. If you want to read more about UART, PWM, analog inputs, and other common microcontroller communication techniques, Arduino has a great primer on analog and digital pin functionality: https://www.arduino.cc/en/Tutorial/Foundations.

**Step 3:**

### Calibration
It is never wise to trust that any sensor will work perfectly out of the box* and a good scientist or engineer (professional or otherwise) would be well served by checking the sensor's accuracy (and improving it, if possible). It is sometimes the case that a sensor manufacturer has very sophisticated calibration equipment and, especially if they provide the calibration data for your instrument, their technique may be hard to replicate for yourself. But, alternatively, sometimes manufacturers do not calibrate every individual instrument and/or their techniques and equipment aren't sufficient for your needs, so you must do it yourself.

The simplest form of (re)calibration or data correction is a simple "offset" adjustment. A good example of this would be if you used this rangefinder to make a measurement where the sensor reported 0" but you knew that the true distance was actually 4". In this case, the offset you should apply would be 4". In other words, you would add 4" to every reading that the sensor provided. (This is only an example; this sensor is not meant for measuring distances ≤ 6" so all measurements from 0&endash;6" are considered to be inaccurate, regardless of calibration!!!)

In some cases, there can also be a "gain" correction. A gain correction is the same thing as applying a slope to a line and an offset correction is the same as adding an intercept to a line. Following the same example as above where we already know that the offset is 4", now imagine that when the sensor reads 100", the correct actual distance is 124 inches. The offset of 4 inches doesn't get us close to the difference between 100" and 124". For this simple example (see below for directions for a more comprehensive approach), we can calculate a gain of 1.2. First, subtract the offset from the correct answer (124"&endash;4"=120") and then divide by the reported value (120"/100"=1.2). In reverse, we would say that *actual = measured*gain+offset* (124" = 100"\*1.2+4").

In order to calculate the gain and offset correction factors for a linear correction such as the one we described here, we would want to collect as many sensor measurements as possible over the sensor's full range (in this case, 6&endash;254 inches) as well as make measurements of the "true" value at each of the sensor measurement points. We would then make a plot where sensor value would go on the x-axis and the "true" value would go on the y-axis. We would then use linear regression to calculate the slope (gain) and intercept (offset) of the line and we would apply those values to all future sensor measurements. Note that we put "true" inside quotes because it is extraordinarily difficult to know exactly what the true value is. How will you go about making estimates of the true values against which to compare your sensor measurements? A tape measure? A ruler? A string with distance markings on it? Another electronic distance sensor? How do you know that those measurements are accurate? Who calibrated those instruments and how did they do it???

It is also important to recognize that this simple linear calibration technique does not work for all sensors. Sometimes a more appropriate correction equation could use a quadratic equation or an exponential one or perhaps something with an even more bizarre form. But we'll assume that we can use a linear recalibration or correction for this project because it seems to make meaningful improvements in sensor performance without adding too much complexity to the procedure.

\*NB: in reference to the comment at the beginning of this section that "it is never wise to trust that any sensor will work perfectly out of the box," we wish to add that while this is true for research products and that the same principle could be extended to suggest healthy skepticism of anything you may hear or read (always check sources and facts!), most sensors work as well as they need to work for any given application. For instance, the thermometer used at your doctor's office may not be perfectly accurate&emdash;perhaps it is only accurate to 0.1 ºC&emdash;but it is likely good enough for its specified use.
