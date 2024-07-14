# Getting Your MacBook Development Ready

This guide will help you set up your MacBook for development by configuring system preferences, installing essential tools, setting up a terminal, configuring Git and SSH, and managing Node.js and Java installations.

## System Preferences

### Terminal Settings

1. **Show Library Folder**
   ```sh
   chflags nohidden ~/Library
   ```
2. **Show Hidden Files**
   ```sh
   defaults write com.apple.finder AppleShowAllFiles YES
   ```
3. **Show Path Bar**
   ```sh
   defaults write com.apple.finder ShowPathbar -bool true
   ```
4. **Show Status Bar**
   ```sh
   defaults write com.apple.finder ShowStatusBar -bool true
   ```
5. **Restart Finder**
   ```sh
   killall Finder
   ```

## Homebrew

### Install [Homebrew](https://brew.sh/)

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Update Homebrew

Update Homebrew to the latest version:

```sh
brew update
```

### Install GUI Applications

```sh
brew install --cask \
  raycast \
  bitwarden \
  google-chrome  \
  firefox \
  tor \
  iterm2 \
  visual-studio-code \
  docker \
  discord \
  vlc \
  maccy \
  protonvpn \
  ngrok
```

### Install Terminal Applications

```sh
brew install \
  wget \
  git \
  nvm \
  yarn \
  pnpm \
  commitizen
```

## Setting Up Terminal

### Make iTerm2 Default Terminal

Make iTerm2 your default terminal application.
`⌃ Control + ⇧ Shift + ⌘ Command + \`

### Install Oh My Zsh

Install [Oh My Zsh](https://ohmyz.sh/#install):

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Update Oh My Zsh

Update Oh My Zsh to the latest version:

```sh
omz update
```

```sh
source ~/.zshrc
```

### Oh My Zsh Theme and Fonts

We'll be using [`powerlevel10k`](https://github.com/romkatv/powerlevel10k):

```sh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Set `ZSH_THEME="powerlevel10k/powerlevel10k"` in `~/.zshrc`.

Restart Zsh with `exec zsh`.

On the first run, Powerlevel10k configuration wizard will ask you a few questions and configure your prompt. If it doesn't trigger automatically, type `p10k configure`.

If you are using iTerm2, `p10k configure` can install the recommended font for you. Simply answer `Yes` when asked whether to install _Meslo Nerd Font_.

Alternatively, you can manually install the MesloLGS NF font:

- [MesloLGS NF Regular.ttf](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf)
- [MesloLGS NF Bold.ttf](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf)
- [MesloLGS NF Italic.ttf](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf)
- [MesloLGS NF Bold Italic.ttf](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf)

Double-click on each file and click "Install".

Configure your terminal to use this font:

- **iTerm2**: Type `p10k configure` and answer Yes when asked whether to install _Meslo Nerd Font_. Alternatively, open _iTerm2 → Preferences → Profiles → Text_ and set _Font_ to `MesloLGS NF`.
- **Apple Terminal**: Open _Terminal → Preferences → Profiles → Text_, click _Change_ under _Font_ and select `MesloLGS NF` family.
- **Visual Studio Code**: Open _Code → Preferences → Settings_, enter `terminal.integrated.fontFamily` in the search box at the top of _Settings_ tab and set the value to `MesloLGS NF`.

### Oh My Zsh Plugins

Install the following plugins:

