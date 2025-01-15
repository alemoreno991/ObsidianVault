- [ ] **SLAM** could need to restart its service at any point in time. Which means that **Perception** and **PoseGraphAPI** will go nuts. Thus, the `LocalMap` and `Transformations` will be wrong (by a lot.) (@Alejandro)
	- The SOLUTION: to implement an `abort` topic and subscribe to it from GNC. In the event of seeing this abort message come in we just enter HOLD mode or LAND or something.
- [x] Transform the trajectories before sampling them in the MavSender. (@Alejandro)
- [ ] HITL not working (@Alejandro)
- [x] TrajectorySwapping 10% of the cases were failing (@Chris, out-of-office)
- [ ] LocalMap (@Chao-wei)
	- Testing the PoseGraphInterface 
	- Get to the point where we can visualize (again) the LocalMap as PointCloud with all the necessary services running.
- [x] The LocalMapAsPointCloud is not being published to Zenoh AFIK.
	- Everything is in place but we have not added the `publish` function to the middleware `_Publish()` method. 
- [x] Inject the Zenoh config from outside (with a file).


## PX4-EKF steps

- [ ] Figure out the time offset of the External Visual measurement and the corresponding IMU measurement.
- [ ] Subscribe on a topic that would provide the VIO measurements. Propagate to the MavSender.
- [ ] Which message should we send? Pose+Velocity or just Pose?
- [ ] Should we use a real covariance of the VIO estimate or set a fixed Variance to be used instead.