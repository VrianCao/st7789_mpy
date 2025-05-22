# GitHub Actions Workflow for ST7789 MicroPython Module

This directory contains GitHub Actions workflow configurations for automatically building the ST7789 MicroPython module for various target platforms.

## Workflow: `build.yml`

The `build.yml` workflow automates the process of building MicroPython firmware with the ST7789 module for all supported targets.

### Triggers

The workflow runs on:
- Pushes to `main` or `master` branches
- Manual trigger via GitHub Actions UI

Note: This workflow will run alongside the `esp32.yml` workflow, providing builds for all targets.

### Jobs

The workflow consists of the following jobs:

1. **build-esp32-targets**: Builds firmware for ESP32-based targets
   - Uses the official ESP-IDF v5.4 Docker image
   - Builds for multiple ESP32 variants (GENERIC, GENERIC_SPIRAM, ESP32-C3, ESP32-S2, ESP32-S3, etc.)
   - Includes specific board targets (M5CORE, T-DISPLAY, etc.)

2. **build-rp2-targets**: Builds firmware for Raspberry Pi Pico (RP2040) targets
   - Builds for RP2, RP2W, and T-DISPLAY-RP2040

3. **build-stm32-targets**: Builds firmware for STM32-based targets
   - Currently only builds for PYBV11 (Pyboard v1.1)

4. **create-release**: Creates a GitHub release with firmware packages
   - Creates a draft release with separate ZIP files for each target
   - Automatically generates a tag with date and commit hash
   - Includes README files with build information

5. **build-all-in-one**: Creates a combined firmware package
   - Collects all built firmware files
   - Creates a single ZIP file with all targets

### Artifacts

Each job produces artifacts that are uploaded to GitHub:
- Individual firmware files (.bin, .elf, .map, .uf2, .dfu)
- Target-specific ZIP packages
- All-in-one firmware package

### Customization

To customize the build process:
1. Modify the target lists in the matrix configuration
2. Adjust the frozen modules copied to the build
3. Change build parameters as needed

## Usage

To use this workflow:

1. Push to the `main` or `master` branch to trigger a build and release
2. Use the "Run workflow" button in the GitHub Actions UI for manual builds

## Troubleshooting

If a build fails:
1. Check the job logs for specific error messages
2. Verify that the target board is correctly defined in MicroPython
3. Ensure the ESP-IDF version is compatible with the MicroPython version