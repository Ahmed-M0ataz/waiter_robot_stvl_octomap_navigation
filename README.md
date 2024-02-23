# Waiter Robot Graduation Project

## First: Robot Description Package

1. Write a URDF for the robot using XML and use xacro to make the code in separate files for readability.
   - ![Image: Robot](https://github.com/Ahmed-M0ataz/waiter_robot_stvl_octomap_navigation/blob/master/waiter_robot_description_pkg/media/waiter_robot.png)

2. Git Gazebo environment with real-world problems to ensure accurate navigation.
   - World includes:
     - Tables (challenge in 2D mapping and navigation with a tall robot)
     - People walking in the environment (dynamic environment challenge in navigation stack)
     - Glass (laser scan can't detect glasses)

3. Write a launch file to launch Gazebo and spawn the robot.
    ```bash
    roslaunch waiter_robot_description_pkg robot_description.launch
    ```
- ![Image: Gazebo](https://github.com/Ahmed-M0ataz/waiter_robot_stvl_octomap_navigation/blob/master/waiter_robot_description_pkg/media/waiter_robot_world.png)

## Second: Build Map

1. Implement Gmapping to build a 2D map.
   ![Robot Mapping](https://github.com/Ahmed-M0ataz/waiter_robot_stvl_octomap_navigation/blob/master/waiter_robot_gmapping_pkg/media/mapping_cafe.png)


- Problems with 2D map:
  - Laser can't detect glasses or edges for tables.

    ![Robot navigation](https://github.com/Ahmed-M0ataz/waiter_robot_stvl_octomap_navigation/blob/master/waiter_robot_gmapping_pkg/media/map_cafe_2d.png)

2. Explore 3D mapping options like [Octo-map](https://github.com/OctoMap/octomap_mapping) 

## Frist: Octomap

**-links help for make octomap**
1- [octomapping tutorial](https://www.youtube.com/watch?v=dF2mlKJqkUg)
2- [octomap with navigation](https://answers.ros.org/question/286337/octomap-navigation-how-to/)
3- [install for grid map](https://github.com/anybotics/grid_map)
4- [install octomap](https://answers.ros.org/question/270186/errorcannot-launch-node-octomap_serveroctomap_server_node/)

- Octomap installation and mapping instructions.

```bash
    sudo apt-get install ros-noetic-octomap    
```

```bash
    sudo apt-get install ros-noetic-octomap-mapping
```

```bash
    sudo apt-get install ros-noetic-octomap-msgs
```

## Mapping with Octomap

1. First, run the robot description package:

```bash
    roslaunch waiter_robot_description_pkg robot_description.launch
```

2. Next, run navigation with a 2D map to make the robot move and build the map:

```bash
    roslaunch waiter_robot_navigation_pkg waiter_navigation.launch --screen 2> >(grep -Ev 'TF_REPEATED_DATA|buffer_core.cpp' | grep -v '^$')
```

3. Run Octomap mapping:
```bash
    roslaunch waiter_robot_octomap_pkg octomap_mapping.launch --screen 2> >(grep -Ev 'TF_REPEATED_DATA|buffer_core.cpp' | grep -v '^$')
```

   ![octo mapping](https://github.com/Ahmed-M0ataz/waiter_robot_stvl_octomap_navigation/blob/master/waiter_robot_octomap_pkg/media/octomap_mapping.png)

4. After mapping with Octomap, save the generated map:

```bash
    rosrun octomap_server octomap_saver -f name_of_map.bt
```

   You can save it as a binary (.bt) or probabilistic (.ot) map.


5. To integrate this map into the navigation stack like a global planner, convert it to a PGM map:
    1. Run the node to convert the map to PGM:

```bash
        roslaunch waiter_robot_octomap_pkg octomap_to_gridmap.launch
```
    2. Run the map server to save the grid map:
```bash
        rosrun map_server map_saver --occ 60 --free 5 -f my_map_3 map:=/grid_map_visualization/elevation_grid
```

   After experimentation, the optimal threshold for occupied and free cells is 60.

   ![map 3D](https://github.com/Ahmed-M0ataz/waiter_robot_stvl_octomap_navigation/blob/master/waiter_robot_octomap_pkg/media/octo_map_2d.png)

   ![web based navigation](https://github.com/Ahmed-M0ataz/waiter_robot_stvl_octomap_navigation/blob/master/waiter_robot_gmapping_pkg/media/web_based_navigation.gif)

# Navigation

## Navigation Based on Octomap and STVL


1- Navigation is robust in tables or the kitchen but may fail when people come or when the map changes.

![navigation octomap](https://github.com/Ahmed-M0ataz/waiter_robot_stvl_octomap_navigation/blob/master/waiter_robot_octomap_pkg/media/octomap_grid_navigation.gif)

2- Spatio Temporal Voxel Layer (STVL) Algorithm

An algorithm for real-time 3D obstacle avoidance.

- Operates at nearly 400% less CPU load compared to voxel layer.
- Used in various environments globally.
- Suited for real-time collision avoidance.


3- Octomap

Powerful octree implementation for 3D mapping. Can build navigation and collision avoidance on top of Octomap’s octree.

- Memory-efficient solution for real-time collision avoidance applications.


- Navigation with stvl and octomap 

   ![navigation 3d with stvl](https://github.com/Ahmed-M0ataz/waiter_robot_stvl_octomap_navigation/blob/master/waiter_robot_gmapping_pkg/media/octomap_grid_navigation.gif)




### Web based Navigation

- use React To build the website 

- Use Ros bridge to link web site with ROS
```bash
sudo apt-get install ros-<rosdistro>-rosbridge-server

```
   ![web based navigation](https://github.com/Ahmed-M0ataz/waiter_robot_stvl_octomap_navigation/blob/master/waiter_robot_gmapping_pkg/media/web_based_navigation.gif)

### Video explain :

![waiter robot navigation video](https://github.com/Ahmed-M0ataz/waiter_robot_stvl_octomap_navigation/blob/master/waiter_robot_octomap_pkg/media/octomap_grid_navigation.gif)