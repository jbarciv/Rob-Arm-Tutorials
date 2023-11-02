# Arm-Rob-Tutorials: Review
Here are some brief notes that serve as a summary and shortcut for putting the Interbotix X-series robotic arms into use. In our case we will use the [WidowX 250 (6DOF) model](https://www.trossenrobotics.com/widowx-250-robot-arm-6dof.aspx#documentation).

The tutorials come from [this Youtube channel](https://www.youtube.com/watch?v=o9EXEgzbAxQ&list=PL8X3t2QTE54sMTCF59t0pTFXgAmdf0Y9t&ab_channel=TrossenRobotics) (many thanks to Interbotix and all credit for them)

## #1 Getting Started With The X-Series Arm

**Aim:** Rub interbotix_xsarm_descriptions \
**Objectives:**
- Useful to figure out joint limits and which way is postive rotation
- Counter Clock Wise is positive. Thumb points along positive axis of rotation
- Where all the Transport System (TF) joints are located - point out base_link and ee_gripper_link frame

First run:
```
roslaunch interbotix_xsarm_descriptions xsarm_description.launch robot_model:=wx250s use_joint_pub_gui:=true
```
you can include `show_ar_tag:=true` in the launch command to include a square black piece used in the perception ross package. 

Then for controlling the robot arm directly use:
```
roslaunch interbotix_xsarm_control xsarm_control.launch robot_model:=wx250s
```
With `rostopic list` to see published and subscribers. You will see for example this topic:

> /wx250s/joint_states
and using
```
rostopic echo /wx250s/joint_states
```
you will be able to see all the join_states message information, like this:
> name: [waist, shoulder, elbow, wrist_angle, wrist_rotate, gripper, left_finger, right_finger]\
> position: [0, 0, 0, 0, 0, 0, 0, 0]\
> velocity: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]\
> effort: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]
