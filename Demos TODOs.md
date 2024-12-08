- [ ] Local map:
	- [ ] [Chao-Wei] Emphasis on usability (not on performance)
	- [ ] [Chris] Mock the ingestion of point clouds for SITL
- [ ] Re-architecting codebase:
	- [ ] [Ale] Trajectory Swapper into singleton
	- [x] [Chris] Reference frame transformation
- [ ] [Tomas] Coordinate with BPRT on how to get the goals:
	- In GPS-enabled mission -> GPS coordinate frame
	- In GPS-denied mission -> Local reference frame
- [ ] [Tomas/Ale] Manual Flight Mode:
	- Make sure that it's possible to switch to manual mode at any time and keep streaming data out.
- [ ] [?] Initialization maneuver:
	- Hardcoded maneuver to be executed right after takeoff
- [ ] [Ale] Cross-compiling

- [ ] [Teo] Streaming data out (sensors)



## Thoughts
- [ ] Path planner should run for as little time as possible
- [ ] Visualization of trajectories
- [ ] Meaningful SITL test
- [ ] Benchmark performance
- [ ] LocalMap updates into its own thread.
- [ ] Cross-compilation