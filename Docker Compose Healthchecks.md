---
tags:
  - "#docker-compose"
  - "#devops"
---
## Overview

The `healthcheck` attribute is used to run a command inside the container that will determine if the service is executing correctly or not (healthy). 

> [!Note]
It works the same way as the `HEALTHCHECK` defined in Docker. It will override the healthcheck defined in the Dockerfile (if any).

When a container has a `healthcheck` specified its status will initially be set to `starting`. After passing the checks it will become `healthy`. If the check is failed a certain number of consecutive times it will become `unhealthy`.

## Usage

```docker-compose
healthcheck:
  test: ["CMD-SHELL", "curl -f http://localhost || exit 1"]
  interval: 1m30s
  timeout: 10s
  retries: 3
  start_period: 40s
  start_interval: 5s
```

- `interval`: defines the time delay after which the healthcheck will be first run. It also defines the time delay after which the healthcheck will be run after the previous check completes.
- `timeout`: defines the amount of time that a single run of a check can take. After that the check is considered to have failed. (useful if your checks might take a long time in some cases).
- `start period`: provides initialization time for containers that need time to bootstrap. A failed check during this time period is not considered towards the maximum number of retries.
- `start interval`: is the time between health checks during the start period.
- `retries`: number of times the health check will be attempted before considering the container `unhealthy`.
- `test`: defines the command to be run to check the container's health. The expected *healthy value=0* and the *unhealthy value=1*
	- `CMD-SHELL`: will use the container's default shell (`/bin/sh` for Linux) 

## References

- [Docker Compose user reference](https://docs.docker.com/reference/compose-file/services)
- [Docker user reference](https://docs.docker.com/reference/dockerfile/#healthcheck)
