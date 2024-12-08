## x86_64 

Please refer to the [documentation](https://pcl.readthedocs.io/projects/tutorials/en/master/compiling_pcl_docker.html#docker-container-configuration) in case of any doubts.
```shell
cd /some/path/you/want
# Clone the repository
git clone https://github.com/PointCloudLibrary/pcl.git --branch pcl-1.14.1 --depth 1
# Pull the docker image where we'll build PCL from source
docker pull pointcloudlibrary/env:22.04
# Run the container
docker run --user $(id -u):$(id -g) -v $PWD/pcl:/home -w /home -e CC=/usr/bin/clang -e CXX=/usr/bin/clang++ --rm -it pointcloudlibrary/env:22.04 bash
```


Inside the container

```shell
cmake -Bbuild -H. && cmake --build build -j4
```

- [x] /usr/include  (look for headers here)
- [x] /usr/lib/x86_64-linux-gnu/libflann_cpp.so
- [x] /usr/lib/x86_64-linux-gnu/libusb-1.0.so
- [x] /usr/include/ni (headers for NI)
- [x] /usr/lib/libOpenNI.so (what does libusb::libusb mean?)
- [x] /usr/include/openni2 (headers for NI2)
- [x] /usr/lib/x86_64-linux-gnu/libOpenNI2.so
- [x] /opt/ensenso/development/c/include (headers of Ensenso)
- [x] /usr/lib/libNxLib64.so
- [ ] /usr/local/include (look for header of realsense2)
- [ ] /usr/lib/x86_64-linux-gnu/libz.so
- [ ] /usr/lib/x86_64-linux-gnu/libpng.so
- [ ] /usr/lib/x86_64-linux-gnu/libGLEW.so
- [ ] /usr/lib/x86_64-linux-gnu/libOpenGL.so
- [ ] /usr/lib/x86_64-linux-gnu/libmpi.so
- [ ] /usr/lib/x86_64-linux-gnu/hdf5/serial/libhdf5.so
- [ ] /usr/lib/x86_64-linux-gnu/libcrypto.so
- [ ] /usr/lib/x86_64-linux-gnu/libcurl.so
- [ ] /usr/lib/x86_64-linux-gnu/libpthread.a
- [ ] /usr/lib/x86_64-linux-gnu/libsz.so
- [ ] /usr/lib/x86_64-linux-gnu/libz.so
- [ ] /usr/lib/x86_64-linux-gnu/libdl.a
- [ ] /usr/lib/x86_64-linux-gnu/libm.so
- [ ] /usr/include (look for NetCDF)
- [ ] /usr/lib/x86_64-linux-gnu/libogg.so
- [ ] /usr/lib/x86_64-linux-gnu/libtheora.so
- [ ] /usr/lib/x86_64-linux-gnu/libjsoncpp.so
- [ ] /usr/lib/x86_64-linux-gnu/libxml2.so
- [ ] /usr/lib/x86_64-linux-gnu/libgl2ps.so
- [ ] /usr/include (look for SQLite3)
- [ ] /usr/lib/x86_64-linux-gnu/libproj.so
- [ ] /usr/include/eigen3
- [ ] /usr/include (look for X11)
- [ ] /usr/lib/x86_64-linux-gnu/libX11.so
- [ ] /usr/lib/x86_64-linux-gnu/libXext.so
- [ ] /usr/lib/x86_64-linux-gnu/libexpat.so
- [ ] /usr/lib/x86_64-linux-gnu/libdouble-conversion.so
- [ ] /usr/lib/x86_64-linux-gnu/liblz4.so
- [ ] /usr/lib/x86_64-linux-gnu/liblzma.so
- [ ] /usr/lib/x86_64-linux-gnu/libjpeg.so
- [ ] /usr/lib/x86_64-linux-gnu/libtiff.so
- [ ] /usr/lib/x86_64-linux-gnu/libfreetype.so
- [ ] /usr/include/utf8cpp

- [ ] VTK
- [ ] QHull
- [ ] Qt
- [ ] /usr/lib/x86_64-linux-gnu/libpcap.so

- [ ] /usr/lib/x86_64-linux-gnu/cmake/Boost-1.74.0/BoostConfig.cmake (found suitable version "1.74.0", minimum required is "1.65.0") found components: filesystem iostreams system

```shell
PLC_ROOT=~/Workspace/pcl-packaged
CONTAINER_ID=
mkdir -p ${PLC_ROOT}/usr/include
docker cp ${CONTAINER_ID}:/usr/include/flann ${PLC_ROOT}/usr/include
mkdir -p ${PLC_ROOT}/usr/lib/flann
docker cp ${CONTAINER_ID}:/usr/lib/x86_64-linux-gnu/libflann* ${PLC_ROOT}/usr/lib/flann

```
## aarch64 

Please refer to the [documentation](https://pcl.readthedocs.io/projects/tutorials/en/master/compiling_pcl_docker.html#docker-container-configuration) in case of any doubts.
```shell
cd /some/path/you/want
# Clone the repository
git clone https://github.com/PointCloudLibrary/pcl.git --branch pcl-1.14.1 --depth 1
# Pull the docker image where we'll build PCL from source
docker pull pointcloudlibrary/env:22.04
# Run the container
docker run --user $(id -u):$(id -g) -v $PWD/pcl:/home -w /home -e CC=/usr/bin/clang -e CXX=/usr/bin/clang++ --rm -it pointcloudlibrary/env:22.04 bash
```


Inside the container

```shell
cmake -Bbuild -H. && cmake --build build -j4
```