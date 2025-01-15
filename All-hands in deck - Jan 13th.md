## DONE

- [x] SSD disks are installed in the drones
- [x]  Deployment script for `GNC v1.5.0` that allows to configure
	- zenoh 
	- mavlink
- [x] Fully setup HITL (with the RPI, FC and simulation).
## Reviewing PR

- [ ] Ingestion of goal from BPRT and handling them in GNC
- [ ] End-to-end test of the LocalMap service 
	- automatic deployment through docker compose
## WIP

- [ ] [Alejandro] Ingest External Vision into PX4.
- [ ] [Alejandro] Closing MavLink ports when sigkill.

---
## TODOs

- [ ] [Chao-Wei] (medium) End-to-end test of the LocalMap service in the RPI

- [ ] [Alejandro] (easy) Stream out the trajectory data.
- [ ] [Alejandro] (hard??) Fix errors that happen in the RPI when we run the tests (unit and integration).

- [ ] [Alejandro] Import the binaries into bazel and try cross-compilation.