# Build Classic ASP Release Action

This GitHub Action packages a Classic ASP project into a zip file and creates a GitHub release with semantic versioning and automated release notes. It automates the packaging and release process for Classic ASP applications, with support for both standard and pre-release versioning.

---

## Features
- Packages the entire content of a specified Classic ASP project folder into a versioned zip file.
- Automatically generates a semantic version tag (with pre-release support and suffix).
- Publishes a GitHub Release and attaches the zip file (for standard releases).
- Adds a step summary with release details.
- Posts status comments on related pull requests.
- Outputs the outcome of the release (Release Created, No Release, or Release Failed).
- Timestamps the release in Central Daylight Time (CDT).

---

## Inputs

| Name                | Description                                                                                  | Required |
|---------------------|----------------------------------------------------------------------------------------------|----------|
| `project-folder-path` | The path to the Classic ASP project folder to be packaged.                                 | Yes      |
| `prerelease`        | Mark the release as a pre-release (`true`/`false`).                                          | Yes      |
| `prerelease-suffix` | The suffix to append to the version number for pre-releases. For non-pre-releases, the version is based on the latest pre-release with this suffix. | Yes      |
| `package-name`      | The base name for the zip file containing the build artifacts.                               | Yes      |
| `token`             | The GitHub token used for authentication to create the release.                              | Yes      |

---

## Outputs

| Name              | Description                                                                                                   |
|-------------------|--------------------------------------------------------------------------------------------------------------|
| `version-tag`     | The new tag created for the release (e.g., `v1.0.0`).                                                        |
| `version-number`  | The new tag with the "v" prefix removed (e.g., `1.0.0`).                                                     |
| `zip-file-name`   | The name of the zip file containing the build artifacts (e.g., `my-package.1.0.0.zip`).                      |
| `zip-file-path`   | The full path to the zip file containing the build artifacts.                                                |
| `build-status`    | The build/release outcome—one of: Release Created, No Release, or Release Failed.                            |

---

## Usage

**Important:**  
- This action uses PowerShell for archiving and can be run on both Windows and Ubuntu (but requires PowerShell 7+ on Ubuntu runners).
- Designed to be used as part of your pull request or main branch release flow.

### Example Workflow

```yaml
name: Build and Release Classic ASP Project

on:
  pull_request:
    types: [closed]

jobs:
  build-and-release:
    if: github.event.pull_request.merged == true
    name: Create Release
    runs-on: windows-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v6

      - name: Package Classic ASP Project and Create Release
        uses: lee-lott-actions/build-classic-asp-release@v1
        with:
          project-folder-path: 'src/ClassicASP'
          prerelease: 'false'
          prerelease-suffix: 'beta'
          package-name: 'my-asp-package'
          token: ${{ secrets.GITHUB_TOKEN }}
```

---

## Notes

- Tag and release creation use [semantic versioning](https://semver.org/).
- For pre-releases, the action creates an annotated tag but does not publish a GitHub Release.
- For standard releases, a zip file will be created and attached to a GitHub Release.
- A release summary, including release URL and zip file name, is added to the workflow run summary.
- The release timestamp reflects Central Daylight Time (CDT).

---
