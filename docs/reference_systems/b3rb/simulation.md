# B3RB Simulation

Simulation uses [gazebo garden](https://gazebosim.org/home) to simulate sensors and physics in the ["dream" worlds](../../dream/worlds/worlds.md) that connects to Cerebri running ontop Zephyr RTOS `native_sim`.

## Before running simulation

Before running the simulation for the first time make sure to have first followed the [installation for development computer guide](../../getting_started/install.md). Once at [building the workspace](../../getting_started/install.md#build-the-workspace) make sure to select `1. b3rb` for the platform. This will also build Cerebri for `native_sim` so the section below **["Build Cerebri for `native_sim`"](#build-cerebri-for-native_sim)** can be skipped if the `build_workspace` script was just run. If other images have been built in Cerebri since running the script make sure to follow **["Build Cerebri for `native_sim`"](#build-cerebri-for-native_sim)**. 

## Build Cerebri for `native_sim`

To build [Cerebri for native_sim (posix)](../../cerebri/about.md) make sure the Zephyr RTOS build environment is up to date and that at some point previously the [`build_workspace` script was run for b3rb](../../getting_started/install.md#build-the-workspace).

??? tip "If the `build_workspace` script was just run, the steps in this section can be skipped."

    This section only needs to be run if different images have been built in Cerebri since having run the `build_workspace` script.

```bash title="Update Zephyr RTOS build environment with west:"
cd ~/cognipilot/ws/cerebri
git pull
west update
```

Build the Cerebri for `native_sim` and install it so it can be found.
```bash title="Build and install Cerebri for native_sim:"
west build -b native_sim app/b3rb/ -p -t install
``` 

## Run Electrode

???+ tip "If using the foxglove backend for Electrode."

     Make sure to have first run the `build_foxglove` script at some point and follow the prompts.
     ```bash title="Run build_foxglove script:"
     build_foxglove
     ```


To visualize and control navigation [run Electrode with the preffered backend set](../../electrode/about.md), the default and reccomended is foxglove.

### Run Electrode with the [foxglove backend](../../electrode/foxglove.md) for simulation.
```bash title="Electrode for simulation with foxglove:"
ros2 launch electrode electrode.launch.py sim:=true
``` 
??? question "How do I open the foxglove studio viewer?"

    [Click here for more information on how to get foxglove studio and open it.](../../electrode/foxglove.md)

??? question "I opened foxglove, how do I connect it to the simulation?"

    After launching the foxglove websocket bridge by passing to electrode `sim:=true`
    connect to it on `ws://localhost:8765`
    ![Connect foxglove with simulation websocket](images/foxglove_simulation_socket.png "Connect foxglove with simulation web socket")

??? picture "Example of depot world simulation with electrode running foxglove."

    ![B3RB Depot world simulation with foxglove](images/b3rb_depot_foxglove.png "B3RB Depot world simulation with foxglove")


### Optionally run Electrode with the [RVIZ 2 backend](../../electrode/rviz2.md) for simulation.
Electrode can be optionally run with the [RVIZ 2 backend](../../electrode/rviz2.md) for simulation, however, it requires a physical joystick device for input.
```bash title="Electrode for simulation with RVIZ 2:"
ros2 launch electrode electrode.launch.py gui:=rviz sim:=true joy:=true
```
??? picture "Example of depot world simulation with electrode running rviz2."

    ![B3RB Depot world simulation with rviz2](images/b3rb_depot_rviz.png "B3RB Depot world simulation with rviz2")


## Run B3RB SIL
The default dream world for B3RB is the [basic map world](../../dream/worlds/worlds.md#basic-map-world).
```bash title="Launch simulation with basic map world:"
ros2 launch b3rb_gz_bringup sil.launch.py
```

??? question "My ROS 2 cerebri_bringup node is showing an error and is keeping simulation from running."

    If the simulation launch script is throwing an error about cerebri_bringup make sure that [cerebri is built, installed and sourced properly for `native_sim`](#build-cerebri-for-native_sim).

!!! tip "**If running on a machine with a powerful graphics card optionally run the more gaphics intensive [depot world](../../dream/worlds/worlds.md#depot-world).**"

    ```bash title="Launch simulation with depot world:"
    ros2 launch b3rb_gz_bringup sil.launch.py world:=depot
    ```

## Video example of using the simulation
??? video "Example of using simulation with electrode running foxglove."

    <video controls>
      <source src="../videos/b3rb_depot_foxglove.mp4" type="video/mp4">
    </video>



