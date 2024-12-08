## Tests that don't work
Max parallelism
```
bazel test //test/integration/trajectory_swapping_logic:trajectory_swapping_wrapper_test \
--runs_per_test=100 \
--nocache_test_results \
--test_filter=*.MixedRequests
```

No parallel tests
```
bazel test //test/integration/trajectory_swapping_logic:trajectory_swapping_wrapper_test \
--runs_per_test=100 \
--local_test_jobs=1 \
--nocache_test_results \
--test_filter=*.MixedRequests
```


## Tests that work
```
bazel test //test/integration/trajectory_swapping_logic:trajectory_swapping_wrapper_test \
--runs_per_test=100 \
--nocache_test_results \
--test_filter=*.SingleNormalRequest:*.SingleNormalRequestEarlyTermination:*.SingleCriticalRequest:*.SingleCriticalRequestEarlyTermination:*.SingleCriticalRequestExtremelyEarlyTermination:*.MultipleNormalRequest:*.MultipleSlowCriticalRequest:*.MultipleFastCriticalRequest:*.PostponeNormalRequests
```

## TODOs

- [ ] LocalMap inside wrapper to get the latest in case of postponed request
- [ ] Current trajectory inside wrapper to get the latest
- [ ] Update Goal based on current trajectory info