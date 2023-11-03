# Rob-Arm-Tutorials: Review
Here are some brief notes that serve as a summary and shortcut for putting the Interbotix X-series robotic arms into use. In our case we will use the [WidowX 250 (6DOF) model](https://www.trossenrobotics.com/widowx-250-robot-arm-6dof.aspx#documentation).

The tutorials come from [this Youtube channel](https://www.youtube.com/watch?v=o9EXEgzbAxQ&list=PL8X3t2QTE54sMTCF59t0pTFXgAmdf0Y9t&ab_channel=TrossenRobotics) (many thanks to Interbotix and all credit for them)

## #1 Getting Started With The X-Series Arm

**Aim:** Run interbotix_xsarm_descriptions \
**Objectives:**
- Useful to figure out joint limits and which way is positive rotation
- Counter Clock Wise is positive. Thumb points along positive axis of rotation
- Where all the Transport System (TF) joints are located - point out base_link and ee_gripper_link frame

First run:
```
roslaunch interbotix_xsarm_descriptions xsarm_description.launch robot_model:=wx250s use_joint_pub_gui:=true
```
you can include `show_ar_tag:=true` in the launch command to include a square black piece used in the perception ross package. 

Then for **controlling the robot arm** directly use:
```
roslaunch interbotix_xsarm_control xsarm_control.launch robot_model:=wx250s
```
Use `rostopic list` to see published and subscribers in another terminal. You will see among others this topic:

> /wx250s/joint_states

and using
```
rostopic echo /wx250s/joint_states
```
you will be able to see for this topic all the message information, something like this:
> name: [waist, shoulder, elbow, wrist_angle, wrist_rotate, gripper, left_finger, right_finger]\
> position: [-0.184, -1.905, 1.657, 0.621, 0.0, -0.811, 0.012, -0.012]\
> velocity: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]\
> effort: [0.0, -10.76, 0.0, 2.69, 0.0, 0.0, 0.0, 0.0]

Now, we can move the robot to `home pose` using the next `rostopic`:
```
rostopic pub -1 /wx250s/commands/joints_group interbotix_xs_sdk/JointGroupCommand "name: 'arm' cmd: [0,0,0,0,0]"
```
> *¡make sure your robot arm has enough space to move!* 

You may notice there is a bit of a *sag* in the elbow. It is due to gravity, we can fix that by increasing the position $p$ gain from 800 to, for example, 1500. We can do it typing:
```
rosservice call /wx250s/set_motor_register "cmd_type: 'single' name: 'elbow' reg: 'Position_P_Gain' value : 1500" values: []
```
And now again we can move it to sleep position. So for this we need to know the correc values for our robot arm. We should look for them here: `interbotix_ros_manipulators/interbotix_ros_xsarms/interbotix_xsarm_control_config/wx250s.yaml` there we can read `sleep_position: [0, -1.88, 1.5, 0.8, 0, 0]`. Finally with a `rostopic` we can send that position within a message to the arm:
```
rostopic pub -1 /wx250s/commands/joint_group interbotix_xs_sdk/JointGroupCommand "name: 'arm' cmd: [0, -1.88, 1.5, 0.8, 0, 0]
```
Now, lets see the `ros services` you can call:
```
rosservice list
```
for example if we can check the *robot info*, this way:
```
rosservice call /wx250s/get_robot_info "cmd_type: 'group' name: 'arm'"
```
it will show us what *mode* is the arm group in and probably you will see the *position mode*.

Also, we are goint to use the `torque_enable` service. Use this (but only press enter if the robot arm is in the sleep position or you are holding it to prevent it from collapsing.):
```
rosservice call /wx250s/torque_enable "cmd_type: 'group' name: 'arm' enable: false"
```
and now you can manually manipulate it. For example, this could be useful to position the arm manually, and then reuse the previous command but with `enable: true`. And then, if you want to know what the join states are for that specific pose you can type:
```
rostopic echo /wx250s/joint_states
```
Another thing that could be interesting is the parameters list (or launch arguments) in `interbotix_ros_manipulators/interbotix_ros_xsarms/interbotix_xsarm_control/launch/xsarm_control.launch`. There for example, we can read the `load_configs` parameter (by default set to *true*). It will load all the *eeprom* area registers to the motors. After running the control launch file once you can set that parameter to false if you want your arm to take less time to boot up.
