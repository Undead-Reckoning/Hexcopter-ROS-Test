This is a ROS 2 component that can be used to add a custom flight mode to a PX4 autopilot which will cause it fly in the shape of the letter R. It relies on [Auterion's PX4 ROS2 interface library](https://github.com/Auterion/px4-ros2-interface-lib) which has a number of other examples worth checking out.

## Requirements 
- Ubuntu (22.04)
- USE PX4 1.16, px4_msgs and px4_ros2_cpp must be on the 1.16 branch as well.
- PX4 and ROS 2 installations as detailed by https://docs.px4.io/main/en/ros2/user_guide.html 
    - This involves setting up the PX4 toolchain, ROS 2, and the Micro XRCE-DDS Agent 
- QGroundControl
    - Works with the latest QGC daily 

## Setup
1. Setup and build a new ROS workspace
    ```bash
    # Make a new folder for the workspace
    mkdir -p custom_flightmode/src
    cd custom_flightmode/src

    # Clone components
    git clone --branch release/1.16 https://github.com/PX4/px4_msgs.git
    git clone --branch release/1.16 https://github.com/Auterion/px4-ros2-interface-lib.git
    git clone https://github.com/Undead-Reckoning/Hexcopter-ROS-Test.git
    
    # Build the workspace and setup the shell to run ros commands later
    cd ..
    source /opt/ros/humble/setup.bash
    colcon build
    source install/setup.bash
    ```
2. **In a new terminal**, start MicroXRCEAgent
    ```bash
    MicroXRCEAgent udp4 -p 8888
    ```
3. **In another new terminal**, start a Gazebo PX4 simulation
    ```bash
    cd ~/PX4-Autopilot # Change this depending on where you installed PX4
    
    # Ignore these 2 lines if you are already on the correct version of PX4 (1.16)
    git checkout release/1.16
    git submodule update --init --recursive

    make px4_sitl gz_x500
    ```
4. Start QGroundControl. It should connect to the drone in the simulator.
5. **Return to the first terminal** and run the following. If you closed the terminal, you'll need to run the 2 `source` commands from the first step again beforehand.
    ```bash
    ros2 run draw_flight_mode draw_r
    ```
6. The drone should automatically arm, takeoff, draw the letter R, and then land. QGC may display a weird flight mode value; ignore this.
    - You can check that the custom flight mode is recognized by running `commander status` in the simulator shell. You should see an external flight mode named "Draw R".
