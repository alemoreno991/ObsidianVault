# Bazel 101

**EXT_BUILD_DEPS** is where bazel stores the dependencies that we list in the BUILD.bazel file with the `deps` parameter.

**CMAKE_PREFIX_PATH** is the flag that allows one to tell CMake where a `<PackageName>Config.cmake` or `Find<PackageName>.cmake` file is. Note that this file is used by [find_package](https://cmake.org/cmake/help/latest/command/find_package.html) to find a dependency. 

**CMAKE_PREFIX_PATH** is the flag that allows one to also tell CMake where to find dependencies in other cases. For example,  [pkg_check_modules](https://cmake.org/cmake/help/latest/module/FindPkgConfig.html) will use **CMAKE_PREFIX_PATH** to look for the dependency there.

## Handling third party dependencies

Therefore, in Bazel we can use the [cmake rule](https://github.com/bazel-contrib/rules_foreign_cc/blob/main/docs/README.md#cmake) to build a third party dependency. Let's assume this dependecy has another dependency, something like this:

> 							dep_A <- dep_B

Then, you can build dep_B with cmake rule and then build the second as follows:

```bazel
cmake(
    name = "dep_A",
    cache_entries = {
        "CMAKE_PREFIX_PATH": "$EXT_BUILD_DEPS/<path/to/Find<PackageName>.cmake",
    },
    lib_source = "@dep_A//:all_srcs",
    out_shared_libs = [
        "libdep_A.so",
    ],
    out_static_libs = [
        "libdep_A.a",
    ],
    deps = [
        "@@//third_party/dep_B",
    ],
)
```

To actually find the `<path/to/Find<PackageName>.cmake`:

1. `bazel build --sandbox_debug //third_party/dep_A/...`
2. In the logs, look for the following 

```shell
_____ BEGIN BUILD LOGS _____

Bazel external C/C++ Rules. Building library fcl

Environment:______________
BUILD_SCRIPT=bazel-out/k8-fastbuild/bin/third_party/fcl/fcl_foreign_cc/build_script.sh
EXT_BUILD_ROOT=/tmp/bazel-local-cache/eb271057bb1e47b445844703ace459a1/sandbox/processwrapper-sandbox/4/execroot/_main
BUILD_LOG=bazel-out/k8-fastbuild/bin/third_party/fcl/fcl_foreign_cc/CMake.log
PWD=/tmp/bazel-local-cache/eb271057bb1e47b445844703ace459a1/sandbox/processwrapper-sandbox/4/execroot/_main
BUILD_WRAPPER_SCRIPT=bazel-out/k8-fastbuild/bin/third_party/fcl/fcl_foreign_cc/wrapper_build_script.sh
TMPDIR=/tmp
EXT_BUILD_DEPS=/tmp/bazel-local-cache/eb271057bb1e47b445844703ace459a1/sandbox/processwrapper-sandbox/4/execroot/_main/bazel-out/k8-fastbuild/bin/third_party/fcl/fcl.ext_build_deps
BUILD_TMPDIR=/tmp/bazel-local-cache/eb271057bb1e47b445844703ace459a1/sandbox/processwrapper-sandbox/4/execroot/_main/bazel-out/k8-fastbuild/bin/third_party/fcl/fcl.build_tmpdir
SHLVL=2
ZERO_AR_DATE=1
BAZEL_CXXOPTS=-std=c++20
LD_LIBRARY_PATH=/usr/local/lib
INSTALLDIR=/tmp/bazel-local-cache/eb271057bb1e47b445844703ace459a1/sandbox/processwrapper-sandbox/4/execroot/_main/bazel-out/k8-fastbuild/bin/third_party/fcl/fcl
PATH=/tmp/bazel-local-cache/eb271057bb1e47b445844703ace459a1/sandbox/processwrapper-sandbox/4/execroot/_main:/root/.cache/bazelisk/downloads/sha256/80ccd1ecb4b88750fbe5d7622d67072fddcba9da7808f13356555e480bf67875/bin:/root/.cargo/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
_=/usr/bin/env
```

Note the following:

- **EXT_BUILD_ROOT**: the directory where the dependency is being built (this is how Bazel achieves the sandboxing. By copying there everything it needs to build the target)
- **EXT_BUILD_DEPS**: the directory where the dependencies of the target are copied. In our example bazel would copy the build artifacts of dep_B here.

3. Identify the **EXT_BUILD_DEPS** directory and navigate there. (i.e `cd $EXT_BUILD_DEPS`)
4. In the **EXT_BUILD_DEPS** directory look for the dependency (i.e. dep_B) and then look for the `<PackageName>Config.cmake` or `Find<PackageName>.cmake`. Concatenate the path and assign it to **CMAKE_PREFIX_PATH** in the `cache_entries` parameter of the cmake rule. It would look something like this:
```bazel
cmake(
...
	cache_entries = {
		"CMAKE_PREFIX_PATH": "$EXT_BUILD_DEPS/<identified/path/to/file",	
	},
...
)
```

### Special case: BOOST

In the case of boost need to do steps 1, 2 and 3 of the general way of handling third party dependencies section. But step 4 differs.

4. In the **EXT_BUILD_DEPS** directory look for the boost dependency  and then look for the for the `include` and `lib` directories. Concatenate the path and assign it as follows:
```bazel
# Assumming that BOOST version 1.85.0 is being used

cmake(
...
	cache_entries = {
        "Boost_INCLUDE_DIR": "$EXT_BUILD_DEPS/boost/include",
        "Boost_LIBRARY_DIRS": "$EXT_BUILD_DEPS/boost/lib",
        "Boost_VERSION_STRING": "1.85.0",
        "Boost_LIB_VERSION": "1.85.0",
	},
...
)
```
# Debugging  101

When you add  the flag`-s` you can see a more detailed log of what Bazel is doing. For example, you will see something like this whenever a new target is started: 
```
SUBCOMMAND: # //third_party/<dependency-name>:<dependency-name> [action 'Foreign Cc - CMake: Building <dependency-name>', configuration: 0429927d459275d3655a4ccfc56e17bccb70da2a3cb5dd0e662b0bc864ea31f8, execution platform: @@platforms//host:host, mnemonic: CcCmakeMakeRule]
(cd /tmp/bazel-local-cache/eb271057bb1e47b445844703ace459a1/execroot/_main && \
  exec env - \
    BAZEL_CXXOPTS='-std=c++20' \
    LD_LIBRARY_PATH=/usr/local/lib \
    PATH=/root/.cache/bazelisk/downloads/sha256/80ccd1ecb4b88750fbe5d7622d67072fddcba9da7808f13356555e480bf67875/bin:/root/.cargo/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin \
  /bin/bash -c bazel-out/k8-fastbuild/bin/third_party/<dependency-name>/<dependency_name>_foreign_cc/wrapper_build_script.sh)
```
Note that the first thing that Bazel does is to change to a directory where the build will happen. So, you can inspect that directory if you need to.