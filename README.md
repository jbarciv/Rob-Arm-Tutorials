# Rob-Arm-Tutorials: a review
Here are some brief notes that serve as a summary and shortcut for putting the Interbotix X-series robotic arms into use. In our case we will use the [WidowX 250 (6DOF) model](https://www.trossenrobotics.com/widowx-250-robot-arm-6dof.aspx#documentation).

The tutorials come from [this Youtube channel](https://www.youtube.com/watch?v=o9EXEgzbAxQ&list=PL8X3t2QTE54sMTCF59t0pTFXgAmdf0Y9t&ab_channel=TrossenRobotics) (many thanks to Interbotix and all credit for them)

## Cheat Sheet

### Installation
```
sudo apt install curl

curl 'https://raw.githubusercontent.com/Interbotix/interbotix_ros_manipulators/main/interbotix_ros_xsarms/install/amd64/xsarm_amd64_install.sh' > xsarm_amd64_install.sh

chmod +x xsarm_amd64_install.sh

./xsarm_amd64_install.sh -d noetic
```
### Getting Started
To visualize the robot and move it **just in Rviz**:
```
roslaunch interbotix_xsarm_descriptions xsarm_description.launch robot_model:=wx250s use_joint_pub_gui:=true
```

To **move the robot arm manually**, launch this in one terminal: 
```
roslaunch interbotix_xsarm_control xsarm_control.launch robot_model:=wx250s
```
it will start in the *sleep pose*. Now, we are goint to use the `torque_enable` service. Use the next command (but only press enter if the robot arm is in the sleep position or you are holding it to prevent it from collapsing!):
```
rosservice call /wx250s/torque_enable "cmd_type: 'group'
name: 'arm'
enable: false"
```
and now you can manually manipulate it. For example, this could be useful to position the arm manually, and then reuse the previous command but with `enable: true`.
### Perception Pipeline
```
roslaunch interbotix_xsarm_perception xsarm_perception.launch robot_model:=wx250s use_armtag_tuner_gui:=true use_pointcloud_tuner_gui:=true
```
### Rviz Simulator
``` 
roslaunch interbotix_xsarm_control xsarm_control.launch robot_model:=wx250s use_rviz:=true use_sim:=true
```
For moving the robot arm with the use the **joystick ros package** (you will need a PS4 controller connected over bluetooth to your laptop) in terminal type:
```
roslaunch interbotix_xsarm_joy xsarm_joy.launch robot_model:=wx250s
```

![joystick](/gifs/joystick.gif)

### MoveIt in Rviz

```
roslaunch interbotix_xsarm_moveit_interface xsarm_moveit_interface.launch robot_model:=wx250s dof:=6 use_cpp_interface:=true use_actual:=true
```

### Record/Playback

Recording procedure:
```
roslaunch interbotix_xsarm_puppet xsarm_puppet_single.launch robot_model:=wx250s record:=true
```
then type `ctrl+c` to stop the recording process.

Now type:
```
roslaunch interbotix_xsarm_puppet xsarm_puppet_single.launch robot_model:=wx250s playback:=true bag_name:=wx250s_commands
```

![record_playback](/gifs/record_playback.gif)

### Gazebo
**Directly with ros topics**. Start typing:
```
roslaunch interbotix_xsarm_gazebo xsarm_gazebo.launch robot_model:=wx250s dof:=6 use_position_controller:=true
```
Now type in another terminal:
```
rosservice call /gazebo/unpause_physics
```
As an example, move the wrist angle with this rostopic: 
```
rostopic pub -1 /wx250s/wrist_angle_controller/command std_msgs/Float6
```

**Now with MoveIt:**
```
roslaunch interbotix_xsarm_moveit xsarm_moveit.launch robot_model:=wx250s dof:=6 use_gazebo:=true
```
Rviz might hang until you unpause the physics engine in Gazebo:
```
rosservice call /gazebo/unpause_physics
```