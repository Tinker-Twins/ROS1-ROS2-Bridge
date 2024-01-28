# ROS1-ROS2 Bridge

> **Note:** Tested with [ROS 1 Noetic](https://wiki.ros.org/noetic) and [ROS 2 Foxy](https://docs.ros.org/en/foxy/index.html) on [Ubuntu 20.04](https://releases.ubuntu.com/focal).

> **Demo:** [TurtleBot3](https://github.com/Tinker-Twins/TurtleBot3) simulation is launched using [ROS 1 Noetic](https://wiki.ros.org/noetic) and it is controlled from [ROS 2 Foxy](https://docs.ros.org/en/foxy/index.html) using [`ros1_bridge`](https://github.com/ros2/ros1_bridge) package.

1. Install [`ros1_bridge`](https://github.com/ros2/ros1_bridge) for ROS 2:

```bash
$ sudo apt update
$ sudo apt install ros-foxy-ros1-bridge
```

2. Fire up a terminal window and launch [TurtleBot3](https://github.com/Tinker-Twins/TurtleBot3) simulation using ROS 1:

```bash
$ source /opt/ros/noetic/setup.bash
$ source ~/ROS1_WS/TurtleBot/devel/setup.bash
$ roslaunch turtlebot3_gazebo turtlebot3_world.launch
```

3. Fire up another terminal tab/window and execute ROS 1 Bridge:

```bash
$ source /opt/ros/noetic/setup.bash
$ source /opt/ros/foxy/setup.bash
# ROS_DISTRO was set to 'noetic' before. Please make sure that the environment does not mix paths from different distributions.
$ echo $ROS_MASTER_URI
# http://localhost:11311
$ export ROS_MASTER_URI=http://localhost:11311
$ ros2 run ros1_bridge dynamic_bridge
# created 2to1 bridge for topic '/rosout' with ROS 2 type 'rcl_interfaces/msg/Log' and ROS 1 type 'rosgraph_msgs/Log' ...
```

4. Fire up yet another terminal tab/window to control the ROS 1 [TurtleBot3](https://github.com/Tinker-Twins/TurtleBot3) simulation from ROS 2:

```bash
$ source /opt/ros/foxy/setup.bash
$ ros2 topic pub /cmd_vel geometry_msgs/Twist "
linear:
  x: 0.5
  y: 0.0
  z: 0.0
angular:
  x: 0.0
  y: 0.0
  z: 0.0
"
# publisher: beginning loop
# publishing #1: geometry_msgs.msg.Twist(linear=geometry_msgs.msg.Vector3(x=0.5, y=0.0, z=0.0), angular=geometry_msgs.msg.Vector3(x=0.0, y=0.0, z=0.0)) ...
```

5. Observe the results:

```
1. Simulated TurtleBot3 should start moving forward with 0.5 m/s velocity.

2. In the terminal where you’re running the ros1_bridge, you should see messages similar to the following:
created 2to1 bridge for topic '/cmd_vel' with ROS 2 type 'geometry_msgs/msg/Twist' and ROS 1 type ''
[INFO] [1693968527.777201784] [ros_bridge]: Passing message from ROS 2 geometry_msgs/msg/Twist to ROS 1 geometry_msgs/Twist (showing msg only once per type)
[INFO] [1693968527.777518090] [ros_bridge]: Passing message from ROS 2 rcl_interfaces/msg/Log to ROS 1 rosgraph_msgs/Log (showing msg only once per type)created 2to1 bridge for topic '/cmd_vel' with ROS 2 type 'geometry_msgs/msg/Twist' and ROS 1 type ''
[INFO] [1693968527.777201784] [ros_bridge]: Passing message from ROS 2 geometry_msgs/msg/Twist to ROS 1 geometry_msgs/Twist (showing msg only once per type)
[INFO] [1693968527.777518090] [ros_bridge]: Passing message from ROS 2 rcl_interfaces/msg/Log to ROS 1 rosgraph_msgs/Log (showing msg only once per type)
```
