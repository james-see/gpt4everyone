---
name: Multi-OS Build and Branding Update
overview: Add Linux and Windows builds to GitHub Actions workflow, update all user-facing branding from GPT4All to GPT4Everyone, and update the welcome screen to fetch product updates from GitHub releases.
todos:
  - id: add-linux-build
    content: Add Linux build job to release.yml workflow with Qt installation, CMake build, and linuxdeployqt bundling
    status: completed
  - id: add-windows-build
    content: Add Windows build job to release.yml workflow with Qt installation, CMake build, and windeployqt bundling
    status: completed
  - id: update-app-name
    content: Update application name from 'GCP4ALL' to 'GPT4Everyone' in main.cpp and main.qml
    status: completed
  - id: update-welcome-text
    content: Update welcome screen text from 'GPT4All' to 'GPT4Everyone' in HomeView.qml
    status: completed
  - id: update-ui-strings
    content: Update all user-facing strings in QML files (ChatView, ModelsView, Settings, etc.) from 'GPT4All' to 'GPT4Everyone'
    status: completed
  - id: update-cpack-config
    content: Update CPack configuration with GPT4Everyone branding for installers and package names
    status: completed
  - id: update-news-source
    content: Modify download.cpp to fetch release notes from GitHub Releases API instead of gpt4all.io
    status: completed
  - id: update-workflow-name
    content: Update workflow name and artifact names to reflect GPT4Everyone branding
    status: completed
isProject: false
---

# Multi-OS Build and Branding Update Plan

## Overview

This plan adds Linux and Windows build jobs to the existing GitHub Actions release workflow, updates all user-facing branding from "GPT4All" to "GPT4Everyone", and modifies the welcome screen to display product updates from GitHub releases.

## Current State Analysis

### Existing Mac Build

- Working GitHub Actions workflow at `.github/workflows/release.yml`
- Uses Qt

6.8.0, CMake, Ninja

- Includes code signing, notarization, and DMG creation
- Creates releases with DMG artifacts

### Branding References Found

- Application name: "GCP4ALL" in `main.cpp` (line 81) - should be "GPT4Everyone"
- Welcome text: "Welcome to GPT4All" in `HomeView.qml` (line 49)
- Multiple QML files with "GPT4All" strings
- CMakeLists.txt: project name, bundle identifiers, output names
- CPack config: package names, installer titles
- News source: Currently fetches from `http://gpt4all.io/meta/latestnews.md` (line 173 in `download.cpp`)

## Implementation Tasks

### 1. Add Linux Build Job

**File**: `.github/workflows/release.yml`

- Add `build-linux` job similar to `build-macos`
- Use `ubuntu-latest` runner
- Install Qt 6.8.0 using `jurplel/install-qt-action@v3`
- Install dependencies: `ninja`, `cmake`, `python3-pip`, `click`
- Build using CMake/Ninja (same as Mac)
- Use `linuxdeployqt` for bundling (already configured in `cmake/deploy-qt-linux.cmake.in`)
- Create AppImage or tarball package
- Upload artifact to release

**Key differences from Mac**:

- No code signing required
- Use `linuxdeployqt` instead of `macdeployqt`
- Package as AppImage or tar.gz instead of DMG

### 2. Add Windows Build Job

**File**: `.github/workflows/release.yml`

- Add `build-windows` job
- Use `windows-latest` runner
- Install Qt 6.8.0 using `jurplel/install-qt-action@v3`
- Install dependencies: `ninja`, `cmake`, `python3-pip`, `click`
- Build using CMake/Ninja
- Use `windeployqt` for bundling (already configured in `cmake/deploy-qt-windows.cmake.in`)
- Create installer using CPack IFW (Qt Installer Framework)
- Upload artifact to release

**Key differences**:

- May need Windows code signing (optional, can add later)
- Use `windeployqt` instead of `macdeployqt`
- Package as `.exe` installer

### 3. Update Branding - Application Name

**Files to update**:

- `gpt4all-chat/src/main.cpp` (line 81): Change `"GCP4ALL"` to `"GPT4Everyone"`
- `gpt4all-chat/qml/main.qml` (line 24): Update window title from `"GPT4All v%1"` to `"GPT4Everyone v%1"`
- `gpt4all-chat/qml/HomeView.qml` (line 49): Change `"Welcome to GPT4All"` to `"Welcome to GPT4Everyone"`
- `gpt4all-chat/qml/HomeView.qml` (line 257): Change `"GCP4ALL"` to `"GPT4Everyone"`
- `gpt4all-chat/qml/HomeView.qml` (line 282): Change `"GCP4ALL Documentation"` to `"GPT4Everyone Documentation"`

