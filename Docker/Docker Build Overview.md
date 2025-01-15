---
tags:
  - "#docker"
---
---
**Docker Build** implements a client-server architecture:

- Client: **Buildx** is the user interface for running and managing builds.
- Server: **BuildKit** handles the build execution

 When you invoke a build (e.g. `docker build <your-options>`), the **Buildx** client sends a build request to the **BuildKit** backend. **BuildKit** resolves the build instructions and executes the build steps. The build output is either sent back to the client or uploaded to a registry (e.g. DockerHub).

## BuildKit

**BuildKit** is a deamon process that executes the build workloads

The build request includes:

- The *Dockerfile*
- Build arguments
- Export options
- Caching options

While **BuildKit** is executing the build, **Buildx** monitors the build status and prints the progress to the terminal. 

If the build requires resources from the client, such as local files or build secrets, **BuildKit** requests the resources that it needs from **Buildx** (only when they are needed). Examples of resources that **BuildKit** can request from **Buildx** include:

- Local filesystem build contexts
- Build secrets
- SSH sockets
- Registry authentication tokens
