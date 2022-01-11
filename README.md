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
<img src="https://render.githubusercontent.com/render/math?math=TTC = \frac{r}{[-\dot{r}]_+} ">


## III. Automatic Emergency Braking with TTC

## IV: Deliverables and Submission
In addition to the three deliverables described in this document, fill in the answers to the questions listed in **`SUBMISSION.md`**.

We'll be using Github classroom throughout the semester to manage submissions for lab assignments. After you're finished, directly commit and push to the repo Github classroom created for you.

## V: Grading Rubric
- Compilation: **30** Points
- Provided Video: **20** Points
- Correctly stops before collision: **30** Points
- Correctly calculates TTC: **10** Points
- Able to navigate through the hallway: **10** Points