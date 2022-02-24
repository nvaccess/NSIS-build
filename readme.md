# NSIS Distribution - NVDA Dependency 

This repository contains an installed distribution of [NSIS](https://nsis.sourceforge.io/).

Building NSIS from source is slow, so a prebuilt distribution is used as a dependency for NVDA.

### Current Version

v3.08

### Updating NSIS

NVDA doesn't require most of NSIS, so to make the dependency smaller,
a custom installation is created.
Custom NSIS installations can be created using the installer.
However, to save time, a full installation distributed zip is extracted.
Then the contents are filtered by `include/nsis/.gitignore`.
This creates the same result as building a custom installation with the installer.

Updating NSIS steps:
- Remove the folder `include/nsis/NSIS`.
- Download the latest `.zip` distribution from [the NSIS sourceforge](https://sourceforge.net/projects/nsis/files/).
  - Example: `nsis-3.08.zip`
- Extract the installation to `include/nsis/NSIS`
- From git bash at `include/nsis`
  - Perform `git add .` to track new files.
  - Update `include/nsis/.gitignore` if necessary.
    - Perform `git diff --stat`, ensure no unexpected files are being added.
    - Check `git clean -xdn`, ensure no unexpected files are being ignored.
  - Perform `git clean -xdf`, as fresh clones need to be able to work without ignored files.
- From a command prompt at NVDA root: `scons launcher`
- Check for build errors or warnings
- Test the created launcher across supported versions of Windows
