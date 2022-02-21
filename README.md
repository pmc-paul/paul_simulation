# paul_simulations

ROS package for the PAUL project simulations environment. Tested for ROS Melodic with Gazebo 9.0 on Ubuntu 18.04. SDF models found in the *paul_gazebo/models/* were taken from the Ignition Robotics library, available [here](https://app.ignitionrobotics.org/dashboard). These models are licensed under the Creative Commons 4.0 license.

## Installation

The installation is fairly easy. You only need to add the package in your ROS workspace in order to use it. The following steps create a workspace from scratch.

1. Create directory and go into *src/*:

    ```bash
    mkdir -p ~/catkin_ws/src && cd ~/catkin_ws/src
    ```

1. Clone the code from GitHub:

    ```bash
    git clone https://github.com/pmc-paul/paul_simulations.git
    ```

1. Go back to main directory:

    ```bash
    cd ~/catkin_ws
    ```

1. Install the missing dependencies:

    ```bash
    rosdep install --from-paths src --ignore-src -r -y
    ```

1. Build the package:

    ```bash
    catkin_make
    ```

## Usage

### Basic example

If no error occured while building the package, you should be able to start the simulation without problems. This can be done with the following command.

```bash
roslaunch paul_gazebo world.launch
```

This command will only launch Gazebo with the specified world. By default, this will be the simplified testbench. By passing the **world** argument, you can choose between *testbench_simplified*, *testbench* and *grocery*. 

### Kinova Gen3 and Gen3 Lite

This package was built with Kinova's Gen3 and Gen3 Lite arms. Both of these arms can be spawned in the testbench world with the following command.

```bash
roslaunch paul_gazebo arm_simulation.launch
```

Unlike *world.launch*, this file starts a ROS node to control the Gen3 and Gen3 Lite. Thus, the arm will be loaded in the **robot_description** parameter from Kinova's URDF. It is expected that you have the [ros_kortex](https://github.com/Kinovarobotics/ros_kortex) repository in your workspace. You can specify which type of arm to load with the **arm** parameter in the launch file. You can choose between *gen3* and *gen3_lite*.

### Custom launch file

Normally, you would start the simulation world from your main launch file. If that is the case for you, you'll need to add the following lines in your launch file. These lines are only useful for the mobile base simulation. It will launch the grocery world, and not the testbench.

```xml
<!-- Launch simulation environment -->
<group if="$(arg sim)">
    <!-- Launch gazebo -->
    <include file="$(find paul_gazebo)/launch/simulation.launch"/>
</group>
```

If you want to use the arm simulation in a custom launch file, don't use *arm_simulation.launch*. It's a better idea to only call the *world.launch* file with the required parameters.

```xml
<!-- Parameters -->
<arg name="gui" default="true"/>
<arg name="arm" default="gen3_lite"/>

<!-- Start the testbench in Gazebo -->
<include file="$(find paul_gazebo)/launch/world.launch">
    <arg name="world" value="testbench_simplified"/>
    <arg name="gui" value="$(arg gui)"/>
    <arg name="paused" value="true"/>
</include> 
```