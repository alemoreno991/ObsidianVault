---
tags:
  - "#nix"
---
## Introduction

> [!Note] 
> Flakes is an experimental feature that improves **reproducibility**. However, it's been around for several years now. 
> **SO USE IT**! 

There is a `flake.nix` file where one can describe dependencies and a `flake.lock` file to lock the versions of the dependencies, ensuring project reproducibility.

## Enable NixOS with Flakes

> [!Tip]
> Use **Flakes** to manage system configurations.

Flakes is not enabled by default. We need to manually modify the `/etc/nixos/configuration.nix` file to enable Flakes feature and the accompanying new nix command-line tool:

```nix
# /etc/nixos/configuration.nix
{ config, pkgs, ... }:

{
  imports = [
    # Include the results of the hardware scan.
    ./hardware-configuration.nix
  ];

  # ......

  # Enable the Flakes feature and the accompanying new nix command-line tool
  nix.settings.experimental-features = [ "nix-command" "flakes" ];
  environment.systemPackages = with pkgs; [
    # Flakes clones its dependencies through the git command,
    # so git must be installed first
    git
    vim
    wget
  ];
  # Set the default editor to vim
  environment.variables.EDITOR = "vim";

  # ......
}
```

## Structure of Flake

```nix
# /etc/nixos/flake.nix
{
  inputs = {
    # NixOS official package source, using the nixos-24.11 branch here
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-24.11";
  };

  outputs = { self, nixpkgs, ... }@inputs: {
    # Omitting previous configurations......
  };
}
```

The `inputs` attribute set defines all the dependencies of this flake. These dependencies will be passed as arguments to the `outputs` function after they are fetched.

Dependencies in `inputs` have many types and definitions:
- Another Flake
- Git repository
- Local path
Here, we only define a dependency named `nixpkgs` (after `nixpkgs` is defined in the `inputs` it can be used in the parameters of the subsequent `output` function).

The `outputs` is a function that takes the dependencies from `inputs` as its parameters and it returns an attribute set, which represents the build results of the Flake.

```nix
{
  description = "A simple NixOS flake";

  inputs = {
    # NixOS official package source, here using the nixos-24.11 branch
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-24.11";
  };

  outputs = { self, nixpkgs, ... }@inputs: {
    # The host with the hostname `my-nixos` will use this configuration
    nixosConfigurations.my-nixos = nixpkgs.lib.nixosSystem {
      system = "x86_64-linux";
      modules = [
        ./configuration.nix
      ];
    };
  };
}
```

Flakes can have various purposes and can have different types of outputs. Here, we are only using the `nixosConfigurations` output type, which is used to configure NixOS systems.

### Depending on other Flakes

By default, a Flake searches for a `flake.nix` file in the root directory of each of its dependencies (i.e., each item in `inputs`) and lazily evaluates their outputs functions (lazy evaluation means that a Flake's output function is only evaluated when it's actually needed). It then passes the attribute set returned by these functions as arguments to its own outputs function, enabling us to use the features provided by the other flakes within our current flake.

From the source code of the Nixpkgs repository, we can see that its flake outputs definition includes the `lib` attribute, and in our example, we use the `lib` attribute's `nixosSystem` function to configure our NixOS system.

The attribute set following `nixpkgs.lib.nixosSystem` is the function's parameter. We have only a set of two parameters here:

- `system`: the system architecture parameter.
- `modules`: This is a list of modules, where the actual NixOS system configuration is defined. The `/etc/nixos/configuration.nix` configuration file itself is a Nixpkgs Module, so it can be directly added to the modules list for use.

## References
- https://nixos-and-flakes.thiscute.world/nixos-with-flakes/introduction-to-flakes
- https://nixos-and-flakes.thiscute.world/nixos-with-flakes/nixos-with-flakes-enabled