# Installation

Learn how to install Tuist in your environment

## Overview

Tuist is a [command-line application](https://en.wikipedia.org/wiki/Command-line_interface) that you need to install in your environment before you can use it. The installation consists of an executable, dynamic frameworks, and a set of resources (for example, templates). Although you could manually build Tuist from the sources, we recommend using one of the following installation methods to ensure a valid installation.

### Recommended: [mise](https://github.com/jdx/mise)

Tuist defaults to [mise](https://github.com/jdx/mise) as a tool to deterministically manage and activate versions of Tuist.
If you don't have it installed on your system,
you can use any of these [installation methods](https://mise.jdx.dev/getting-started.html).
Remember to add the suggested line to your shell, which will ensure the right version is activated when you choose a Tuist project directory in your terminal session.

> Note: mise is recommended over alternatives like [Homebrew](https://brew.sh) because it supports scoping and activating versions to directories, ensuring every environment uses the same version of Tuist deterministically.

Once installed, you can install Tuist through any of the following commands:


```bash
# Tuist
mise install tuist            # Install the current version specified in .tool-versions/.mise.toml
mise install tuist@x.y.z      # Install a specific version number
mise install tuist@3          # Install a fuzzy version number
mise use tuist@x.y.z          # Use tuist-x.y.z in the current project
mise use -g tuist@x.y.z       # Use tuist-x.y.z as the global default
mise use tuist@latest         # Use the latest tuist in the current directory
mise use -g tuist@system      # Use the system's tuist as the global default

# Tuist Cloud (For users of Tuist Cloud)
# Note: You need to install one OR the other, not both
mise install tuist-cloud            # Install the current version specified in .tool-versions/.mise.toml
mise install tuist-cloud@x.y.z
mise install tuist-cloud@3
mise use tuist-cloud@x.y.z   
mise use -g tuist-cloud@x.y.z       # Use tuist-cloud-x.y.z as the global default
mise use tuist-cloud@latest         # Use the latest tuist-cloud in the current directory
mise use -g tuist-cloud@system      # Use the system's tuist-cloud as the global default
```

#### Continuous integration

If you are using Tuist in a continuous integration environment, the following sections show how to install Tuist in the most common ones:

##### Xcode Cloud

You'll need a [post-clone](https://developer.apple.com/documentation/xcode/writing-custom-build-scripts#Create-a-custom-build-script) script that installs Mise and Tuist:

```bash
#!/bin/sh
curl https://mise.jdx.dev/install.sh | sh
mise install # Installs the version from .mise.toml

# Runs the version of Tuist indicated in the .mise.toml file
mise x tuist generate
```

##### Codemagic

To install and use Mise and Tuist in [Codemagic](https://codemagic.io), you can add an additional step to your workflow, and run Tuist through `mise x tuist` to ensure that the right version is used:

```yaml
workflows:
  lint:
    name: Build
    max_build_duration: 30
    environment:
      xcode: 15.0.1
    scripts:
      - name: Install Mise
        script: |
          curl https://mise.jdx.dev/install.sh | sh
          mise install # Installs the version from .mise.toml
      - name: Build
        script: mise x tuist build
```

##### GitHub Actions

On GitHub Actions you can use the [mise-action](https://github.com/jdx/mise-action), which abstracts the installation of Mise and Tuist and the configuration of the environment:

```yaml
name: test
on:
  pull_request:
    branches:
      - main
  push:
    branches:
      - main
jobs:
  lint:
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v3
      - uses: jdx/mise-action@v2
```


> Tip: We recommend using `mise use --pin` in your Tuist projects to pin the version of Tuist across environments. The command will create a `.tool-versions` file containing the version of Tuist.

> Important: **Tuist Cloud** (<doc:tuist-cloud>), a closed-source extension of Tuist with optimizations such as binary caching and selective testing, is distributed as a different asdf plugin, tuist-cloud. Note that by using it, you agree to the [Tuist Cloud Terms of Service](https://tuist.io/terms/).

### Alternative: Homebrew

If version pinning across environments is not a concern for you,
you can install Tuist using [Homebrew](https://brew.sh) and [our formulas](https://github.com/tuist/homebrew-tuist):

```bash
brew tap tuist/tuist
brew install tuist
brew install tuist@x.y.z
brew install tuist-cloud # If you are a Tuist Cloud user
```
