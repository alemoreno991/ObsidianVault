---
tags:
  - nixos
  - system76
---
## How-to

For hardware support, this line needs to be added to your `/etc/nixos/configuration.nix` file then rebuild the OS:

```
# System76
hardware.system76.enableAll = true;
```

```
sudo nixos-rebuild switch
```

## Known issues

### Power-profile-deamon

> [!warning]
> If your system has power-profiles-daemon installed (done by default on GNOME), you'll need to disable it for system76-power to start. 
> 
 
 Add this line to your `/etc/nixos/configuration.nix` file then rebuild the OS:

```
services.power-profiles-daemon.enable = false;
```

```
sudo nixos-rebuild switch
```
## References

[System76 documentation for other distros](https://support.system76.com/articles/system76-software/)
