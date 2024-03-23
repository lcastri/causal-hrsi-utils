# Experimental Evaluation of ROS-Causal in Real-World Human-Robot Spatial Interaction Scenarios

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.10844902.svg)](https://doi.org/10.5281/zenodo.10844902)

<p float="left">
    <img src="https://github.com/lcastri/ROS-Causal/blob/master/images/A1.gif" width="32%" height="32%" />
    <img src="https://github.com/lcastri/ROS-Causal/blob/master/images/A2.gif" width="32%" height="32%" />
    <img src="https://github.com/lcastri/ROS-Causal/blob/master/images/A3.gif" width="32%" height="32%" />
    <br>
    <img src="https://github.com/lcastri/ROS-Causal/blob/master/images/A4.gif" width="32%" height="32%" />
    <img src="https://github.com/lcastri/ROS-Causal/blob/master/images/A5.gif" width="32%" height="32%" />
    <img src="https://github.com/lcastri/ROS-Causal/blob/master/images/A6.gif" width="32%" height="32%" />
    <!-- <br>
    <img src="https://github.com/lcastri/ROS-Causal/blob/master/images/A7.gif" width="32%" height="32%" />
    <img src="https://github.com/lcastri/ROS-Causal/blob/master/images/A8.gif" width="32%" height="32%" />
    <img src="https://github.com/lcastri/ROS-Causal/blob/master/images/A9.gif" width="32%" height="32%" />
    <br>
    <img src="https://github.com/lcastri/ROS-Causal/blob/master/images/A10.gif" width="32%" height="32%" />
    <img src="https://github.com/lcastri/ROS-Causal/blob/master/images/A11.gif" width="32%" height="32%" />
    <img src="https://github.com/lcastri/ROS-Causal/blob/master/images/A12.gif" width="32%" height="32%" />
    <br>
    <img src="https://github.com/lcastri/ROS-Causal/blob/master/images/A13.gif" width="32%" height="32%" />
    <img src="https://github.com/lcastri/ROS-Causal/blob/master/images/A14.gif" width="32%" height="32%" />
    <img src="https://github.com/lcastri/ROS-Causal/blob/master/images/A15.gif" width="32%" height="32%" /> -->
</p>

This repository contains the scripts for processing and visualising our dataset named "Human-Robot Spatial Interaction Dataset for Causal Analysis from Mobile Platforms".

The dataset captures a Human-Robot Spatial Interaction (HRSI) scenario between a person and the TIAGo robot. It includes: 
* rosbags containing: Velodyne LiDAR point cloud, robot and human state (position, orientation and velocities);
* CSV files containing trajectories of the person and the robot generated by post-processing the rosbags;
* the map of the environment extracted from the TIAGo robot.

**15 participants** took part in the experiment, with the dataset capturing **5 minutes of Human-Robot Spatial Interaction (HRSI) motion for each participant**.

The dataset is available on Zenodo at this [link](https://zenodo.org/records/10844902)

## 1 - RosBag Processing
The rosbags, contained in the **RosBags** folder of the [dataset](https://zenodo.org/records/10844902), have been processed to extract the trajectories of both the agent and the TIAGo robot for each of the 15 experiments. The processing pipeline involves two main steps:
1. Extracting the goal position of the participant for each timestep
2. Extracting the trajectory of the participant (along with the goal) and the robot

The first step is carried out by launching the **extract_Goal.launch** launch file in the **bag_processing_bringup** ROS node.
```
roslaunch bag_processing_bringup extract_Goal.launch bagname:=A1
```
> [!NOTE]
> Replace **A1** with the corresponding rosbag identifier for the participant, which could be any value between **A1** and **A15**. Ensure that the chosen rosbag is placed inside the **bags** folder of the **bag_processing_bringup** ROS node.

This procedure will generate a CSV file named A1_goal.csv in the **data** folder of the **bag_processing_bringup** ROS node.

The extracted CSV file is then utilised for the second step, executed by launching the **extract_Agent.launch** launch file in the **bag_processing_bringup** ROS node.
```
roslaunch bag_processing_bringup extract_Agent.launch bagname:=A1 only_visual:=False
```
> [!NOTE]
> Replace **A1** with the corresponding rosbag identifier for the participant, which could be any value between **A1** and **A15**. Ensure that the chosen rosbag is placed inside the **bags** folder of the **bag_processing_bringup** ROS node.
> [!NOTE]
> the **only_visual** flag determines whether to generate the CSV file containing the trajectories. If set to True, the CSV is not generated.

This process will generate a CSV file in the **traj** folder of the **bag_processing_bringup** ROS node and the post-processed CSV file in the **pptraj** folder of the same node. The post-processing script is located within the roscausal framework, specifically in /roscausal/roscausal_data/pp_scripts. It calculates the velocity of the participant (**v**), his/her distance to the goal (**$d_g$**) and the risk of collision with the robot (**r**) from the trajectories.

> [!NOTE]
> If you intend to derive different variables from the trajectories, you can place your post-processing script in the folder /roscausal/roscausal_data/pp_scripts.

## 2 - Trajectory Plot and Animation 
The CSV files containing the trajectories, which have already been processed, are stored in the **Trajectories** folder of the [dataset](https://zenodo.org/records/10844902). You can visualize these trajectories using two scripts located in the **trajectory_plot** folder:
1. trajPlot.py -- it plots both the participant and robot trajectories corresponding to the 5 minutes experiment
2. trajAnim.py -- it generates an animation of the experiment

For both scripts to function properly, it's necessary to place the CSV file containing the trajectories in the path **/trajectory_plot/trajectories**. Below is an example:


trajPlot of A1      |  trajAnim of A1 
:-------------------------:|:-------------------------:
![](https://github.com/lcastri/ROS-Causal/blob/master/images/A1_trajplot.png)  |  ![](https://github.com/lcastri/ROS-Causal/blob/master/images/A1.gif) 


