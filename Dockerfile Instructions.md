---
tags:
  - "#docker"
  - "#cheetsheet"
---
---
## References 

[Dockerfile reference](https://docs.docker.com/reference/dockerfile/)

## Often used instructions

| Instruction              | Description                                                                                                                                                                                                                   |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `FROM <image>`           | Defines a base for your image                                                                                                                                                                                                 |
| `RUN <cmd>`              | Executes any comands in a new layer on top of the current image and commits the result. `RUN` also has a shell form for running commands                                                                                      |
| `WORKDIR <dir>`          | Sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY` and `ADD` instruction that follow it in the Dockefile                                                                                                   |
| `COPY <src> <dest>`      | Copies new files or directories from `<src>` and adds them to the filesystem of the container at the path `<dest>`.                                                                                                           |
| `CMD <cmd>`              | Lets you define the default program that is run once you start the container based on this image. Each Dockefile only has one `CMD`, and only the last `CMD` instance is respected when multiple exist.                       |
| `ARG <arg>`              | Allows one to pass arguments to the Dockerfile at build-time                                                                                                                                                                  |
| `ENV <key>=<value>`      | Set environment variable for the image                                                                                                                                                                                        |
| `SHELL `                 | Set the default shell of an image. The default shell on Linux is `["/bin/sh", "-c"]`.                                                                                                                                         |
| `USER <user>:[:<group>]` | The `USER` instruction sets the user name (or UID) and optionally the user group (or GID) to use as the default user and group for the remainder of the current stage.                                                        |
| `EXPOSE <port>`          | The `EXPOSE` instruction informs Docker that the container listens on the specified network ports at runtime. You can specify whether the port listens on TCP or UDP, and the default is TCP if you don't specify a protocol. |
## Not yet explored instructions

- `ENTRYPOINT`
- `HEALTHCHECK`
- `ONBUILD`
- `STOPSIGNAL`
- `LABEL`
- `MAINTAINER`

## Instructions I avoid

- `VOLUME`: I think volumes should be defined through the **cli** or **docker-compose.yaml** files.