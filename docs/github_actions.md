# Using GitHub Actions with ST7789 MicroPython Module

This document explains how to use the GitHub Actions workflows to automatically build and manage the ST7789 MicroPython module.

## Available Workflows

The repository includes the following GitHub Actions workflows:

1. **Build Workflow** - Builds firmware for all supported targets
2. **Version Update Workflow** - Updates the module version across all files

For version management details, see [Version Management](version_management.md).

## Overview

The GitHub Actions workflow automates the process of building MicroPython firmware with the ST7789 module integrated. It uses the ESP-IDF Docker image to ensure a consistent build environment and builds firmware for all supported targets.

## Supported Targets

The workflow builds firmware for the following targets:

### ESP32-based Targets
- GENERIC-7789
- GENERIC_SPIRAM-7789
- GENERIC_C3
- GENERIC_C3_USB
- GENERIC_S2
- GENERIC_S3_7789
- GENERIC_S3_SPIRAM
- GENERIC_S3_SPIRAM_OCT
- LOLIN_S2_MINI
- M5CORE
- M5CORE2
- T-DISPLAY-ESP32
- T-DONGLE-S3
- TWATCH-2020
- ESP32_BOX_LITE

### Raspberry Pi Pico (RP2040) Targets
- RP2
- RP2W
- T-DISPLAY-RP2040

### STM32-based Targets
- PYBV11 (Pyboard v1.1)

## How the Workflow Works

1. The workflow checks out both the ST7789 module repository and the MicroPython repository
2. It sets up the build environment using the ESP-IDF Docker image
3. For each target:
   - It builds the MicroPython cross-compiler
   - Optionally copies font files as frozen modules
   - Builds the firmware with the ST7789 module integrated
   - Uploads the resulting firmware files as artifacts
4. If triggered by a tag, it creates a GitHub release with firmware packages
5. It also creates an all-in-one firmware package containing all targets

## Using the Built Firmware

After the workflow completes, you can:

1. Download the artifacts from the GitHub Actions run
2. Flash the appropriate firmware to your device:
   - For ESP32: Use `esptool.py` to flash the `.bin` file
   - For RP2040: Copy the `.uf2` file to the RP2040 in bootloader mode
   - For STM32: Use DFU tools to flash the `.dfu` file

## Triggering the Workflow

The workflow can be triggered in two ways:

1. **Automatically on tag**: When you create a tag starting with `v` (e.g., `v1.0.0`)
2. **Manually**: Using the "Run workflow" button in the GitHub Actions UI

Note: Regular pushes to the repository are handled by the `esp32.yml` workflow, which builds only ESP32 targets.

## Creating a Release

To create a release with all firmware files:

1. Create and push a tag starting with `v`:
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```

2. The workflow will automatically:
   - Build all firmware targets
   - Create a draft GitHub release
   - Attach firmware packages to the release

3. Review the draft release on GitHub and publish it when ready

## Customizing the Build

To customize the build process:

1. Modify the `.github/workflows/build.yml` file:
   - Add or remove targets from the matrix configuration
   - Change the frozen modules included in the build
   - Adjust build parameters

2. Commit and push your changes to trigger a new build

## Troubleshooting

If you encounter issues with the build:

1. Check the GitHub Actions logs for specific error messages
2. Verify that your target board is correctly defined in MicroPython
3. Ensure you're using a compatible ESP-IDF version
4. Check that the ST7789 module code is compatible with the MicroPython version

For more detailed information, refer to the README in the `.github/workflows` directory.