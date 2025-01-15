## Overview

This tool allows you to attach to an existing zenoh network for debugging purposes.

- Listen to topics
- Publish to topics
- Visualize the network
## Install the tool

```
pip install zenoh-cli=0.5.2
```

## Subscribe to a topic

Get the help as follows:
```
zenoh subscribe -h
```

Usage example:
```
zenoh --mode=client --connect="tcp/localhost:7447" subscribe -k <your-zenoh-topic>
```

## Publish to a topic

Get the help as follows:
```
zenoh put -h
```

Usage example:
```
zenoh --mode=client --connect="tcp/localhost:7447" put -k <your-topic-name> -v "your-message-goes-here"
```

## Other useful usage

### Visualize network

```
zenoh --mode=client --connect="tcp/localhost:7447" network
```

### Get info

```
zenoh --mode=client --connect="tcp/localhost:7447" info
```