### 4. Update Branding - User-Facing Text

**Files to update**:

- `gpt4all-chat/qml/HomeView.qml` (line 191): Update accessible description
- `gpt4all-chat/qml/ChatItemView.qml` (line 83): Change `"GPT4All"` to `"GPT4Everyone"`
- `gpt4all-chat/qml/ChatView.qml` (line 777): Update message text
- `gpt4all-chat/qml/ModelsView.qml` (line 49): Update message text
- `gpt4all-chat/qml/AddGPT4AllModelView.qml` (line 31): Update description text
- `gpt4all-chat/qml/ApplicationSettings.qml`: Update help text references
- `gpt4all-chat/qml/StartupDialog.qml`: Update all "GPT4All" references
- `gpt4all-chat/qml/NetworkDialog.qml`: Update all "GPT4All" references
- `gpt4all-chat/qml/ModelSettings.qml`: Update tooltip text

### 5. Update Branding - Build Configuration

**Files to update**:

- `gpt4all-chat/CMakeLists.txt`:
- Line 11: Project name (keep as `gpt4all` for compatibility, but update display names)
- Line 406: `MACOSX_BUNDLE_GUI_IDENTIFIER` - consider updating
- Line 409: `OUTPUT_NAME` - keep as `gpt4all` for binary compatibility
- `gpt4all-chat/cmake/cpack_config.cmake`:
- Line 40: `CPACK_PACKAGE_EXECUTABLES` - update to "GPT4Everyone"
- Line 41: `CPACK_CREATE_DESKTOP_LINKS` - update to "GPT4Everyone"
- Line 42: `CPACK_IFW_PACKAGE_NAME` - update to "GPT4Everyone"
- Line 43: `CPACK_IFW_PACKAGE_TITLE` - update to "GPT4Everyone Installer"
- Line 44: `CPACK_IFW_PACKAGE_PUBLISHER` - update to appropriate publisher name
- Line 45: `CPACK_IFW_PRODUCT_URL` - update to gpt4everyone URL

### 6. Update News/Updates Feature

**File**: `gpt4all-chat/src/download.cpp`

- Line 173: Change news URL from `http://gpt4all.io/meta/latestnews.md` to fetch from GitHub Releases API
- Implement function to fetch latest release notes from `https://api.github.com/repos/james-see/gpt4everyone/releases/latest`
- Parse release body/notes and display in welcome screen
- Fallback gracefully if API call fails

**Alternative approach**: Create a simple markdown file in the repo (e.g., `docs/latestnews.md`) and serve it via GitHub Pages or raw.githubusercontent.com

### 7. Update Release Workflow Name

**File**: `.github/workflows/release.yml`

- Line 1: Change workflow name from `"Build and Release GCP4ALL"` to `"Build and Release GPT4Everyone"`

### 8. Update Workflow Artifacts

**File**: `.github/workflows/release.yml`

- Ensure all three jobs (macos, linux, windows) upload artifacts
- Use consistent naming: `gpt4everyone-installer-{platform}.{ext}`
- Update release creation step to include all platform artifacts

## Implementation Notes

### Linux Build Considerations

- `linuxdeployqt` needs to be available - may need to download/build it
- Consider creating AppImage for better portability
- May need to handle library dependencies

### Windows Build Considerations

- Qt Installer Framework needs to be available
- May want to add code signing later (requires Windows certificate)
- Consider both x64 and ARM64 builds if needed

### Branding Strategy

- Keep internal names (binary names, folder names) as `gpt4all` for compatibility
- Update all user-visible strings to `GPT4Everyone`
- Update URLs to point to `james-see/gpt4everyone` repository

### News/Updates Strategy

- Fetch from GitHub Releases API for automatic updates
- Parse markdown from release notes
- Display in existing welcome screen news section
- Cache or handle gracefully on network errors

## Testing Checklist

- Mac build still works
- Linux build completes successfully
- Windows build completes successfully
- All artifacts upload to releases
- Application shows "GPT4Everyone" in all UI locations
- Welcome screen displays news from GitHub releases
- Installers have correct branding
- Application functionality unchanged

