## #3 Rviz Simulator

Working with Rviz to control a simulation of a dynamism-based robot as opposed to a physical robot.

Reasons?
1) As a try before you buy, just to see how the robots will work in a virtual environment
2) You can test out your code on a simulated robot before loading it up onto a physical robot just to make sure that nothing strange is going to happen.

Terminal commands:
``` 
roslaunch interbotix_xsarm_control xsarm_control.launch robot_model:=wx250s use_rviz:=true use_sim:=true
```
note how the `use_sim:=true` is the key one here.

To run an example use the following commands:
```
roscd interbotix_xsarm_control/
cd ..
cd examples/python_demos/
python bartender.py
```
in this case the arm joints are working in position control mode using a time-based profile as opposed to a velocity-based profile. Usually, you will desire to work with time based profile when you are in position mode. 
But you can even control joints in pwm or current control mode (only recommended for wrippers).

Now we are going to use the **joystick ros package**. You will need a PS4 controller connected over bluetooth to your laptop.

In terminal type:
```
roslaunch interbotix_xsarm_joy xsarm_joy.launch robot_model:=wx250s use_sim:=true
```
This is the expected behavior:\
![joystick](./joystick.gif)

To connect your joystick on ubuntu I recommend: setting bluetooth to *on*, and then press and hold the *Share* button and also the *PS* button. After a few seconds, you will see a "Wirleless Controller" pop up. 

Use **START/OPTIONS** tp move robot arm to its *Home pose* and the 
**SELECT/SHARE** button to	move robot arm to its *Sleep pose*.

More info [here](https://www.trossenrobotics.com/docs/interbotix_xslocobots/ros_packages/joystick_control.html).

---
Something you might be wondering about is why even have this tool why not just use *Gazebo* (becuase Gazebo is meant to be used as a simulation environment)... and yeah you are right you can, but there are a few advantages to using *Rviz* instead of Gazebo:
1) Gazebo is extremely processor intensive (for example, in a *Raspberry Pi* computer you probably don't want to be running that).
2) Gazebo being such a complex simulation tool there are a lot of different factors that go into the simulation as a result some weird things can happen like gravity isn't simulated correctly or joints might just oscillate for no reason whereas with Rviz all you are doing is publishing joint states so you can't really go wrong with that.
3) When you command joints in Rviz your input is exactly what you see at the output. You will not see any kind of sag due to gravity or friction. In that way, it is a great way for you to figure out if the math that you are doing in correct from a theoretical standpoint. 