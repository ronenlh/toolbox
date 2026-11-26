# Homebrew installation

Homebrew is a package manager for macOS and Linux. On macOS, install Apple's Command Line Tools first:

```bash
xcode-select --install
```

Then run Homebrew's official installer:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Follow the post-install instructions printed by the installer. On Apple Silicon Macs, this commonly adds Homebrew to Zsh with:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

Verify the installation:

```bash
brew --version
brew doctor
```

Common commands:

```bash
brew search PACKAGE
brew install PACKAGE
brew update
brew upgrade
brew uninstall PACKAGE
```

Official documentation: <https://brew.sh/>
