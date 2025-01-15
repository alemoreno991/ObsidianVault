```dockerfile
FROM ubuntu:22.04

RUN apt update && apt install --no-install-recommends -y git

WORKDIR /home
RUN git clone --depth=1 --branch=v1.14.3 https://github.com/PX4/PX4-Autopilot.git

WORKDIR /home/PX4-Autopilot/Tools/setup
RUN ./ubuntu.sh
RUN apt remove gz-harmonic && \
	apt install aptitude && \
	aptitude install gazebo libgazebo11 libgazebo-dev

WORKDIR /home/PX4-Autopilot
RUN DONT_RUN=1 make px4_sitl_default gazebo-classic
```
