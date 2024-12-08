## Useful flags

### Building

To not use the remote cache
```
--noremote_accept_cached
```

To not discard the sandbox environment used to build
```
--sandbox_debug
```

To relax the constraints on the sandbox
```
--spawn_strategy=
```

### Testing

To force the tests to run
```
--nocache_test_results
```

To run the test repeatedly
```
--runs_per_test=`<number-of-repetitions>`
```

To tell how many tests can run in parallel (set to 1 for serial execution)
```
--local_test_jobs=`<number-of-parallel-test>`
```

To run a specific test from a test suite
```
--test_filter=TestSuite.TestName
```

To run multiple tests from a test suite
```
--test_filter=TestSuite.TestName1:TestSuite.TestName2
```

To debug memory related issues with address sanitizer
```
# In your target definition (i.e. BUILD.bazel)

cc_library(
    name = "foo",
    srcs = ["src/foo.cpp"],
    hdrs = ["include/foo.hpp"],
	
	...

	# Flags to the compiler to add the address sanitizer stuff
    copts = ["-fsanitize=address", "-fno-omit-frame-pointer"],
    linkopts = ["-fsanitize=address"],
)
```