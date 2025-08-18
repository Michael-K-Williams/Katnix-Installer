# Katnix Installer

An interactive installer script for setting up Katnix NixOS configurations on new machines.

## Overview

This installer simplifies the process of setting up a complete Katnix NixOS system by:

1. **Interactive Configuration**: Prompts for hostname, machine type, and graphics configuration
2. **Automatic Setup**: Clones the configuration repository and sets up the system
3. **Machine-Specific Configs**: Generates custom machine configurations based on user choices
4. **Conditional Features**: Enables/disables features based on machine type (desktop vs laptop)

## Prerequisites

- **NixOS system** with a valid `/etc/nixos/hardware-configuration.nix`
- **Git installed** (can be installed with `nix-shell -p git`)
- **Internet connection** to clone repositories and download packages

## Quick Start

1. **Install Git** (if not already installed):
   ```bash
   nix-shell -p git
   ```

2. **Download and run the installer**:
   ```bash
   curl -O https://raw.githubusercontent.com/Michael-K-Williams/Katnix-Installer/main/install.sh
   chmod +x install.sh
   ./install.sh
   ```

## What it does

### 1. Interactive Configuration
- **Hostname**: Choose a suffix for your machine (e.g., "Desktop" → "Katnix-Desktop")
- **Machine Type**: 
  - Desktop: Includes Elite Dangerous tools (EDHM, EDMC)
  - Laptop: Excludes gaming tools for better battery life
- **Graphics**: Intel or NVIDIA graphics configuration

### 2. System Setup
- Clones the Katnix NixOS configuration repository to `~/nixos`
- Copies your hardware configuration from `/etc/nixos/hardware-configuration.nix`
- Generates a machine-specific configuration file
- Updates the flake configuration to include your machine
- Creates a conditional module for Elite Dangerous applications

### 3. Installation
- Updates flake inputs
- Builds the configuration to check for errors
- Switches to the new configuration
- Provides instructions for future updates

## Configuration Structure

After installation, your configuration will be organized as:

```
~/nixos/
├── flake.nix                    # Main flake configuration
├── configuration.nix            # Core system configuration
├── hardware-configuration.nix   # Your hardware-specific config
├── machines/
│   └── katnix-[hostname].nix   # Your machine-specific config
├── modules/
│   ├── boot.nix                # Bootloader configuration
│   ├── nix.nix                 # Nix settings
│   ├── networking.nix          # Network configuration
│   ├── audio.nix               # Audio/PipeWire setup
│   ├── desktop.nix             # Desktop environment
│   ├── packages.nix            # System packages
│   ├── users.nix               # User and locale settings
│   └── elite-dangerous.nix     # Elite Dangerous apps (conditional)
├── intel-graphics.nix          # Intel graphics configuration
├── nvidia.nix                  # NVIDIA graphics configuration
└── home.nix                    # Home Manager configuration
```

## Features

### Machine Types
- **Desktop**: Full-featured configuration with gaming applications
- **Laptop**: Power-optimized configuration without resource-intensive gaming tools

### Graphics Support
- **Intel**: Optimized for Intel integrated graphics
- **NVIDIA**: Configured for NVIDIA discrete graphics with proper drivers

### Modular Design
- Clean separation of concerns across multiple modules
- Easy to maintain and customize
- Conditional feature loading based on machine type

## Future Updates

After installation, you can update your system with:

```bash
cd ~/nixos
nixos-rebuild switch --flake .#[YourHostname]
```

For example, if your hostname is `Katnix-Desktop`:
```bash
cd ~/nixos
nixos-rebuild switch --flake .#KatnixDesktop
```

## Customization

The installer creates a foundation that you can customize:

1. **Add packages**: Edit `modules/packages.nix`
2. **Modify services**: Update relevant module files
3. **Add new machines**: Create new files in the `machines/` directory
4. **Adjust features**: Modify the conditional logic in modules

## Troubleshooting

### Common Issues

1. **Permission errors**: Ensure you're not running as root
2. **Git not found**: Install with `nix-shell -p git`
3. **Hardware config missing**: Ensure `/etc/nixos/hardware-configuration.nix` exists
4. **Build failures**: Check the error output and verify your hardware configuration

### Getting Help

If you encounter issues:
1. Check the error messages carefully
2. Verify your hardware configuration is correct
3. Ensure all prerequisites are met
4. Review the generated configuration files for any obvious issues

## License

This project is open source and available under the MIT License.
