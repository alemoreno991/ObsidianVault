# Frontend

https://github.com/asus-robotics-and-ai-center/flmg-dam-c-src/pkgs/container/visualizer

# How-to-Install

1. Go to [Github -> Settings -> Developer Settings -> Personal Access Token](https://github.com/settings/tokens)
2. Create a classic token and select the `read:packages` scope
3. Open a terminal and give yourself access to pull the container from the ARAIC hub
```shell
export CR_PAT=<your-newly-created-access-code-token>
echo $CR_PAT | docker login ghcr.io -u <your-github-username> --password-stdin
```
4. Pull the docker image for the visualizer frontend
```shell
docker pull ghcr.io/asus-robotics-and-ai-center/visualizer:latest
```

# Backend

1. Get the `docker-compose.yaml` 
2. `docker-compose up`
3. Build the **Protobuf** with version 21.12

```shell
cd ~/Downloads
# Download Protobuf 3.21.12
curl -L --fail https://github.com/protocolbuffers/protobuf/releases/download/v21.12/protobuf-cpp-3.21.12.tar.gz -o - | tar -xvf -
cd protobuf-cpp-3.21.12
# Install protobuf in your system
./configure
make -j$(nproc) # $(nproc) ensures it uses all cores for compilation
make check
sudo make install
sudo ldconfig # refresh shared library cache.
```

5. Build **Zenoh**

```shell

```

6. Build the example with *cmake*
