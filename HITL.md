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
# References

- [HITL testing for PX4 (YouTube)][https://www.youtube.com/watch?v=hgSc6fOrHt8]
- [Joystick Setup](https://docs.qgroundcontrol.com/Stable_V4.3/en/qgc-user-guide/setup_view/joystick.html)