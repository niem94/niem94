---
title: MacOS as a Developer Machine (WIP)
layout: post
---

![blog-post-image](/assets/images/under-construction.jpg)

In this post, I will share my experience setting up MacOS as a developer machine. I will cover the following topics:

- [Managing Packages](#managing-packages)
  - [Installing and Using Homebrew](#installing-and-using-homebrew)
  - [Benefits of Using Homebrew](#benefits-of-using-homebrew)
- [Managing Dotfiles and System Configuration](#managing-dotfiles-and-system-configuration)
  - [Installing and Using MackUp](#installing-and-using-mackup)
  - [Storing Brew Bundles in MackUp](#storing-brew-bundles-in-mackup)
  - [Convenience Scripts](#convenience-scripts)
  - [Benefits of Using MackUp](#benefits-of-using-mackup)
- [Git, SSH, and GPG Keys](#git-ssh-and-gpg-keys)
- [Configuring the Terminal](#configuring-the-terminal)
- [Setting Up Your IDE](#setting-up-your-ide)
- [Remote Development](#remote-development)

## Managing Packages

There are a few options available for managing packages on MacOS. Notable package managers include:

- [Homebrew](https://brew.sh)
- [MacPorts](https://www.macports.org)

For no particular reason, I went with Homebrew, and I have been using it extensively, for a few years now. In my experience, Homebrew has been reliable and easy to use, and I have yet to encounter that it is missing a feature that I need, or that it is not working as expected.

### Installing and Using Homebrew

Installing Homebrew is straightforward, and is well documented on the [Homebrew website](https://brew.sh). Once installed, you can install packages using the `brew install` command. For example, to install Git, you can run:

```bash
brew install git
```

It can also be used to install applications, such as Visual Studio Code:

```bash
brew install --cask visual-studio-code
```

Homebrew can also snapshot your installed packages and applications, which can be useful when migrating to a new machine, or when you own multiple machines. You will learn how I prefer to manage my system configuration including my packages and applications in the next section [Managing Dotfiles and System Configuration](#managing-dotfiles-and-system-configuration).

### Benefits of Using Homebrew

- Easy to install and use
- Extensive library of packages
- Ability to install applications
- Ability to snapshot installed packages and applications

## Managing Dotfiles and System Configuration

Many applications and packages require storing configuration files in your home directory. These files are often referred to as dotfiles, as the folders and files are prefixed with a dot (`.`). Examples of dotfiles include `.bashrc`, `.gitconfig`, and `.vimrc`, but there are many more.

When you use valuable time configuring, for example, your Git settings, you probably want to keep these settings when you switch to a new machine. This is where dotfiles come in handy. By storing your dotfiles in a version-controlled repository, you can easily sync your configuration across multiple machines.

However keeping stuff in sync is no simple task, and there are a multitude of ways to do it. I have personally tried a few different approaches, that varied in complexity and flexibility. It was not untill I discovered [MackUp](https://github.com/lra/mackup) that I found a solution that worked well for me.

MackUp is a simple utility that, when executed, will symlink your dotfiles and system configuration to a storage provider of your choice. It supports a variety of storage providers, such as Dropbox, Google Drive, and OneDrive, and Git. As I prefer to keep all my stuff in Git, I of course use the Git storage provider.

### Installing and Using MackUp

To get started with MackUp, you can install it using Homebrew:

```bash
brew install mackup
```

After installing MackUp, you can configure it by creating a `.mackup.cfg` file in your home directory. Here is an example configuration file:

```ini
[storage]
engine = git
directory = ~/dotfiles
```

This configuration tells MackUp to store your dotfiles in a Git repository located at `~/dotfiles`. You can then run `mackup backup` to symlink your dotfiles to the Git repository. When you switch to a new machine, you can run `mackup restore` to restore your dotfiles.

### Storing Brew Bundles in MackUp

In addition to storing your dotfiles, you can also store your Homebrew packages and applications in MackUp. This can be done by creating a Brewfile, which lists all the packages and applications you want to install. You can create a Brewfile of your installed packages and applications by running:

```bash
brew bundle dump
```

This will create a Brewfile in your home directory that lists all your installed packages and applications. To install these packages and applications on a new machine, you can run:

```bash
brew bundle install
```

> [!NOTE]
> The Brewfile is automatically picked up by MackUp when you run `mackup backup`.

### Convenience Scripts

To make it easier to manage your dotfiles and system configuration, you can create convenience scripts that automate the process. Here is a few scripts for managing your dotfiles and system configuration:

#### Backup Script

This script will backup your dotfiles and system configuration to a Git repository, and store your Homebrew packages and applications in a Brewfile. It supports both macOS and Linux, and is built to support two machine types: `main` and `headless`, as I use MacOS as both my main machine and as a headless server. To run the script you can use the following commands:

```bash
chmod +x backup.sh # To make the script executable
./backup.sh main # To backup your main machine
./backup.sh headless # To backup your headless machine
```

The script must be placed in the root of your dotfiles repository, that should be located in your home directory. You will have to run the script from the machine you want to backup.

```bash
#!/bin/bash
check_os() {
  if [ "$(uname)" != "Darwin" ] && [ "$(uname)" != "Linux" ]; then
    echo "This script is only for macOS and Linux"
    exit 1
  fi
}

check_machine_type() {
  local machine_type=$1
  if [ "$machine_type" != "main" ] && [ "$machine_type" != "headless" ]; then
    echo "You need to provide a valid machine type: main or headless"
    exit 1
  fi
}

create_mackup_config() {
  local machine_type=$1
  echo_title "📝 Creating Mackup config"
  if [ -f "$HOME/.mackup.cfg" ]; then
    return
  fi
  echo "[storage]
engine = file_system
path = $HOME/dotfiles/$machine_type" >"$HOME/.mackup.cfg"
}

install_homebrew() {
  if ! [ -x "$(command -v brew)" ]; then
    echo_title "🍺 Installing Homebrew"
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    (
      echo
      echo 'eval "$(/opt/homebrew/bin/brew shellenv)"'
    ) >>"$HOME"/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
  fi
}

install_rosetta() {
  if ! [ -x "$(command -v arch)" ]; then
    echo_title "📄 Installing Rosetta 2"
    softwareupdate --install-rosetta --agree-to-license
  fi
}

install_mackup() {
  if ! [ -x "$(command -v mackup)" ]; then
    echo_title "📦 Installing Mackup"
    brew install mackup
  fi
}

echo_title() {
  local title=$1
  echo ""
  echo "$title"
}

machine_type=$1

check_os
check_machine_type "$machine_type"
create_mackup_config "$machine_type"
install_homebrew
if [ "$(uname)" == "Darwin" ]; then
  install_rosetta
fi
install_mackup

cd ~/ || exit
brew bundle dump -f
brew bundle --force cleanup
mackup backup --force
mackup uninstall --force
brew upgrade
```

#### Sync Script

This script will sync your dotfiles and system configuration from a Git repository, and install your Homebrew packages and applications from a Brewfile. It supports both macOS and Linux, and is built to support two machine types: `main` and `headless`, as I use MacOS as both my main machine and as a headless server. To run the script you can use the following commands:

```bash
chmod +x sync.sh # To make the script executable
./sync.sh main # To sync your main machine
./sync.sh headless # To sync your headless machine
```

The script must be placed in the root of your dotfiles repository, that should be located in your home directory. You will have to run the script from the machine you want to sync to.

```bash
#!/bin/bash

check_os() {
  if [ "$(uname)" != "Darwin" ] && [ "$(uname)" != "Linux" ]; then
    echo "This script is only for macOS and Linux"
    exit 1
  fi
}

check_machine_type() {
  local machine_type=$1
  if [ "$machine_type" != "main" ] && [ "$machine_type" != "headless" ]; then
    echo "You need to provide a valid machine type: main or headless"
    exit 1
  fi
}

install_homebrew() {
  if ! [ -x "$(command -v brew)" ]; then
    echo_title "🍺 Installing Homebrew"
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    (
      echo
      echo 'eval "$(/opt/homebrew/bin/brew shellenv)"'
    ) >>"$HOME"/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
  fi
}

install_rosetta() {
  if ! [ -x "$(command -v arch)" ]; then
    echo_title "📄 Installing Rosetta 2"
    softwareupdate --install-rosetta --agree-to-license
  fi
}

create_link() {
  local source=$1
  local destination=$2
  local link_type=$3
  echo "   Linking $source to $destination"
  if [ "$link_type" = "hard" ]; then
    sudo ln -f "$source" "$destination"
  else
    ln -sf "$source" "$destination"
  fi
}

echo_title() {
  local title=$1
  echo ""
  echo "$title"
}

symlink_dotfiles() {
  local machine_type=$1
  echo_title "🔄 Symlinking dotfiles - homebrew"
  create_link "$PWD/$machine_type/Mackup/Brewfile" "$HOME/Brewfile" "symbolic"

  echo_title "🔄 Symlinking Mackup"
  create_link "$PWD/$machine_type/Mackup/.mackup.cfg" "$HOME/" "symbolic"
  create_link "$PWD/$machine_type/Mackup/.mackup" "$HOME/" "symbolic"
}

sync_homebrew() {
  echo_title "🍻 Sync Homebrew packages"
  brew bundle install --file "$HOME"/Brewfile
  brew bundle --force cleanup --file "$HOME"/Brewfile
  brew upgrade
}

machine_type=$1

check_os
check_machine_type "$machine_type"
install_homebrew
if [ "$(uname)" == "Darwin" ]; then
  install_rosetta
fi
symlink_dotfiles "$machine_type"
sync_homebrew
mackup restore --force
mackup uninstall --force
```

#### Remove Script

This script will remove your dotfiles and system configuration from a machine, and uninstall your Homebrew packages and applications. To run the script you can use the following command:

```bash
chmod +x remove.sh # To make the script executable
./remove.sh
```

```bash
#!/bin/bash

# Function to print title
echo_title() {
  local title=$1
  echo ""
  echo "$title"
}

# Function to remove a symlink
remove_link() {
  local link=$1
  echo "   Removing link from $link"
  rm -f "$link"
}

# Function to remove dotfiles
remove_dotfiles() {
  echo_title "🔴 Removing Brewfile"
  remove_link "$HOME/Brewfile"

  echo_title "🔴 Removing Mackup"
  remove_link "$HOME/.mackup.cfg"
  remove_link "$HOME/.mackup"
}

# Call the function to remove dotfiles
mackup uninstall --force
remove_dotfiles
brew bundle cleanup --force
```

### Benefits of Using MackUp

- Easy to install and use
- Supports a variety of storage providers
- Supports symlinking dotfiles and system configuration
- Supports restoring dotfiles and system configuration

## Git, SSH, and GPG Keys

> [!NOTE]
> This section is under construction.

## Configuring the Terminal

> [!NOTE]
> This section is under construction.

## Setting Up Your IDE

> [!NOTE]
> This section is under construction.

## Remote Development

> [!NOTE]
> This section is under construction.
