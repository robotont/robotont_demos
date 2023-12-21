# robotont\_demos
This repository is a ROS package that contains various demos showing the capabilities of the Robotont platform

[![Build Status](https://github.com/robotont/robotont_demos/actions/workflows/industrial_ci_action.yml/badge.svg)](https://github.com/robotont/robotont_demos/actions/workflows/industrial_ci_action.yml)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

> [!WARNING]  
> This repository is no longer actively maintained, as its functionality has been transferred to a dedicated organization called [robotont-demos](https://github.com/robotont-demos).


## Before you begin
To run the demos it is necessary to have a Robotont robot and a user PC with Ubuntu Linux and ROS Noetic installed.

There are two approaches to get the Robotont and PC into the same ROS environment. A common prerequisite for both methods is that the hosts are connected to the same network. In the following examples, we assume the Robotont and the PC having the following configuration:

| Machine  | Hostname   | IP-address    |
|----------|------------|---------------|
| Robotont | robotont-1 | 192.168.1.1   |
| PC       | laptop-1   | 192.168.1.101 |

### Method 1: Hostname based setup

In this configuration, the robot and PC query each other via hostnames. It means that both hosts need to have each other's names associated with IP addresses. These hostname <--> IP pairs are defined in the `/etc/hosts` file. Use your favorite text editor and make sure the following entries exist.

**/etc/hosts on Robotont on-board computer:**
```bash
127.0.1.1 robotont-1
192.168.1.101 laptop-1
```

**/etc/hosts on PC:**
```bash
127.0.1.1 laptop-1
192.168.1.1 robotont-1
```

Next, we need to tell the PC to look for a ROS Master on Robotont. We do that by modifying a special environment variable named `ROS_MASTER_URI`, which by default points to localhost.

**on PC**, open a terminal and enter:
```bash
export ROS_MASTER_URI=http://robotont-1:11311
```
Now all ROS nodes you run in this terminal will connect to the Master on the Robotont. Test it with e.g. `rosnode list`.
Note that the environment variable has to be set for each terminal window! To make it automatic, you can add the line to the end of the `.bashrc` file in the home directory of the PC:

```bash
echo 'export ROS_MASTER_URI=http://robotont-1:11311' >> ~/.bashrc
```

### Method 2: IP-address based setup
If you want to configure IP based communication there is no need to edit the hosts file. Instead, a `ROS_IP` environmental variable has to be set on both sides:

**on Robotont on-board computer:**
```bash
export ROS_IP=192.168.200.1
```

**on PC:**
```bash
export ROS_MASTER_URI=http://192.168.200.1:11311
export ROS_IP=192.168.200.101
```

Similarly to the hostname based setup, append the commands to `.bashrc` to set the variables automatically.


## 3D mapping
### Prerequisites on Robotont on-board computer
 * [RTAB-Map ROS Wrapper](http://wiki.ros.org/rtabmap)

This dependency can be obtained from apt by entering the following commands:

```bash
sudo apt update
sudo apt install ros-noetic-rtabmap-ros
```

### Launching the demo

**On Robotont on-board computer**, launch 3d_mapping.launch<br/>
```bash
roslaunch robotont_demos 3d_mapping.launch
```

**On PC**, launch 3d_mapping_display.launch to visualize the result<br/>

```bash
roslaunch robotont_demos 3d_mapping_display.launch
```

## 2D mapping
### Prerequisites installed on the Robotont on-board computer
 * [gmapping](https://wiki.ros.org/gmapping)
 * [cartographer ROS Integration (optional)](https://google-cartographer-ros.readthedocs.io/en/latest/)
 * [hector slam (optional)](http://wiki.ros.org/hector_slam)

To install gmapping, enter the following commands:<br/>
```bash
sudo apt update
sudo apt install ros-noetic-gmapping
```


### Launching the demo

**On Robotont on-board computer**

Start the 2D SLAM:
```bash
roslaunch robotont_demos 2d_slam.launch
```

If hector_slam or cartographer have been installed in Robotont, one can select a different method with a **mapping_method** argument. For example, to use the default gmapping:

```bash
roslaunch robotont_demos 2d_slam.launch mapping_method:=gmapping
```

**On PC**, visualize the result in RViz with:<br/>
```bash
roslaunch robotont_demos 2d_slam_display.launch
```

## AR tracking

### Prerequisites installed on the Robotont on-board computer
 * [ar_track_alvar](http://wiki.ros.org/ar_track_alvar)

### Launching the demo

**On Robotont on-board computer**, launch ar_follow_the_leader.launch (change 5 with the AR tag number you intend to follow)<br/>
```bash
roslaunch robotont_demos ar_follow_the_leader.launch marker_id:=5
```

**On PC**, launch ar_marker_display.launch to visualize the result<br/>
```bash
roslaunch robotont_demos ar_marker_display.launch
```
