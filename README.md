# TurtleBot-20.04



//////// CHANGE ALL OF THIS ////////

This guide assumes you are on an ubuntu machine and that your turtlebot has a RasPi 3B/3B+. This will walk you through the process of getting a turtlebot3 running Ubuntu 20.04 with ROS Noetic and the required firmware. You will need a microSD card that is at least 16GB.

First plug in the microSD card to your computer. Download [balena etcher](https://www.balena.io/etcher/) and flash the robotis [3b+ noetic image](https://www.robotis.com/service/download.php?no=2008) onto it. It should take ~15 minutes to flash and verify the image. 

Once you have the image flashed open a terminal and open the .yaml file in the netplan directory.
`cd /media/$USER/writable/etc/netplan`
`sudo vim 50-cloud-init.yaml`


Then replace “WIFI_SSID” with “RBE” and “WIFI_PASSWORD” with “elm69wisest16poisoned”. This will set the turtlebot up to connect to the RBE wifi network local to ak120d. Now eject the microSD card and put it in the turtlebot.

Turn the turtlebot on and ssh into it with “ssh ubuntu@<TURTLEBOT_NAME>.dyn.wpi.edu”, where <TURTLEBOT_NAME>” is the name written on the turtlebot. The password to connect is “turtlebot”.

Open the .bashrc file with “vim ~/.bashrc”. In the line “export ROS_HOSTNAME={IP_ADDRESS_OF_RASBERRY_PI_3}” replace “{IP_ADDRESS_OF_RASBERRY_PI_3}” with “<TURTLEBOT_NAME>.dyn.wpi.edu”, using the same name used to ssh. Below that line add “OPENCR_PORT=/dev/ttyACM0” and “OPENCR_MODEL=burger_noetic”. Save the file and exit vim then source the .bashrc file.

Now set the timezone to EST using “timedatectl set-timezone America/New_York”. Then set the current date using “sudo ntpdate -s ntp1.wpi.edu”. Verify the datetime by running “date”.

Next install the required firmware by running the below commands:
 sudo dpkg –add-architecture armhf
 sudo apt update
 sudo apt install libc6:armhf
 rm -rf ./opencr_update.tar.bz2
 wget https://github.com/ROBOTIS-GIT/OpenCR-Binaries/raw/master/turtlebot3/ROS1/latest/opencr_update.tar.bz2
 tar -xvf opencr_update.tar.bz2
 cd ./opencr_update
 ./update.sh $OPENCR_PORT $OPENCR_MODEL.opencr

If this install completes successfully, then run the below commands to setup the catkin workspace:
 cd ~/catkin_ws
 rm -rf build devel
 catkin_make


If the catkin_make completes then the turtlebot is fully setup to be used for RBE 3002
