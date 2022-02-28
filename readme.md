# NSIS Distribution - NVDA Dependency 

This repository contains an installed distribution of [NSIS](https://nsis.sourceforge.io/).

Building NSIS from source is slow, so a prebuilt distribution is used as a dependency for NVDA.

### Current Version

v3.08

### NSIS

NVDA doesn't require most of NSIS, so to make the dependency smaller,
a custom installation is created.
Custom NSIS installations can be created using the installer.
However, to save time, a full installation distributed zip is extracted.
Then the contents are filtered by `include/nsis/.gitignore`.
This creates the same result as building a custom installation with the installer.

**Updating NSIS**
1. Remove the folder `include/nsis/NSIS`.
1. Download the latest `.zip` distribution from [the NSIS sourceforge](https://sourceforge.net/projects/nsis/files/).
  - Example: `nsis-3.08.zip`
1. Extract the installation to `include/nsis/NSIS`
1. From git bash at `include/nsis`
  - Perform `git add NSIS` to track new files.
  - Update `include/nsis/.gitignore` if necessary.
    - Perform `git diff --stat NSIS`, ensure no unexpected files are being added.
    - Check `git clean -xdn NSIS`, ensure no unexpected files are being ignored.
  - Perform `git clean -xdf NSIS`, as fresh clones need to be able to work without ignored files.
1. From a command prompt at NVDA root: `scons launcher`
1. Check for build errors or warnings

## Testing
Test the installer and uninstaller, the two executables created by .nsi scripts.

Test across supported versions of Windows.
Consider:
- ARM / 32bit / 64bit
- Older and newer builds
- Windows `.iso`s using locales other than English

###  Testing the installer

**Test all the code pathways for the installer**
- `--minimal` causes no sound to be played results in an installation
- no `--minimal` causes the installer sound to be played
- When running the installer where `GetUserDefaultUILanguage() == "zh-HK"`, `zh-TW` is instead used as a the locale language
  - as zh-HK uses wrong encoding on Vista/7 #1596

**Smoke test the installer**
- via CMD with the flag
  - `--install` results in an installation and NVDA starting afterwards
  - `--install-silent` results in an installation without NVDA starting afterwards
- executing the launcher via UI reaches the NVDA dialog

### Testing the uninstaller

**Test all the code pathways for the installer**
- When running the installer where `GetUserDefaultUILanguage() == "zh-HK"`, `zh-TW` is instead used as a the locale language
  - as zh-HK uses wrong encoding on Vista/7 (#1596)

**Smoke test the uninstaller**
- Executing the uninstaller and completing the un-installation:
  - keeps user config
  - removes NVDA from Start Menu
  - removes NVDA from the installed folder
