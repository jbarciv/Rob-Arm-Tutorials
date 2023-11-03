## #2 Perception Pipeline Tuning

**Aim:** Work with Interbotix arm vision kit \
**Objectives:**
- Useful to figure out joint limits and which way is positive rotation
- Counter Clock Wise is positive. Thumb points along positive axis of rotation
- Where all the Transport System (TF) joints are located - point out base_link and ee_gripper_link frame

Make sure you have
- a computer with running ubuntu linux and ros
- have the interbotix ros package
- (hardware): make sure your arm is connected to computer over a micro usb cable
- you have a $12$ volt power source plugged into the arm
- do you have a camera and it stands and is facing towards your tabletop surface (usb 3 cable connection recommended)

Now, type:
```
roslaunch interbotix_xsarm_perception xsarm_perception.launch robot_model:=wx250s use_armtag_tuner_gui:=true use_pointcloud_tuner_gui:=true
```
 Now manually manipulate the arm so the ar ARtag that is on the end effector is in full view of the camera. And them torque the arm back. A brief reminder:
 ```
 rosservice call /wx250s/torque_enable group arm false
 ```
 In the ArmTag Tuner GUI increase `Num Samples` up to $10$ for a maximum acuracy.
 The values for the *snapped pose* (the transform from the 'camera_color_optical_frame' frame to the 'wx250s/base_link' frame) will be saved to a *yaml* file called `statictransforms.yaml` after ros is shut down. So if you are not going to move the camera or robot from where it is currently set up, you only have to do this once. But if you will need to run this calibration procedure again.

Then from minute [15:50](https://youtu.be/UesfMYM4qcc?list=PL8X3t2QTE54sMTCF59t0pTFXgAmdf0Y9t&t=958) the video will explain how to tune the filter parameters (with PointCloud Tuner GUI). 