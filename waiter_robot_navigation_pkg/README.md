# Navigation Package with Waiter Robot

The Navigation package is to navigate robot in enviroment

## Instructions to Use Navigation with Waiter Robot

1. First, run the robot description with the following command:

    ```bash
    roslaunch waiter_robot_description_pkg robot_description.launch
    ```

   Output:

   ![Robot Description](https://github.com/Ahmed-M0ataz/waiter_robot/blob/main/waiter_robot_gmapping_pkg/media/waiter_robot_house.png)

2. Next, run the Navigation package:

    ```bash
    roslaunch waiter_robot_navigation_pkg waiter_navigation.launch  --screen 2> >(grep -Ev 'TF_REPEATED_DATA|buffer_core.cpp' | grep -v '^$')
    ```

   After running  navigation Package the robot in the environment:

   Output:

   ![Robot Navigation](https://github.com/Ahmed-M0ataz/waiter_robot/blob/main/waiter_robot_navigation_pkg/media/navigation_amcl.gif)

To run STVL 


1. First, run the robot description with the following command:

    ```bash
    roslaunch waiter_robot_description_pkg robot_description.launch
    ```
2. Next, run the Navigation package with STVL:

    ```bash
    roslaunch waiter_robot_navigation_pkg waiter_navigation_stvl.launch --screen 2> >(grep -Ev 'TF_REPEATED_DATA|buffer_core.cpp' | grep -v '^$')

    ```

    but there error is 
     ```bash
TF Exception that should never happen for sensor frame: , cloud frame: hokuyo_link, Lookup would require extrapolation 1.404000000s into the past. Requested time 99.225000000 but the earliest data is at time 100.629000000, when looking up transform from frame [hokuyo_link] to frame [odom]
    ```

   ![Robot Navigation error](https://github.com/Ahmed-M0ataz/waiter_robot/blob/main/waiter_robot_navigation_pkg/media/error_stvl.gif)