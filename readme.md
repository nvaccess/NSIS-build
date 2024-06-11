# NSIS Distribution - NVDA Dependency 

This repository contains an installed distribution of [NSIS](https://nsis.sourceforge.io/).

Building NSIS from source is slow, so a prebuilt distribution is used as a dependency for NVDA.

### Current Version

v3.10

### NSIS

NVDA doesn't require most of NSIS, so to make the dependency smaller,
a custom installation is created.
While a custom NSIS installation could be created using the installer,
to save time a full installation distributed zip is download and extracted.
Then the contents are filtered by [.gitignore](./.gitignore).
[.gitignore](./.gitignore) was written to filter the same contents
as the NSIS 2.51 custom installation used in older versions of NVDA.
Additionally, unused exes are also filtered to minimize disk space usage.

**Updating NSIS**

These steps use the root directory of this git submodule as the working directory.
When developing from the NVDA repository, the working directory for updating NSIS is `include/nsis`.

1. Remove the folder `./NSIS`.
1. Download the latest `.zip` distribution from [the NSIS sourceforge](https://sourceforge.net/projects/nsis/files/).
    - Example: `nsis-3.08.zip`
1. Extract the installation to `./NSIS`
1. From git bash:
    - Perform `git add NSIS` to track new files.
    - Update `.gitignore` if necessary.
      - Perform `git diff --stat NSIS`, ensure no unexpected files are being added.
        - Expected new files may include:
          - graphics
          - languages
      - Check `git clean -xdn NSIS`, ensure no unexpected files are being ignored by .gitignore.
        - This is unlikely unless NVDA requires new features bundled with NSIS.
        - If building the launcher fails, consider updating .gitignore.
    - Perform `git clean -xdf NSIS`, as fresh clones need to be able to work without ignored files.
1. From a command prompt at the NVDA repository root, build the launcher:
    - `scons launcher`
1. Check for build errors or warnings

## Testing
Test the installer and uninstaller, the two executables created by .nsi scripts.

Test across supported versions of Windows.
Consider:
- ARM / 32bit / 64bit
- Older and newer builds
- Windows `.iso`s using locales other than English

### Testing the installer

Example: (nvda_snapshot_source-example-5683e66.exe)

**Test all the code pathways for the installer**
- `--minimal` causes no sound to be played results in an installation
- no `--minimal` causes the installer sound to be played

**Smoke test the installer**
- via CMD with the flag
  - `--install` results in an installation and NVDA starting afterwards
  - `--install-silent` results in an installation without NVDA starting afterwards
- executing the launcher via UI reaches the NVDA dialog

### Testing the uninstaller 

First install NVDA using the installer.

**Finding the uninstaller:**
- Find uninstall.exe in the NVDA installation folder.
- Ensure that "Uninstall NVDA" can be found via the Windows search menu.
- Ensure that NVDA can be found in the Windows uninstaller tool: "Add or Remove programs"

**Smoke test the uninstaller**
- Executing the uninstaller and completing the un-installation:
  - keeps user config
  - removes NVDA from Start Menu
  - removes NVDA from the installed folder
