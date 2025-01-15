### DONE

- [x] Solved some weird issues related to the fact that we upgraded some Bazel Packages & Bazel itself.
- [x] The PR that Tomas worked on (allowing connection between RPI and FC with only one physical link) was merged. 
- [x] Transform the trajectory to PX4 world before sending it to the FC
- [x] Included the reference handle in the point cloud message sent out for visualization.
- [x] Tested the zenoh configuration in the hardware.
- [x] Improve the speed of CI 
- [x] Created a pre-release of GNC with these changes (we are testing this so that we can properly release it).
### Reviewing PR

- [ ] [Pierric] Inject a configuration for the mavlink port

- [x] [Chris] Solve some of the problems that the trajectory swapper is having (twick some parameter: interpolation coefficient, arc length tolerance).
- [x] [Chris] Now our SITL tests check if the drone reached a location at a certain point in time.
- [x] [Chris] Introduce an obstacle along the current trajectory and see the collision warden replan accordingly. 

- [ ] [Chao-Wei] End-to-end test of the LocalMap service.

### WIP

- [ ] [Alejandro] Ingest External Vision into PX4.
- [ ] [Alejandro] (medium) Fully setup HITL (with the RPI, FC and simulation).

---
## TODOs

- [ ] [Chao-Wei] (medium) End-to-end test of the LocalMap service in the RPI

- [ ] [Chris] (medium) Coordinate with BPRT on the goal messages (GPS or Local reference frame).

- [ ] [Alejandro] (easy) Stream out the trajectory data.
- [ ] [Alejandro] (hard??) Fix errors that happen in the RPI when we run the tests (unit and integration).

- [ ] [Alejandro] Import the binaries into bazel and try cross-compilation.

- VIO initialization maneuver