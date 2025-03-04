Tags: #bazel #build-system #toolchain #devops

---
## Sysroot characteristics
### Laptop Characteristics

Kernel version: `5.15.0-25-generic`
```shell
# Command to find the kernel version
uname -r
```

glibc version: `stable release 2.35`
```shell
# Command to find the glibc version
/lib/x86_64-linux-gnu/libc.so.6
```

stdlib version: `GCC_4.2.0` (backwards compatible with `GCC_3.4`, `GCC_3.3`, `GCC_3.0`)
```shell
# Command to find the stdlib version
strings /lib/x86_64-linux-gnu/libstdc++.so.6 | grep 'GCC'
```

### RaspberryPI-5 Characteristics

Kernel version: `6.6.51+rpt-rpi-2712`
```shell
# Command to find the kernel version
uname -r
```

glibc version: `stable release 2.35`
```shell
# Command to find the glibc version
/lib/aarch64-linux-gnu/libc.so.6
```

stdlib version: `GCC_4.3.0` (backwards compatible with `GCC_4.2.0`, `GCC_3.4`, `GCC_3.3`, `GCC_3.0`)
```shell
# Command to find the stdlib version
strings /lib/aarch64-linux-gnu/libstdc++.so.6 | grep 'GCC'
```

## How to build the sysroot
### Automated steps

#### For x86_64
```shell
cd your-path-to-gnc-c-dam-src/docker/sysroot-build
./build.sh x86_64 . base  # to compile the sysroot for x86_64
```
#### For aarch64
> [!Warning]
>The kernel is not properly built yet. The `6.6.51+rpt-rpi-2712` kernel should be used I think so that it coincides with the actual kernel of the RPI5

```shell
cd your-path-to-gnc-c-dam-src/docker/sysroot-build
./build.sh aarch64 . base  # to compile the sysroot for aarch64
```

### Manual steps

1. Untar the tarball generated by the build step described above into `/tmp/sysroot-base-x86` and `/tmp/sysroot-base-aarch64` correspondingly.
2. Add an empty `REPO.bazel` file
3. Add a `BUILD.bazel` file with the following content:
```starlark
filegroup(
    name = "sysroot",
    srcs = glob(["**"]),
    visibility = ["//visibility:public"]
)
```

4. Manually copy the `lz4` library from my host (x86_64) to the `/tmp/sysroot-base-x86`
```shell
cp /usr/include/lz4*.h ~/Workspace/sysroot-base-x86_64/usr/include/
cp /lib/x86_64-linux-gnu/liblz4* ~/Workspace/sysroot-base-x86_64/usr/lib
```

> [!NOTE]
> Do the same thing for aarch (manually in the RPI)
> 

5. Run the `dev-container` (don't forget to mount the directories where the sysroot is located).

## Third party dependencies not building (x86_64)

- [ ] FCL
- [ ] OMPL (because of FCL dependency afaik)
- [x] PCL

## Third party dependencies not building (aarch64)

- [ ] FCL 
- [ ] OMPL (because of FCL dependency afaik)
- [x] PCL

# References
- [Sysroot package generation for use with toolchains_llvm](https://steven.casagrande.io/posts/2024/sysroot-generation-toolchains-llvm/)
- [llvm 18.1.7](https://github.com/llvm/llvm-project/releases/tag/llvmorg-18.1.7)
- [linux kernel 5.15](https://github.com/torvalds/linux/releases/tag/v5.15)
- [rpi kernel](https://www.raspberrypi.com/documentation/computers/linux_kernel.html)
- [gcc-11.4.0](https://ftp.gnu.org/gnu/gcc/gcc-11.4.0/)
- [lz4](https://github.com/lz4/lz4)

