# Fetch Joint Controls

A ROS1 package for joint-level trajectory control of the **Fetch robot**, with support for trajectory planning, smoothing, interpolation, and grasp planning integration. Developed for use in the ROAHM Lab (UMich) and as part of the DeepRob course project.

## What This Does

1. **Joint trajectory execution**: Commands the Fetch robot's arm joints to follow desired position/velocity profiles published via ROS topics.
2. **Trajectory planning and smoothing**: Computes smooth joint trajectories from waypoints using interpolation (spline/polynomial fitting) and discretization.
3. **Optimizer integration**: Subscribes to optimizer-generated trajectory messages (`optimizerMessage.msg`, `traj_opt.msg`) and executes them on the robot.
4. **Grasp planning**: Integration scripts for PointNet-based and TransGrasp-based grasp candidate generation and execution.
5. **IK testing**: Standalone module to test inverse kinematics solutions.

## Package Structure

```
fetch_joint_controls/
├── scripts/
│   ├── test_joints.py              # Basic joint position/velocity testing
│   ├── point_net_grasp.py          # PointNet grasp execution
│   ├── trans_grasp.py              # TransGrasp integration
│   ├── Test/
│   │   ├── viz_best_grasp.py       # Visualize top grasp candidates
│   │   ├── grasp_cluster.py        # Cluster and filter grasp poses
│   │   └── transformed_grasp_pose.csv
├── msg/
│   ├── traj_opt.msg                # Trajectory optimizer output message
│   └── optimizerMessage.msg        # Optimizer command message
├── test/
│   ├── plot.py                     # General trajectory plotting
│   ├── plotJoint0.py               # Single joint debug plot
│   ├── full_plotting_script.py     # Full multi-joint trajectory plots
│   └── *.csv                       # Logged commanded/actual joint data
├── trajectory_plots/               # Saved trajectory visualization PNGs + MP4
├── Map/
│   ├── mymap.yaml                  # ROS nav map
│   └── mymap.pgm
└── configs/deeprob/
    └── deeprob.rviz                # RViz config for DeepRob experiments
```

## Custom ROS Messages

```
traj_opt.msg
  float64[] positions
  float64[] velocities
  float64[] accelerations
  float64   duration

optimizerMessage.msg
  # Optimizer-generated trajectory command
```

## Running

```bash
# Build
cd <catkin_ws>
catkin_make
source devel/setup.bash

# Test basic joint control
rosrun fetch_joint_controls test_joints.py

# Run with optimizer input
rosrun fetch_joint_controls <your_controller_node>.py

# Grasp with PointNet
rosrun fetch_joint_controls point_net_grasp.py

# Grasp with TransGrasp
rosrun fetch_joint_controls trans_grasp.py
```

## Docker

```bash
# Copy data to/from Docker for TransGrasp/PointNet
bash scripts/transcg_host_to_docker.sh
bash scripts/host_to_docker.sh
```

## Dependencies

- ROS1 (Noetic)
- `fetch_ros` driver
- `scipy`, `numpy`, `matplotlib` (trajectory planning/plotting)
- PointNet++ or TransGrasp (for grasp modules)
