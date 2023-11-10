## #4 MoveIt Interface ROS Package

The focus will be in MoveGroup C++ and Python API

Start typing this:
```
roslaunch interbotix_xsarm_moveit_interface xsarm_moveit_interface.launch robot_model:=wx250s use_fake:true dof:=6 use_cpp_interface:=true
```
note how the `use_fake:=true` is used if there is not a real robot available.
Basically we will be using Rviz with the MoveIt plugin with a graphical user interface (GUI) done with the use of PyQT. Each of the buttons correspond to a ROS node that will send a message to the actual node that has the C++ move group API. You can find the code in this directory: `interbotix_ros_toolboxes/interbotix_common_toolbox/interbotix_moveit_interface/src/moveit_interface_obj.cpp` in Github.

Now with **Python**:
```
roslaunch interbotix_xsarm_moveit_interface xsarm_moveit_interface.launch robot_model:=wx250s use_fake:true dof:=6 use_python_interface:=true
```
the difference now is that we can also have a python script running. This script is located here: `interbotix_ros_toolboxes/interbotix_common_toolbox/interbotix_moveit_interface/scripts/moveit_python_interface`.

It could be helpful to use the following script configuration: `interbotix_ros_manipulators/interbotix_ros_xsarm/examples/interobtix_xsarm_moveit_interface/launch/xsarm_moveit_interface.launch` when you do your own development you might want to put that in your own launch file. You will run the xsarm moveit launch file separately and then you can run your own python interface node in another launch file.