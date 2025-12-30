# ROS2 Workspace Setup (Ubuntu 22.04 + ROS2 Humble)

This guide explains how to replicate the **ROS2 workspace** exactly as developed on the reference machine (Ubuntu 22.04, ROS 2 Humble).  
Follow each step carefully to ensure full compatibility.

---

## 🧭 1. Create the workspace

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
```

2. Clone all required repositories
1️⃣ Universal Robot ROS2 Description

```bash
git clone https://github.com/UniversalRobots/Universal_Robots_ROS2_Description.git
```
📖 Universal Robot ROS2 Gazebo Classic simulation
```bash
git clone -b humble https://github.com/UniversalRobots/Universal_Robots_ROS2_Gazebo_Simulation.git
rosdep update && rosdep install --ignore-src --from-paths . -y
cd ~/ros2_ws
colcon build --symlink-install
```

2️⃣ ABC-iRobot / Onrobot-ros2 repository
```bash
cd ~/ros2_ws/src
git clone https://github.com/ABC-iRobotics/onrobot-ros2.git
cd ~/ros2_ws
colcon build
```
- Now you have all the external repository available on your workspace.
- You can create your own package for custumizing the type of robot and assemble with OnRobot RG2 FT gripper 

3️⃣ LearnRoboticsWROS custom package for spawning UR5e + RG2 FT gripper
```bash
cd ~/ros2_ws/src
git clone https://github.com/LearnRoboticsWROS/ur5e_rg2.git
rosdep update && rosdep install --ignore-src --from-paths . -y
cd ~/ros2_ws
colcon build
source install/setup.bash
```

3. Test the simulation
```bash
ros2 launch ur5e_rg2 spawn_ur5e_camera_rg2.launch.py
```
- You should see ur5e robot spawning in Gezabo at 0.8 height from the center of the world

- Test the robot arm moving by sending a trajectory_msgs/JointTrajectory command:
- open a new terminal
```bash
ros2 topic pub /joint_trajectory_controller/joint_trajectory trajectory_msgs/JointTrajectory "{
  header: {
    stamp: {sec: 0, nanosec: 0},
    frame_id: ''
  },
  joint_names: [
    'shoulder_pan_joint',
    'shoulder_lift_joint',
    'elbow_joint',
    'wrist_1_joint',
    'wrist_2_joint',
    'wrist_3_joint'
  ],
  points: [
    {
      positions: [1.0, 0.5, -0.5, 1.2, 0.3, -1.0],
      velocities: [],
      accelerations: [],
      effort: [],
      time_from_start: {sec: 2, nanosec: 0}
    }
  ]
}"
```

- Test the gripper by sending std_msgs/msg/Float64MultiArray
- open a new terminal
```bash
ros2 topic pub --once /gripper_position_controller/commands std_msgs/msg/Float64MultiArray "{data: [0.8]}"
```

- You should see the gripper that close 

---
## 🙌 Credits

Developed by **Francesco Rizzotti**  
from [**Learn Robotics With ROS**](https://www.learn-robotics-with-ros.com)

📧 **Email:** [ros.master.ai@gmail.com](mailto:ros.master.ai@gmail.com)

Francesco is available for:
- Advanced ROS2 / MoveIt2 / Isaac Sim developments  
- Custom industrial deployments with real robots and vision system integrated 
- Vision-based application and AI-powered automation systems  

© 2025 Learn Robotics With ROS – All rights reserved.