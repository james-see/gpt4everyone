---
name: Multi-OS Build and Rebrand
overview: Add Linux and Windows GitHub Actions build jobs to the existing release workflow, update the news/updates source to use GitHub releases, and rebrand user-facing text from "GPT4All" to "GPT4Everyone".
todos:
  - id: 1
    content: Add Linux build job to GitHub Actions workflow
    status: completed
  - id: 2
    content: Add Windows build job to GitHub Actions workflow
    status: completed
  - id: 3
    content: Update welcome screen news source to use GitHub Releases API
    status: completed
  - id: 4
    content: Rebrand user-facing text from GPT4All to GPT4Everyone
    status: completed
isProject: false
---

# Multi-OS Build and GPT4Everyone Rebrand

## 1. Add Linux and Windows Build Jobs

Extend `[.github/workflows/release.yml](.github/workflows/release.yml)` with two new jobs:

### Linux Build Job (`build-linux`)

- Runner: `ubuntu-22.04` (matches installer requirements in `installer_control.qs`)
- Install Qt 6.8.0 with modules: `qt5compat qthttpserver qtpdf qtshadertools qtwebsockets`
- Install dependencies: `cmake`, `ninja-build`, `libvulkan-dev`, `libxcb*-dev`
- Install `linuxdeployqt` for bundling Qt dependencies
- Build with CMake/Ninja
- Package as AppImage or tarball
- Upload to GitHub Release

### Windows Build Job (`build-windows`)

- Runner: `windows-latest`
- Install Qt 6.8.0 for Windows with same modules
- Use MSVC compiler
- Build with CMake
- Use `windeployqt` for dependency bundling
- Package as installer or zip
- Upload to GitHub Release

Key CMake flags for both:

```javascript
-DCMAKE_BUILD_TYPE=Release
-DGPT4ALL_TEST=OFF
```

## 2. Update Welcome Screen News Source

Modify `[gpt4all-chat/src/download.cpp](gpt4all-chat/src/download.cpp)`(gpt4all-chat/src