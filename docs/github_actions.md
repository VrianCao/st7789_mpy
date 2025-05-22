# Using GitHub Actions to Build ST7789 MicroPython Module

This document explains how to use the GitHub Actions workflow to automatically build the ST7789 MicroPython module for various target platforms.

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

1. **Automatically on commit**: When you push to the `main` or `master` branch
2. **Manually**: Using the "Run workflow" button in the GitHub Actions UI

Note: This workflow will run alongside the `esp32.yml` workflow, providing builds for all targets.

## Creating a Release

A release is automatically created whenever the workflow runs:

1. The workflow will automatically:
   - Build firmware for all targets
   - Create a draft release on GitHub with a tag in the format `build-YYYY-MM-DD-COMMIT_HASH`
   - Upload firmware packages as release assets

2. Go to the GitHub repository's "Releases" page
3. Find the draft release and review it
4. Edit the release description if needed
5. Publish the release when ready

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