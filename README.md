# asusctl-git

Pre-built `Arch Linux` packages for [asusctl](https://gitlab.com/asus-linux/asusctl) - a control daemon and CLI tool for ASUS ROG laptops on Linux.

This repository automatically builds the latest `asusctl-git` package from source using GitHub Actions and publishes ready-to-install packages.

## What is asusctl?

asusctl provides Linux support for ASUS ROG laptop features:

- Keyboard RGB lighting control
- Fan curve customization
- Power profile switching
- Battery charge limit settings
- GPU mode switching (integrated/dedicated)
- ROG key configuration

## Installation

### Option 1: Download from Releases (Recommended)

```bash
# Download the latest package
wget https://github.com/benjipeng/asusctl-git/releases/latest/download/asusctl-git-x86_64.pkg.tar.zst

# Install with pacman
sudo pacman -U asusctl-git-*.pkg.tar.zst
```

### Option 2: Using GitHub CLI

```bash
# List available builds
gh release list --repo benjipeng/asusctl-git

# Download latest release
gh release download --repo benjipeng/asusctl-git --pattern "*.pkg.tar.zst"

# Install
sudo pacman -U asusctl-git-*.pkg.tar.zst
```

### Option 3: Manual Download

1. Go to [Releases](https://github.com/benjipeng/asusctl-git/releases)
2. Download the `.pkg.tar.zst` file from the latest release
3. Install with: `sudo pacman -U <filename>.pkg.tar.zst`

## Post-Installation Setup

```bash
# Enable and start the daemon
sudo systemctl enable --now asusd

# Verify installation
asusctl --version
```

## Usage

### Command Line

```bash
# Show current settings
asusctl -s

# Keyboard lighting
asusctl led-mode static -c ff0000      # Set static red color
asusctl led-mode breathe -c 00ff00     # Breathing green
asusctl -k off                          # Turn off keyboard backlight
asusctl -k low                          # Low brightness
asusctl -k med                          # Medium brightness
asusctl -k high                         # High brightness

# Power profiles
asusctl profile -l                      # List available profiles
asusctl profile -P Performance          # Set performance mode
asusctl profile -P Quiet                # Set quiet mode

# Battery charge limit (preserve battery health)
asusctl -c 80                           # Limit charge to 80%

# Fan curves
asusctl fan-curve -m Quiet              # Quiet fan profile
asusctl fan-curve -m Performance        # Performance fan profile
```

### GUI Application

```bash
# Launch the control center
rog-control-center
```

## Dependencies

**Runtime (installed automatically):**
- libusb
- systemd
- udev

**Optional (for GUI):**
```bash
sudo pacman -S libappindicator-gtk3 gtk3
```

## Building Locally

If you prefer to build the package yourself:

```bash
# Clone this repository
git clone https://github.com/benjipeng/asusctl-git.git
cd asusctl-git

# Build with makepkg
makepkg -si
```

## Upstream Resources

- **Source Code:** https://gitlab.com/asus-linux/asusctl
- **ASUS Linux Project:** https://asus-linux.org/
- **Documentation:** https://asus-linux.org/manual/asusctl-manual/
- **Arch Wiki:** https://wiki.archlinux.org/title/ASUS_Linux

## Automated Builds

This repository uses GitHub Actions to automatically build packages:

- **Weekly builds:** Every Sunday at midnight UTC
- **On PKGBUILD changes:** When the build recipe is updated
- **On version tags:** When new versions are tagged

## Troubleshooting

### Daemon not starting

```bash
# Check daemon status
sudo systemctl status asusd

# View logs
journalctl -u asusd -f
```

### Kernel modules not loaded

```bash
# Load required modules
sudo modprobe asus-wmi
sudo modprobe asus-nb-wmi

# Verify modules are loaded
lsmod | grep asus
```

### Permission issues

```bash
# Add yourself to the input group
sudo usermod -aG input $USER

# Log out and back in for changes to take effect
```

## License

The asusctl project is licensed under MPL-2.0. See the [upstream repository](https://gitlab.com/asus-linux/asusctl) for details.
