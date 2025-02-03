
## Gazebo classic

Launch the simulation with dynamic resolution of the `ttyACM` port used by the Flight Controller.
```bash
cd <gnc-dam-c-src>/docker/hitl-simulation
PIXHAWK_TTY_PORT=$(sudo dmesg | grep -i ttyACM | tail -n 1 | awk '{print $5}' | sed 's/.$//') docker compose up
```

> []
## PixHawk

Go to `QGroundControl -> Analyze tool -> MAVLink console` for all the commands in this section.
### Reset the flight controller
> [!Warning]
> You will need to restart the Gazebo-classic simulation if you reboot. 

> [!Tip]
> For now this is the simplest way to relaunch a test and make sure everything is fresh as a lettuce.


```
reboot
```
### Reset the UDP port 

```
mavlink stop -u 14550
mavlink start -u 14550 -o 14550
```