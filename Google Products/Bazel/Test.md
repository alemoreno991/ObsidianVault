
In order to run just one specific test with Google Test Framework through Bazel:
`bazel test //path/to:your_test_target --test_filter=YourTestSuite.YourTestName`