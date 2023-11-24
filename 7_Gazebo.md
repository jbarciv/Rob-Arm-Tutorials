## #6 Interbotix with Gazebo

Here you will learn two different ways that Gazebo can be used to work with a simulated arm. 
1) The first approach involves **creating a custom ROS node to directly interface with the Gazebo ROS topics** that control each joint’s movement. 
2) The second approach shows **how MoveIt can be used to control the Gazebo simulated arm** via the FollowJointTrajectory Action interface. 

By the end of the video, you should have a good idea of which approach is more applicable to your application.

Start typing:
```
roslaunch interbotix_xsarm_gazebo xsarm_gazebo.launch robot_model:=wx250s dof:=6 use_position_controller:=true
```
note that we could also use `use_trajectory_controllers:=true` as well. And the difference is:
- `position_controllers`: it creates a ROS control interface to control each joint independently. Like if a joint is at 0 degrees and you want to go to 1.5 radians you just command to a certain topic to go 1.5 radians and it'll go there.
- `trajectory_controllers`: we will be using it with MoveIt because the way movement interfaces with Gazebo is it sends a joint trajectory message wich is a list of positions and could be velocities and accelerations that all the joints should be at given moments and time. 

Now type in another terminal:
```
rosservice call /gazebo/unpause_physics
```
it is very important to do that. That's going to unpause the physics engine.

If you then type: `rostopic list` you will see some of the topics you want to pusblish for positions to command the different positions for each of these joints.

An example of using one of them could be (for moving the wrist angle):
```
rostopic pub -1 /wx250s/wrist_angle_controller/command std_msgs/Float64 "data: -1.0"
```
And basically this is the way to work with the arm in Gazebo by directly interfacing with the topics. In this case also, if you want to close the gripper you would actually have to publish to the left and right finger controller topics with the same but opposite command.

If now we `ctrl+c` the terminal where the master is running you should notice how Gazebo takes a few seconds to shut itself down... (because it is such a processor intensive application).

Now let's lauch **MoveIt with Gazebo**:
```
roslaunch interbotix_xsarm_moveit xsarm_moveit.launch robot_model:=wx250s dof:=6 use_gazebo:=true
```
Rviz might hang until you unpause the physics engine in Gazebo:
```
rosservice call /gazebo/unpause_physics
```
Some things in Rviz for Moveit:
- If you want to show the *green arm* that represents the start state of the robot you can just go to `MotionPlanning/Planning Request` and check the `Query Start State`.
- The same fo the *orange robot* that represents the goal state, checking the `Query Goal State`. 

After draging and moving the robot arm to the desired position there are some commands buttons in the MotionPlanning panel. For example the `Plan` button, the `Execute` and the `Plan & Execute`. Sometimes if the executions fails, you should update the `Start State` to be *\<current>*. We can also do random poses changing `Goal State` to be *\<ramdom valid>*.

It is good to know is that you have control over how fast the arm should move: so there is something called `velocity scaling` and `acceleration scaling`. If you go to your `interbotix_ros_manipulators/interbotix_ros_xsarm/interbotix_xsarm_moveit/config/joint_limits/6dof_joint_limits.yaml` in the repository. Using the options offered by the MotionPlanning panel you will be scaling the max velocity and max acceleration listed in the `.yaml` document commented above.

You can control the gripper. In the `Planning Request/Planning Group` change the *interbotix_arm* by *interbotix_gripper*. 

