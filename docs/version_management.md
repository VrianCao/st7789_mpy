# Version Management

This document explains how to manage versions of the ST7789 MicroPython module using GitHub Actions.

## Automatic Version Updates

The repository includes a GitHub Actions workflow that can automatically update the version number in all relevant files.

### How to Update the Version

1. Go to the GitHub repository's "Actions" tab
2. Select the "Update Module Version" workflow
3. Click the "Run workflow" button
4. Enter the new version number (e.g., "1.2.3")
5. Click "Run workflow"

The workflow will:
1. Update the version number in `st7789/__init__.py`
2. Update the version number in `st7789/st7789.c` (if it exists)
3. Create or update a `version.txt` file with the new version
4. Commit the changes to the repository
5. Create a new tag with the version number (e.g., "v1.2.3")

### Version Format

The recommended version format is [Semantic Versioning](https://semver.org/):

```
MAJOR.MINOR.PATCH
```

Where:
- MAJOR: Incompatible API changes
- MINOR: Backwards-compatible new features
- PATCH: Backwards-compatible bug fixes

## Manual Version Updates

If you need to update the version manually:

1. Update the version in `st7789/__init__.py`:
   ```python
   __version__ = "1.2.3"
   ```

2. Update the version in `st7789/st7789.c` (if it exists):
   ```c
   #define ST7789_VERSION "1.2.3"
   ```

3. Create or update `version.txt`:
   ```
   1.2.3
   ```

4. Commit the changes:
   ```bash
   git add st7789/__init__.py st7789/st7789.c version.txt
   git commit -m "Bump version to 1.2.3"
   git tag -a "v1.2.3" -m "Version 1.2.3"
   git push origin main --tags
   ```