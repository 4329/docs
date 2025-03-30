## How to solve robot software problems

While there are some do's and don'ts that could be added to a list, a much more useful approach is to know how to diagnose and solve problems ( you know - the whole give a man a fish vs. teach a man to fish kinda thing).
There are two main principles in solving problems:

1. Let data and facts guide you.
2. Scruntize recent changes - even if they seem unrelated or innocuous.

### Let data and facts guide you
##### Use the following:
- AdvantageKit logs
- Driver station logs
- match footage
- driver feedback
- eyewitness feedback 
- visual inspection (loose wires, sensor misalignment, etc.)

### Scruntize recent changes
##### The best baselines you have are previous matches.  You should have logs and match footage available to you.   If you observe bad behavior you haven't seen before and things have changed since then, its very likely the recent changes are the cause of the problem.


### Example - Rev Color Sensor - Miami Valley Regional 2025
- During our last practice match, we noticed very strange behavior that we didn't have at the previous STL regional.   During
  some commands such as raising/lowering the elevator or even driving, we'd see stuttering behavior where the robot engaged in stop/start or jerky motions for a period of time, 
  then it would "snap out of it" and start driving/behaving normally.  

- When we got back to the pits we were looking for clues and found in the driver station logs that several command loop overruns were reported during the time of jerkiness.  In fact these were large overruns - almost 80ms (command loops should take < 20 ms).  The largest culprit at the time seemed to be the Logging subsystem which was extremely puzzling because we were not attempting much of anything else logging wise than what we did at STL.
  
- That night, we didn't have much to go on, but we started looking at the recent changes that had been done.  
  One of the last changes we had made in preparation for this regional was to add a rev color I2C sensor to our coral boot
  so we could detect when the human player successfully loaded the robot up with coral during autonomous.  We had added a 
  line of code in that sensor's subsystem to log the prox value of the sensor periodically.  We pulled up an AdvantageKit log from the practice match and noticed that for some periods of time, that sensor reported a proximity value of 0.   Very strange.  0 is never an accurate reading cause that sensor is attached to the boot and there's all kinds of stuff around it.
 
- We then pulled up match footage and saw that the timestamps of the 0 values coincided with the undesireable robot behavior.  We knew that we found the issue.   Now, how do we solve it?
- We then did some searching and found this link: https://docs.wpilib.org/en/stable/docs/yearly-overview/known-issues.html which reported that Onboard I2C can cause system lockups.  While that didn't describe our issue perfectly, it did point out that:
  - Onboard I2C is buggy
  - If we are having problems with an onboard I2C device, we should probably stop using it.
- For posterity... here's what that link said:
  > Onboard I2C Causing System Lockups
   
  > Issue: Use of the onboard I2C port on the roboRIO 1 or 2, in any language, can result in system lockups. The frequency of these lockups appears to be dependent on the specific hardware (i.e. different roboRIOs will behave differently) as well as how the bus is being used.

  > Workaround: The only surefire mitigation is to use the MXP I2C port or another device to read the I2C data. Accessing the device less frequently and/or using a different roboRIO may significantly reduce the likelihood/frequency of lockups, it will be up to each team to assess their tolerance of the risk of lockup. This lockup can not be definitively identified on the field and a field fault will not be called for a match where this behavior is believed to occur. This lockup is a CPU/kernel hang, the roboRIO will completely stop responding and will not be accessible via the DS, webpage or SSH. If you can access your roboRIO via any of these methods, you are experiencing a different issue.

  > Several alternatives exist for accessing the REV color sensor without using the roboRIO I2C port. A similar approach could be used for other I2C sensors.
    Use a Raspberry Pi Pico. Supports up to 2 REV color sensors, sends data to the roboRIO via serial. The Pi Pico is low cost (less than $10) and readily available.
    Use a Raspberry Pi. Supports 1-4 color sensors, sends data to the roboRIO via NetworkTables. Primarily useful for teams already using a Raspberry Pi as a coprocessor.

#### SOLUTION:
  - The next day, we verified that we could get sensor readings by connecting it to the I2C headers on the NavX2 instead of our device.
  - We changed our code to only log the sensor values when we were actually using them (i.e. during autonomous)
  - Our robot ran into other troubles throughout the regional, but we never saw this stuttering problem again.