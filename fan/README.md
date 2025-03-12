## DIY PWM Fan for CPU Temp

According to this [info](https://www.instructables.com/PWM-Regulated-Fan-Based-on-CPU-Temperature-for-Ras/), a small fan can be controlled with PWM to run when the temperature reaches a certain level, as for the Pi5. The parts needed are:

* 5V Fan
* NPN transistor that supports at least 300mA, like a 2N2222A
* 1K resistor
* 1 diode

With the following circuit diagram:

![diagram](/fan/pwm-fan-temp.jpg)

## Controlling fan speed according to temperature

To control fan speed, we use a software PWM signal from the RPi.GPIO library. A PWM Signal is well adapted to drive electric motors, as their reaction time is very high compared to the PWM frequency.

Use the calib_fan.py program to find the FAN_MIN value by running in the terminal:

python calib_fan.py

Check several values between 0 and 100% (should be around 20%) and see what is the minimum value for your fan to turn on.

You can change the correspondence between temperature and fan speed at the beginning of the code. There must be as many tempSteps as speedSteps values. This is the method that is generally used in PC motherboards, moving points on a Temp / Speed 2-axis graph.

## Run the program at startup

To run the program automatically at startup, I made a bash script where I put all the programs I want to launch, and then I launch this bash script at startup with `rc.locale`

Create a directory /home/cartheur/Scripts/ and place the `fan_ctrl.py` file inside that directory. In the same directory, copy the file launcher.sh. Edit `/etc/rc.locale` file and add a new line before the `"exit 0": sudo sh '/home/pi/Scripts/launcher.sh'`.

If you want to use it with OSMC for example, you need to start it as a service with `systemd`.

* Download the cooling.service file.
* Check the path to your python file.
* Place cooling.service in `/lib/systemd/system`.
* Enable the service with `sudo systemctl enable cooling.service`.

This method is safer, as the program will be automatically restarted if killed by the user or the system.