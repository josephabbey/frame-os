# frame-os

BitBake Setup project configuration for **Frame OS** — a Yocto Project-based operating system designed for [Frame](https://github.com/josephabbey/frame) running on the Raspberry Pi Zero 2 W with Pimoroni Inky Impression e-ink displays.

## Ecosystem Overview

The Frame OS stack consists of three complementary repositories:

1. **[frame](https://github.com/josephabbey/frame)**: The Python application and PIL rendering toolkit for Inky Impression displays.
2. **[meta-frame](https://github.com/josephabbey/meta-frame)**: The Yocto / OpenEmbedded layer containing machine configurations, kernel overlays, Python library recipes, and system image targets.
3. **[frame-os](https://github.com/josephabbey/frame-os)** *(this repository)*: The `bitbake-setup` configuration template defining all required upstream layers, branches, and reproducible revisions.

## Quick Start with `bitbake-setup`

### 1. Install `bitbake-setup`

Install `bitbake-setup` in a Python virtual environment:

```bash
python3 -m venv ~/yocto-workspace/bitbake-setup-venv
source ~/yocto-workspace/bitbake-setup-venv/bin/activate
pip install bitbake-setup
```

### 2. Initialize the Setup

To set up the workspace, layers, and configuration interactively:

```bash
bitbake-setup init https://raw.githubusercontent.com/josephabbey/frame-os/main/frame.conf.json
```

Or non-interactively (e.g. for development with sstate acceleration, dev machine, and poky distro):

```bash
bitbake-setup init --non-interactive \
  https://raw.githubusercontent.com/josephabbey/frame-os/main/frame.conf.json \
  frame-with-sstate \
  distro/poky \
  machine/raspberrypi0-2w-64-frame-dev
```

#### Reproducible Builds with Fixed Revisions

To guarantee an exact, bit-for-bit reproducible checkout with pinned Git commit SHAs, supply the `--source-overrides` option:

```bash
bitbake-setup init \
  --source-overrides https://raw.githubusercontent.com/josephabbey/frame-os/main/sources-fixed-revisions.json \
  https://raw.githubusercontent.com/josephabbey/frame-os/main/frame.conf.json
```

### 3. Build an Image

Navigate into the created setup directory (`frame/`) and initialize the BitBake environment:

```bash
cd frame
source build/init-build-env
```

Build the desired image:

- **Development Image** (includes USB Ethernet gadget, SSH dropbear server, debug tools):
  ```bash
  bitbake frame-image-dev
  ```
- **Production Image** (minimal appliance image running `frame` on boot):
  ```bash
  bitbake frame-image
  ```

### 4. Flashing to SD Card

WIC disk images are generated in `build/tmp/deploy/images/raspberrypi0-2w-64-frame-dev/` (or `-frame/`):

```bash
# Using bmaptool (fast & safe)
bmaptool copy build/tmp/deploy/images/raspberrypi0-2w-64-frame-dev/frame-image-dev-raspberrypi0-2w-64-frame-dev.rootfs.wic.bz2 /dev/sdX
```

## Layers Included

| Layer | Upstream Repository | Branch |
| --- | --- | --- |
| **bitbake** | https://git.openembedded.org/bitbake | `2.18` |
| **openembedded-core** | https://git.openembedded.org/openembedded-core | `wrynose` |
| **meta-yocto** | https://git.yoctoproject.org/meta-yocto | `wrynose` |
| **meta-openembedded** | https://git.openembedded.org/meta-openembedded | `wrynose` |
| **meta-raspberrypi** | https://git.yoctoproject.org/meta-raspberrypi | `wrynose` |
| **meta-frame** | https://github.com/josephabbey/meta-frame | `main` |

## License

This repository configuration and documentation are licensed under the MIT License.
