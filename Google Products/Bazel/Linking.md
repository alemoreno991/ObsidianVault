
To examine what shared libraries your shared library or binary depends on.

```shell
# This command allows you to see the runtime library path of your shared library or binary (sometimes refered to as 'rpath')
chrpath -l libexample.so
```

You can also change the `rpath` as follows:

```shell
# Change the rpath of your shared library or binary
chrpath -r /your/new/path libexample.so
```

# What did I do?
I was having problems with the binary that Bazel was generating. I need to properly fix this. But for now I found a way around it by patching the rpath of the binary itself.

```shell
# Build the binary in question with Bazel
bazel build //test/unit/path_planner:all --sandbox_debug -s --noremote_accept_cached --config=x86
# Find the location of the binary and assign it to an environment variable
MY_EXECUTABLE=<location/of/your/binary>
# Assign the known location of the shared library
SO_PATH=/tmp/extdep/lib # This is where I put mine

# Append the `SO_PATH` to the current RPATH of the binary
CURRENT_RPATH=$(patchelf --print-rpath $MY_EXECUTABLE)
patchefl --set-rpath "$CURRENT_RPATH:$RPATH" $MY_EXECUTABLE 
```
