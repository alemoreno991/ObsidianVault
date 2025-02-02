---
tags:
  - nixos
---
## Configure your Nvidia GPU in NixOS

### Allow unfree software

First of all you need to allow [unfree software](https://nixos.wiki/wiki/Unfree_Software) to be installed in your system. 

> [!Note]
> This is because Nvidia drivers are proprietary. E.g. the NVIDIA packages include `nvidia-x11`, `nvidia-settings` and `nvidia-persistenced`

```nix
  # Add this in your `configuration.nix`
  nixpkgs.config.allowUnfree = true;
```

### Enable the Nvidia drivers

To enable the Nvidia drivers follow the [NixOS documentation](https://nixos.wiki/wiki/Nvidia). The following nix module represents the final result that one should import to their `configuration.nix`

```nix
# Create `nvidia.nix` file for this configuration
{ config, lib, pkgs, ... }:
{

  # Enable OpenGL
  hardware.graphics = {
    enable = true;
  };

  # Load nvidia driver for Xorg and Wayland
  services.xserver.videoDrivers = ["nvidia"];

  hardware.nvidia = {

    # Modesetting is required.
    modesetting.enable = true;

    # Nvidia power management. Experimental, and can cause sleep/suspend to fail.
    # Enable this if you have graphical corruption issues or application crashes after waking
    # up from sleep. This fixes it by saving the entire VRAM memory to /tmp/ instead 
    # of just the bare essentials.
    powerManagement.enable = false;

    # Fine-grained power management. Turns off GPU when not in use.
    # Experimental and only works on modern Nvidia GPUs (Turing or newer).
    powerManagement.finegrained = false;

    # Use the NVidia open source kernel module (not to be confused with the
    # independent third-party "nouveau" open source driver).
    # Support is limited to the Turing and later architectures. Full list of 
    # supported GPUs is at: 
    # https://github.com/NVIDIA/open-gpu-kernel-modules#compatible-gpus 
    # Only available from driver 515.43.04+
    # Currently alpha-quality/buggy, so false is currently the recommended setting.
    open = false;

    # Enable the Nvidia settings menu,
	# accessible via `nvidia-settings`.
    nvidiaSettings = true;

    # Optionally, you may need to select the appropriate driver version for your specific GPU.
    package = config.boot.kernelPackages.nvidiaPackages.stable;
  };
  
	prime = {
	    sync.enable = true;
		
		# Make sure to use the correct Bus ID values for your system!
		intelBusId = "PCI:0:2:0";
		nvidiaBusId = "PCI:14:0:0";
        # amdgpuBusId = "PCI:54:0:0"; For AMD GPU
	};
}

```

## Testing 

### Trivial 

```bash
# Run the following command and observe that 
nvidia-smi
```

![[Pasted image 20250126171028.png]]
### Stress test

```bash
nix-shell -p glmark2
```

Inside the nix shell:

```
glmark2
```

![[Pasted image 20250126171525.png]]