- [zsh-completions](https://github.com/zsh-users/zsh-completions)
- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

```sh
git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

Activate the plugins in `~/.zshrc`:

```sh
plugins=(
    # other plugins...
    zsh-autosuggestions
    zsh-syntax-highlighting
)

# needs to be added before `source $ZSH/oh-my-zsh.sh`
fpath+=${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions/src

source $ZSH/oh-my-zsh.sh
```

## Git Configuration

### Set Global Name and Email

```sh
git config --global user.name "Your Name"
git config --global user.email "you@your-domain.com"
```

### Set Default Branch to Development

```sh
git config --global init.defaultBranch development
```

### Automatically Create Upstream Branches

```sh
git config --global push.autoSetupRemote true
```

## Managing Multiple SSH Keys for GitHub

### Step 1: Generate SSH Keys

**1.1 Generate SSH Key for Personal GitHub Account**

```sh
ssh-keygen -t ed25519 -C "your_personal_email@example.com" -f ~/.ssh/id_ed25519_personal
```

**1.2 Generate SSH Key for Work GitHub Account**

```sh
ssh-keygen -t ed25519 -C "your_work_email@example.com" -f ~/.ssh/id_ed25519_work
```

### Step 2: Start SSH Agent

Start the ssh-agent in the background:

```sh
eval "$(ssh-agent -s)"
```

### Step 3: Configure SSH Config File

Open or create the `~/.ssh/config` file:

```sh
vim ~/.ssh/config
```

Add the following configurations to the file:

```sh
# Personal GitHub account
Host github.com-personal
  HostName github.com
  User git
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519_personal

# Work GitHub account
Host github.com-work
  HostName github.com
  User git
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519_work
```

### Step 4: Add SSH Keys to the SSH Agent

**4.1 Add Personal SSH Key to SSH Agent**

```sh
ssh-add ~/.ssh/id_ed25519_personal
```

**4.2 Add Work SSH Key to SSH Agent**

```sh
ssh-add ~/.ssh/id_ed25519_work
```

### Step 5: Add SSH Keys to GitHub

**5.1 Add Personal SSH Key to GitHub**
Copy the personal SSH public key to your clipboard:

```sh
cat ~/.ssh/id_ed25519_personal.pub | pbcopy
```

**5.2 Add Work SSH Key to GitHub**
Copy the work SSH public key to your clipboard:

```sh
cat ~/.ssh/id_ed25519_work.pub | pbcopy
```

### Step 6: Verify SSH Connections

**6.1 Test Personal GitHub SSH Connection**

```sh
ssh -T git@github.com-personal
```

**6.2 Test Work GitHub SSH Connection**

```sh
ssh -T git@github.com-work
```

You should see a success message from GitHub.

### Step 7: Modify Git Remote URLs

**7.1 Update Remote URL for Personal Repositories**

Navigate to your personal repository and update the remote URL:

```sh
cd path/to/your/personal/repo
git remote set-url origin git@github.com-personal:username/repo.git
```

**7.2 Update Remote URL for Work Repositories**

Navigate to your work repository and update the remote URL:

```sh
cd path/to/your/work/repo
git remote set-url origin git@github.com-work:username/repo.git
```

**7.3 Set Remote URL for New Repositories**

If setting up a new repository, use the following command:

```sh
git remote add origin git@github.com-personal:username/repo.git
```

### Step 8: Configure Git User Details

**8.1 Set User Email and Name for Personal Repositories**

Navigate to your personal repository and set the user email and name:

```sh
cd path/to/your/personal/repo
git config --local user.email "personal@example.com"
git config --local user.name "Personal Name"
```

**8.2 Set User Email and Name for Work Repositories**

Navigate to your work repository and set the user email and name:

```sh
cd path/to/your/work/repo
git config --local user.email "work@example.com"
git config --local user.name "Work Name"
```

## NVM for Node/NPM

The [node version manager (NVM)](https://github.com/nvm-sh/nvm) allows you to install and manage multiple Node.js versions. After installing it via Homebrew previously, complete the installation by running the following commands:

### Load NVM to zsh

```sh
echo "source $(brew --prefix nvm)/nvm.sh" >> ~/.zshrc
source ~/.zshrc
```

### Verify Installation

```sh
command -v nvm
```

### Install the Latest LTS Version of Node.js

```sh
nvm install --lts
```

### Verify Installation

```sh
node -v && npm -v
```

### Update npm to the Latest Version

```sh
npm install -g npm@latest
```

## Install Java Using SDKMAN

Install [SDKMAN](https://sdkman.io/):

```sh
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
```

**Verify the Installation**

```sh
sdk version
```

### Install Java with SDKMAN

Many vendors provide a JDK. To avoid licensing and support issues, use [Eclipse Temurin](https://adoptium.net/en-GB/temurin/releases/). This is an Open Source JDK that is maintained by the [Adoptium](https://adoptium.net/) project. The versions of Java on the OpenJDK Website are for testers, and the Oracle JDK is a proprietary product.

**List Available Versions**

```sh
sdk list java
```

**Install the LTS Version of Temurin**

```sh
sdk install java 21.0.3-tem
sdk install java 17.0.11-tem
```

**Set the Default Java Version**

```sh
sdk default java 21.0.3-tem
```

This concludes the guide to getting your MacBook development ready. With these steps, you should have a fully configured development environment tailored to your needs.
