- [x] The SIV team made progress with streaming the messages out of the real hardware.
- [x] The FC was successfully hooked to the Laptop in HITL mode and I was able to fly the drone around with a PS5 joystick.
- [ ] There was also an issue to connect the real hardware (RPI and FC) which was addressed by a PR that Tomas worked on. This PR is gonna be merged soon into `main` so that we can create a release with all the changes we've done. 
- [x] Pierric and I worked very hard to improve the release process of the GNC application because we noticed that we are making changes very frequently and the release process with 2 branches (arm and x86) was introducing too much friction.
- [x] Chao-wei was able to conduct several unit tests on the PoseGraphInterface and the LocalMap. However, there were some issues when we tried to wrap those tests into our test suites that run in CI because of a `double free error` that we believe comes from one of our third party libraries (FCL or Octomap).
- [x] Agreed on a Protobuf schema with @neureddine for the EV-PX4.

## TODOs

- [ ] [Chao-Wei] (medium) End-to-end test of the LocalMap service.

- [ ] [Chris] (hard??) Solve some of the problems that the trajectory swapper is having.
- [ ] [Chris] (medium) Coordinate with BPRT on the goal messages (GPS or Local reference frame).

- [ ] [Alejandro] (medium-hard) External Vision into PX4.
- [x] [Alejandro] (very easy) Put reference handle on the GNC point clouds sent out.
- [ ] [Alejandro] (easy) Stream out the trajectory data.
- [x] [Alejandro] (easy)  Transform the trajectories before sampling them in the MavSender.
- [ ] [Alejandro] (medium) Fully setup HITL (with the RPI, FC and simulation).
- [ ] [Alejandro] (hard??) Fix errors that happen in the RPI when we run the tests (unit and integration).

- [necessary still? NO] Abort mission if VIO goes crazy.