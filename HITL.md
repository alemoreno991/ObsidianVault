Tags: #simulation 

---

## Overview

## Setup

Follow the [HITL documentation](https://docs.px4.io/main/en/simulation/hitl.html) 

```
cd <gnc-dam-c-src>/docker/hitl-simulation
docker compose up
```

> [!Warning] 
> You need to make sure that Pixhawk is available at `/dev/ttyAMC0`. Otherwise, you need to modify the `volume` to map the appropriate serial port.

> [!Note] 
> The serial device depends on what port is used to connect the vehicle to the computer (this is usually `/dev/ttyACM0`). An easy way to check on Ubuntu is to plug in the autopilot, open up a terminal, and type `dmesg | grep "tty"`. The correct device will be the last one shown.

## Issues encountered

### Tx queue overflow 

#### Problem description

Right after starting the gazebo simulator `gazebo Tools/simulation/gazebo-classic/sitl_gazebo-classic/worlds/hitl_iris.world` 

```
Tx queue overflow
Tx queue overflow
Tx queue overflow
Tx queue overflow
Tx queue overflow
Tx queue overflow
```

#### Solution

- [DID NOT WORK] Go to the following file `Tools/simulation/gazebo-classic/sitl_gazebo-classic/worlds/hitl_iris.world` and change the `real_time_update_rate` to 150

## Flashing the PX4 firmware 

### Pull the repository

```bash
# Clone the repository
git clone --shallow-since="2024-10-29T00:00:00Z" --recurse-submodules https://github.com/Asus-Robotics-and-AI-Center/px4-firmware.git
# Initialize the repo (maybe it's not needed but just in case)
cd px4-firmare
git pull
git submodule sync --recursive
git submodule update --init --recursive
```

### Setup the PixHawk board for HIL

> [!Warning]
> Only a couple Flight Controller Boards come pre-configured for HIL (not ours).

You need to enable the PWM (Pulse Width Modulation) in the simulated environment. This will allow one to command the simulated motors in Gazebo. 

```bash
# Set the PWM parameter needed for HIL 
cd <px-firmware>/boards/<your-board-type>/<your-board-model>
echo "CONFIG_MODULES_SIMULATION_PWM_OUT_SIM=y" >> default.px4board
```

Where the specifics in **GNC** (at the moment) are:

- `<your-board-type>`: `px4`
- `<your-board-model>`: `fmu-v6x`

### Build the firmware

```bash
# Build the firmware for your board
cd <px4-firmware>
# Make sure you start from a clean state
make clean
# Build the target
make <your-target-board>
```

Where the specifics in **GNC** (at the moment) are:

- `<your-target-board>`: `px4_fmu-v6x_default`

### Flash the firmware

> [!Note]
> Make sure to connect the flight controller to your laptop using the USB cable


```bash
# Flash the firmware to the board
make <your-target-board> upload
```

Where the specifics in **GNC** (at the moment) are:

- `<your-target-board>`: `px4_fmu-v6x_default`
# References

- [HITL testing for PX4 (YouTube)][https://www.youtube.com/watch?v=hgSc6fOrHt8]
- [Joystick Setup](https://docs.qgroundcontrol.com/Stable_V4.3/en/qgc-user-guide/setup_view/joystick.html)
- https://github.com/Asus-Robotics-and-AI-Center/siv-dam-c-src/blob/main/docs/hardware_setup/01_raspberry-pi-px4-setup.md
- https://github.com/Asus-Robotics-and-AI-Center/gnc-dam-c-src/blob/2bcea9ab08287d98dd5aa6e27da40ec771f489e0/docker/gnc-runtime-arm64/README.md
- https://docs.px4.io/v1.14/en/simulation/hitl.html#gazebo-classic
- https://docs.px4.io/v1.14/en/sim_gazebo_classic/
- https://docs.px4.io/main/en/dev_setup/building_px4.html