## #6 Interbotix Python API

Objectives:
- Understand how the Interbotix Python API is structured.
- Know the basics of working with X-Series arms using Python scripts.

Open a terminal and type:
```
roslaunch interbotix_xsarm_control xsarm_control.launch robot_model:=wx250s use_sim:=true
```
the python script that we will use can be found here: `interbotix_ros_manipulators/interbotix_ros_xsarm/examples/python_demos/bartender.py`, but there are others in the same folder.

The content is: 
```
from interbotix_xs_modules.arm import InterbotixManipulatorXS
import numpy as np
import sys

def main():
    bot = InterbotixManipulatorXS("wx250", "arm", "gripper")

    if (bot.arm.group_info.num_joints < 5):
        print('This demo requires the robot to have at least 5 joints!')
        sys.exit()

    bot.arm.set_ee_pose_components(x=0.3, z=0.2)
    bot.arm.set_single_joint_position("waist", np.pi/2.0)
    bot.gripper.open()
    bot.arm.set_ee_cartesian_trajectory(x=0.1, z=-0.16)
    bot.gripper.close()
    bot.arm.set_ee_cartesian_trajectory(x=-0.1, z=0.16)
    bot.arm.set_single_joint_position("waist", -np.pi/2.0)
    bot.arm.set_ee_cartesian_trajectory(pitch=1.5)
    bot.arm.set_ee_cartesian_trajectory(pitch=-1.5)
    bot.arm.set_single_joint_position("waist", np.pi/2.0)
    bot.arm.set_ee_cartesian_trajectory(x=0.1, z=-0.16)
    bot.gripper.open()
    bot.arm.set_ee_cartesian_trajectory(x=-0.1, z=0.16)
    bot.arm.go_to_home_pose()
    bot.arm.go_to_sleep_pose()

if __name__=='__main__':
    main()
```
This script makes the end-effector perform pick, pour, and place tasks (note that this script may not work for every arm as it was designed for the wx250). Make sure to adjust commanded joint positions and poses as necessary
To get started, open a terminal and type:
```
roslaunch interbotix_xsarm_control xsarm_control.launch robot_model:=wx250s dof:=6
```
then in another terminal change to this directory with:
```
cd ~/interbotix_ws/src/interbotix_ros_manipulators/interbotix_ros_xsarms/examples/python_demos/
```
there should be a file called bartender.py we can open it with *nano* so to check whether or not the robot model within the script corresponds to the real one we are going to use, so type:
```
nano bartender.py
```
and modify (if necessary) the robot model. Save and exit, finally execute with:
```
python3 bartender.py
```
(python if using ROS Melodic).