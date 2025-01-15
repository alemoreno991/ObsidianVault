---
tags:
  - "#docker"
---
---
It all starts with a *Dockerfile*. 

Docker build images by reading the instructions from a *Dockerfile*. 

## Most Common instructions

| Instruction         | Description                                                                                                                                                                                             |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `FROM <image>`      | Defines a base for your image                                                                                                                                                                           |
| `RUN <cmd>`         | Executes any comands in a new layer on top of the current image and commits the result. `RUN` also has a shell form for running commands                                                                |
| `WORKDIR <dir>`     | Sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY` and `ADD` instruction that follow it in the Dockefile                                                                             |
| `COPY <src> <dest>` | Copies new files or directories from `<src>` and adds them to the filesystem of the container at the path `<dest>`.                                                                                     |
| `CMD <cmd>`         | Lets you define the default program that is run once you start the container based on this image. Each Dockefile only has one `CMD`, and only the last `CMD` instance is respected when multiple exist. |
For a more comprehensive list of instructions refer to [[Dockerfile Instructions]]
## Docker Images

Docker images consist of layers. Each layer is the result of a build instruction in the Dockerfile. Layers are stacked sequentially, and each one is a delta representing the changes applied to the previous layer.



