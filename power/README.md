## Using a high-capacity external battery

The key of this is that the battery is _not_ in the neighborhood, so the wires are connected according to the figure:

![image](/power/external-battery-connect.jpg)

## Indicating charge level

In the most simple case, according to a method for [arduino](https://www.instructables.com/DIY-Battery-Level-Checker/), this is transformed for pi. The idea is to have a simple battery checker that will have an LED light up indicator to tell the user how much power a battery contains to indicate whether or not to apply an external source to the power input on the robot. A future version would have a single LED that has the following behaviour:

* LED lights when battery level is > 50%
* LED blinks once every five seconds when level is > 25%
* LED blinks one every second when level is > 10%

the level-LED behaviour is modeled after the colour scheme of the [PiJuice](https://github.com/PiSupply/PiJuice/blob/master/Hardware/README.md).

_Things needed_

* Arduino or RaspberryPi
* Zener Diode
* 5mm Red LED
* 5mm Green LED
* 5mm Yellow LED
* 3x 100Ω Resistor
* 1x 2.2kΩ Resistor
* 3.7V LiPo for calibration

![level](/power/level-connectivity.jpg)

The Arduino's pins, by definition, reads the voltage levels of the signal. However, feeding raw input from the battery can damage the Arduino. This is where the 2.2k Ω resistor comes in. It reduces the current from the battery to a readable value for the Arduino without damaging it, since the pins have a current input limit.

The Zener diode allows us to test batteries that have a voltage greater than 8 volts. A Zener diode, like all other diode ensures that current will flow in only one direction until it hits a voltage threshold, which is dependent on the diode type. For a Zener diode, the limit is around 5.1V. When this limit is reached, current can go in the reverse direction. This is important since it is used to protect the Arduino from over-voltage.

![image](/power/charge-monitor-led.jpg)

### LED Battery Indicator

The LED battery indicator will be in order of red, yellow, then green, where red means low, yellow is medium, and green is high or full.

* Connect the cathodes of each of the LEDs to the ground rail on the breadboard.
* Connect the anodes of each of the LEDs to it's own 100Ω resistor. Then connect the other end of each of the resistors to a jumper wire to pins 2,3 and 4, respectively.
* Green LED: pin 2
* Yellow LED: pin 3
* Red LED: pin 4
* Connect the ground rail on the breadboard to the GND pin on the Arduino

### Battery Checker

* Connect the 2.2kΩ resistor to A0.
* Connect the other end of this 2.2kΩ resistor to the negative end of the zener diode, indicated by the black bar.
* Connect the end of the free end of the diode to an orange jumper wire. This will connect to the positive terminal of any battery you want to test
* Connect a white jumper wire on the ground rail on the breadboard. This will connect to the negative terminal of any battery you want to test.
* The zener diode is veryimportant as it is used to protect the Arduino if the battery has a lot of charge left. Note the polarity of the zener diode, since it will be used to only allow current to flow in one direction!

### Code

```
int greenLed = 2;
int yellowLed = 3;
int redLed = 4;
int analogValue = 0;
float voltage = 0;
int ledDelay = 1000;
void setup()
{
  pinMode(greenLed, OUTPUT);
  pinMode(yellowLed,OUTPUT);
  pinMode(redLed,OUTPUT);
}
void loop()
{
  analogValue = analogRead(A0);
  voltage = 0.0048*analogValue;
  
  if( voltage >= 1.6 )
    digitalWrite(greenLed, HIGH);
  else if (voltage > 1.2 && voltage < 1.6)
    digitalWrite(yellowLed, HIGH);
  else if( voltage <= 1.2)
    digitalWrite(redLed, HIGH);  
 
  delay(ledDelay);
  digitalWrite(redLed, LOW);
  digitalWrite(yellowLed, LOW); 
  digitalWrite(greenLed, LOW);
}
```