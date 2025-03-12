## DIY PWM Fan for CPU Temp

According to this [info](https://www.instructables.com/PWM-Regulated-Fan-Based-on-CPU-Temperature-for-Ras/), a small fan can be controlled with PWM to run when the temperature reaches a certain level, as for the Pi5. The parts needed are:

* 5V Fan
* NPN transistor that supports at least 300mA, like a 2N2222A
* 1K resistor
* 1 diode

With the following circuit diagram:

![diagram](/fan/pwm-fan-temp.jpg)