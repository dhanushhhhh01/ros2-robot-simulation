# 🤖 ROS 2 Robot Simulation - Fleet Orchestration

> Multi-robot fleet management system using ROS 2 Humble and Gazebo Harmonic. Features autonomous navigation with Nav2, dynamic task allocation, conflict resolution, and MQTT bridge for external integration.
>
> [![ROS2](https://img.shields.io/badge/ROS%202-Humble-22314E?style=for-the-badge&logo=ros&logoColor=white)](https://docs.ros.org/en/humble)
> [![Gazebo](https://img.shields.io/badge/Gazebo-Harmonic-000000?style=for-the-badge)](https://gazebosim.org)
> [![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
> [![Nav2](https://img.shields.io/badge/Nav2-Navigation-blue?style=for-the-badge)](https://nav2.org)
> [![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docker.com)
>
> ---
>
> ## 🏗️ System Architecture
>
> ```
> External Systems (MQTT/REST)
>           │
>           ▼
> ┌─────────────────────────────────┐
> │    Fleet Orchestrator Node      │ /fleet/task_command
> │  - Task allocation              │ /fleet/status
> │  - Conflict resolution          │ /fleet/emergency_stop
> │  - Status monitoring            │
> └──────┬──────────────────────────┘
>        │  Nav2 Action Goals
>        ▼
> ┌──────────────────────────────────────────┐
> │         Individual AMR Nodes             │
> │  AMR_01 │ AMR_02 │ AMR_03               │
> │  Nav2   │ Nav2   │ Nav2                 │
> │  SLAM   │ SLAM   │ SLAM                 │
> └──────────────────────────────────────────┘
>           │
>           ▼
>    Gazebo Harmonic Simulation
>    (Warehouse environment)
> ```
>
> ---
>
> ## 📁 Project Structure
>
> ```
> ros2-robot-simulation/
> ├── src/
> │   ├── fleet_orchestrator/
> │   │   ├── orchestrator_node.py   # Main fleet coordinator (ROS 2 Node)
> │   │   ├── task_allocator.py      # Nearest-robot task assignment
> │   │   ├── robot_registry.py      # Robot state tracking
> │   │   └── conflict_resolver.py   # Spatial conflict detection
> │   ├── amr_navigation/
> │   │   ├── navigator_node.py      # Per-robot Nav2 interface
> │   │   ├── waypoint_follower.py   # Multi-waypoint mission execution
> │   │   ├── docking_controller.py  # Charging dock approach
> │   │   └── config/nav2_params.yaml
> │   ├── mqtt_ros2_bridge/
> │   │   ├── bridge_node.py         # MQTT <-> ROS 2 topic bridge
> │   │   └── message_mapper.py      # JSON <-> ROS msg conversion
> │   └── simulation_worlds/
> │       ├── worlds/warehouse.sdf   # Gazebo warehouse world
> │       ├── models/amr_robot/      # AMR robot model
> │       └── launch/
> │           ├── simulation.launch.py
> │           └── fleet.launch.py
> ├── scripts/
> │   ├── spawn_robot.py             # CLI: spawn robots in sim
> │   ├── send_goal.py               # CLI: send navigation goal
> │   └── fleet_status.py            # CLI: monitor fleet
> ├── docker/
> │   ├── Dockerfile.ros2
> │   └── docker-compose.yml
> └── README.md
> ```
>
> ---
>
> ## 🚀 Quick Start
>
> ### Using Docker (Recommended)
>
> ```bash
> git clone https://github.com/dhanushhhhh01/ros2-robot-simulation.git
> cd ros2-robot-simulation
>
> # Build and launch simulation
> docker-compose up -d
>
> # Open Gazebo GUI
> xhost +local:docker
> docker exec -it ros2_sim ros2 launch simulation_worlds simulation.launch.py
> ```
>
> ### Native ROS 2 Setup
>
> ```bash
> # Source ROS 2
> source /opt/ros/humble/setup.bash
>
> # Build workspace
> colcon build --symlink-install
> source install/setup.bash
>
> # Launch full fleet simulation
> ros2 launch simulation_worlds fleet.launch.py fleet_size:=3
> ```
>
> ---
>
> ## 📡 Topics & Actions
>
> | Topic/Action | Type | Direction | Description |
> |---|---|---|---|
> | `/fleet/task_command` | `std_msgs/String` | IN | JSON task command |
> | `/fleet/status` | `std_msgs/String` | OUT | Fleet status JSON |
> | `/fleet/emergency_stop` | `std_msgs/Bool` | IN | E-stop trigger |
> | `/amr_XX/navigate_to_pose` | `nav2_msgs/NavigateToPose` | Action | Per-robot Nav2 goal |
> | `/amr_XX/odom` | `nav_msgs/Odometry` | OUT | Robot pose |
>
> ---
>
> ## 🎯 Send a Navigation Goal
>
> ```bash
> # Send task via CLI
> python3 scripts/send_goal.py \
>   --robot amr_01 \
>   --x 5.0 --y 3.0 --yaw 0.0 \
>   --task_id PICK_001
>
> # Monitor fleet
> python3 scripts/fleet_status.py --rate 2
> ```
>
> ---
>
> ## 📬 Author
>
> **Dhanush Ramesh Babu** | [LinkedIn](https://linkedin.com/in/dhanushrameshbabu16) | MSc. Industry 4.0 @ SRH Berlin | 🟢 Open to Werkstudent/Internship
