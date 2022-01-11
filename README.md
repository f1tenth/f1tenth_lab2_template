# Lab 1: Automatic Emergency Braking

## I. Learning Goals

- Using the LaserScan message in ROS
- Time to Collision (TTC)
- Safety critical systems

## II. Overview

The goal of this lab is to develop a safety node for the race cars that will stop the car from
collision when travelling at higher velocities. We will implement Time to Collision (TTC) using
the `LaserScan` message in the simulator.

For different ROS2 messages, they are mostly same as in ROS1. You can use `ros2 interface show <import directory>` to see the definition of messages.

### The `LaserScan` Message

[LaserScan](http://docs.ros.org/en/noetic/api/sensor_msgs/html/msg/LaserScan.html) message contains several fields that will be useful to us. You'll need to subscribe to the scan topic and calculate TTC with the LaserScan messages.

### The `Odometry` Message

Both the simulator node and the car itself publish [Odometry](http://docs.ros.org/en/noetic/api/nav_msgs/html/msg/Odometry.html) messages. Within its several fields, the message includes the cars position, orientation, and velocity. You'll need to explore this message type in this lab.

### The `AckermannDriveStamped` Message

[AckermannDriveStamped](http://docs.ros.org/en/jade/api/ackermann_msgs/html/msg/AckermannDriveStamped.html) is the message type that well use throughout the course to send driving commands to the simulator and the car. Note that we won't be sending driving commands to the car from this node, were only sending the brake commands. By sending an AckermannDriveStamped message with the velocity set to 0.0, the simulator and the car will interpret this as a brake command and hit the brakes.

### The TTC Calculation

Time to Collision (TTC) is the time it would take for the car to collide with an obstacle if it maintained its current heading and velocity. Between the car and its obstacle, we can calculate it as:

![TTC=\frac{r}{[-\dot{r}]_+}](https://latex.codecogs.com/svg.latex?TTC=\frac{r}{[-\dot{r}]_+}) 

where ![r](https://latex.codecogs.com/svg.latex?r) 
is the distance between the two objects and 
![\dot{r}](https://latex.codecogs.com/svg.latex?\dot{r}) is the time derivative of that distance. 
![\dot{r}](https://latex.codecogs.com/svg.latex?\dot{r}) is computed by projecting the relative velocity of the car onto the distance vector between the two objects. The operator 
![[]_+](https://latex.codecogs.com/svg.latex?[]_+) is defined as: 
![[x]_+:=max(x,0)](https://latex.codecogs.com/svg.latex?[x]_+:=max(x,0))
.

You’ll need to calculate the TTC for each beam in the laser scan. Projecting the velocity of the car onto each distance vector is very simple if you know the angle between the cars velocity vector and the distance vector (which can be determined easily from information in the `LaserScan` message).

## III. Automatic Emergency Braking with TTC

For this lab, you will make a Safety Node that should halt the car before it collides with obstacles.
To do this, you will make a ROS node that subscribes to the LaserScan and Odometry messages.
It should analyze the `LaserScan` data and, if necessary, publish an `AckermannDriveStamped` with the velocity field set to 0.0 m/s.

Note the following topic names for your publishers and subscribers (also detailed in the skeleton
code):

- LaserScan: /scan
- Odometry: /odom
- Drive: /drive

## IV: Deliverables and Submission
You can implement this node in either C++ or Python. Please follow the submission instructions in **`SUBMISSION.md`**.

We'll be using Github classroom throughout the semester to manage submissions for lab assignments. After you're finished, directly commit and push to the repo Github classroom created for you.

## V: Grading Rubric
- Compilation: **30** Points
- Provided Video: **20** Points
- Correctly stops before collision: **30** Points
- Correctly calculates TTC: **10** Points
- Able to navigate through the hallway: **10** Points
