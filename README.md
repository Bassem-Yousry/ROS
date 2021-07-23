# Over The Air RC vehicle for ROS
 [ROS](http://www.ros.org/) package to control an RC vehicle , connect with cloud (Docker and AWS) based on Raspberry Pi4.
## Prerequisites
This project assumes that you have already installed and run the [Dashboard](https://github.com/VirtualabIoTDashboard/OTA_RemoteDrivingDashboard)
## PC setup
### Requirements
- [ROS Noetic](http://wiki.ros.org/noetic/Installation/Ubuntu) Other versions may work, however they have not been tested

```bash
cd $HOME/PC_WS
# If missing dependencies are serious enough,
# use 'pip3' to install Python dependencies, 'apt' for any other dependencies
rosdep check --from-paths src/ -i

# finally build the workspace:
catkin_make 

```

#### Run
```bash
source $HOME/PC_WS/devel/setup.bash
roscore
rosrun CloudConnectNodes real-time_actuate.py 
rosrun CloudConnectNodes webcam_sub.py 

```


## Raspberry PI setup
### Hardware
![{4ED04FED-06D2-44B9-89ED-2CDC35C720DC} png](https://user-images.githubusercontent.com/63354831/126548418-9d62d7d1-16a9-4d9d-bad0-5ba33eab0aa1.jpg)
![{F23D11E7-BC1B-4BB6-8ACC-B5CFB3FAD4B4} png](https://user-images.githubusercontent.com/63354831/126548423-4a4d1e1e-6b9d-438a-b133-8b800f79b08f.jpg)
### Circuit Diagram 
![WhatsApp Image 2021-07-18 at 11 44 24 PM](https://user-images.githubusercontent.com/63354831/126550374-f945384c-5cfc-4376-9f50-e8855759a196.jpeg)
### Configuration:
In current version, the platform has a camera , speed senor ,3 ultrasonics , servo motor ,h bridge and gps module  
### Requirements
- [ROS Melodic](http://wiki.ros.org/melodic/Installation/Ubuntu) 
- Python3
- [ROS on Multiple Computers | Connecting Raspberry Pi with PC](https://razbotics.wordpress.com/2018/01/23/ros-distributed-systems/)
```bash
cd $HOME/rpiWS
#make sure these are installed
sudo apt install libpython3-dev python-catkin-tools
pip3 install -U catkin_tools

# these modules are needed but are not automatically installed
pip3 install wheel empy defusedxml netifaces

# just the standard build-ros-from-source dependencies
pip3 install -U rosdep rosinstall_generator wstool rosinstall


# I use check here to first see what is missing. If missing dependencies are serious enough,
# use 'pip3' to install Python dependencies, 'apt' for any other dependencies
rosdep check --from-paths src/ -i

# finally build the workspace:
catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python3

source $HOME/rpiWS/devel/setup.bash

```
### AWS Setup

#### AWS Credentials
You will need to create an AWS Account and configure the credentials to be able to communicate with AWS services. You may find [AWS Configuration and Credential Files](https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html) helpful.
```bash
sudo apt-get install -y ros-$ROS_DISTRO-cloudwatch-metrics-collector
sudo apt-get install -y ros-$ROS_DISTRO-ros-monitoring-msgs
sudo apt-get install -y ros-$ROS_DISTRO-kinesis-video-streamer
sudo apt-get install -y ros-$ROS_DISTRO-h264-video-encoder

```
#### Configuration File 
An example configuration file is provided that contains a detailed example configuration for the Node.
for more info :
- [cloudwatch](https://github.com/aws-robotics/cloudwatchmetrics-ros1)
- [KinesisVS](https://github.com/aws-robotics/kinesisvideo-ros1)
- [h264encoder](https://github.com/aws-robotics/kinesisvideo-encoder-ros1) #subscription_topic: "/usb_cam/image_raw"
 
#### Run
```bash
source ~/rc-car/devel/setup.bash

roslaunch usb_cam usb_cam-test.launch
roslaunch sense senseLaunch.launch
rosrun actuate actuatereciever.py

#AWS Nodes
roslaunch cloudwatch_metrics_collector sample_application.launch 
roslaunch kinesis_video_streamer sample_application.launch 
```
## ROS System Diagram
![Diagram](./img.png?raw=true "Title")

## Complite tasks of the project:
* remote controll via ros_node
* streaming service
* connect with Docker and AWS 
* read sensors data (gps , ultra sonic , temp and speed ) send them to The cloud
