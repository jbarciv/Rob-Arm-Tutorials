## #0 Installation

All info [here](https://www.trossenrobotics.com/docs/interbotix_xsarms/ros_interface/software_setup.html).

> Computer running Ubuntu Linux 18.04 or **20.04**\
>*Virtual Linux machines* have not been tested *are not supported*.

Basic steps:
```
sudo apt install curl

curl 'https://raw.githubusercontent.com/Interbotix/interbotix_ros_manipulators/main/interbotix_ros_xsarms/install/amd64/xsarm_amd64_install.sh' > xsarm_amd64_install.sh

chmod +x xsarm_amd64_install.sh

./xsarm_amd64_install.sh -d noetic
```
I recommend let the installer to install ROS Noetic...