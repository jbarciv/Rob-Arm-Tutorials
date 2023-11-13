## #5 Record/Playback

In the video there is documentation about moving two or more robots at the same time. But here we are only going to focus in the *process of recording the movements of a robot and then doing the same robot to reproduce the recorded motion*. This would be good for manually moving the arm to desired position and then playing it back as many times as necessary. We are going to use the `interbotix_xsarm_puppet` library.

This is the expected behavior:\
![record_playback](./record_playback)

Recording procedure:
```
roslaunch interbotix_xsarm_puppet xsarm_puppet_single.launch robot_model:=wx250s record:=true
```
then type `ctrl+c` to stop the recording process. There should be a new "bag" file in the  config directory. We can go there: 
```
roscd interbotix_xsarm_puppet/bag
```
there would be something called `wx250s_commands.bag` that the one we have just generated. We can go now to home directory for comodity:
```
cd ~
```
and type:
```
roslaunch interbotix_xsarm_puppet xsarm_puppet_single.launch robot_model:=wx250s playback:=true bag_name:=wx250s_commands
```
note that it is possible to use `use_sim:=true` if you want to simulate it first.

If you want to replay it you can keep *Rviz* open and in a nother Terminal type:
```
roscd interbotix_xsarm_puppet/bag/
```
and then
```
rosbag play wx250s_commands.bag
```