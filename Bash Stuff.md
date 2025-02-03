```bash
(timeout 2s zenoh --mode=client --connect="tcp/localhost:7447" subscribe -k drone/gnc/heartbeat > /tmp/heartbeat_buffer); cat /tmp/heartbeat_buffer | grep -i DEAE=
```


```bash
# Find the ID of a given process
pgrep my_process
```