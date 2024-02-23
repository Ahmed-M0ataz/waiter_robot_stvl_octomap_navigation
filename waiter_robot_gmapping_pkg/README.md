# Gmapping Package with Waiter Robot

The Gmapping package is utilized for localizing the waiter robot and building a map.

## Instructions to Use Gmapping with Waiter Robot

1. First, run the robot description with the following command:

    ```bash
    roslaunch waiter_robot_description_pkg robot_description.launch
    ```

   Output:

   ![Robot Description](https://github.com/Ahmed-M0ataz/waiter_robot/blob/main/waiter_robot_gmapping_pkg/media/waiter_robot_house.png)

2. Next, run the Gmapping package:

    ```bash
    roslaunch waiter_robot_gmapping_pkg gmapping.launch
    ```

3. Move the robot to build the map using teleop:

    ```bash
    rosrun teleop_twist_keyboard teleop_twist_keyboard.py
    ```

   After running these packages and navigating the robot in the environment:

   Output:

   ![Robot Mapping](https://github.com/Ahmed-M0ataz/waiter_robot/blob/main/waiter_robot_gmapping_pkg/media/gmapping_3.gif)

4. After completing the mapping, save the map:

    ```bash
    rosrun map_server map_saver -f waiter_robot_map
    ```

   This concludes the localization and mapping process.

   ![Mapping done](https://github.com/Ahmed-M0ataz/waiter_robot/blob/main/waiter_robot_gmapping_pkg/media/waiter_robot_house_map.png)

# Troubleshooting

If you encounter the following warning after running Gmapping:

```bash
[ WARN] [1702344438.657562065, 283.750000000]: TF_REPEATED_DATA ignoring data with redundant timestamp for frame base_footprint at time 283.669000 according to authority unknown_publisher
```
![Gmapping Warnings](https://github.com/Ahmed-M0ataz/waiter_robot/blob/main/waiter_robot_gmapping_pkg/media/warnnimg_gmapping.png)

2. **Solution:**
   Use the following command to launch Gmapping while filtering out specific warnings related to `TF_REPEATED_DATA` and `buffer_core.cpp`, as well as empty lines:

   ```bash
   roslaunch waiter_robot_gmapping_pkg gmapping.launch --screen 2> >(grep -Ev 'TF_REPEATED_DATA|buffer_core.cpp' | grep -v '^$')