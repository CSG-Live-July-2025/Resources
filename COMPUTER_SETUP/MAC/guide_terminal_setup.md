# macOS Terminal Setup Guide

## iTerm2 Installation (macOS)

iTerm2 is a powerful terminal alternative for macOS, offering advanced features like split panes, themes, and customization options.

### Step 1: Download and Install iTerm2 (Optional)
1. Go to the [iTerm2 website](https://iterm2.com/).
2. Click **Download** and choose the latest stable release.
3. Once downloaded, open the `.zip` file and move **iTerm2.app** to your **Applications** folder.

### Step 2: Open and Configure iTerm2 (Optional)
1. Open **iTerm2** from your Applications folder.
2. Explore customization options by going to **iTerm2 -> Preferences**, where you can adjust color schemes, set up split panes, and more.

*Optional Benefits of iTerm2*:
   - **Split Panes**: Open multiple terminal sessions within a single window.
   - **Customizable Appearance**: Change themes, colors, fonts, and transparency.
   - **Profiles and Session Logging**: Set different configurations for different tasks and maintain a log of sessions.

---

## macOS Terminal Configuration

### Step 1: Open the Terminal or iTerm2
1. Open **Finder**, go to **Applications -> Utilities -> Terminal.app**.

### Step 2: Set `zsh` as the Default Shell
1. Run the following command to set `zsh` as your default shell:
   ```bash
   chsh -s /bin/zsh
   ```
   - Enter your password if prompted.

### Step 3: Configure Colors, Autocompletion, and Aliases
1. Run the following commands to add color settings, set up aliases, and configure autocompletion:
   ```bash
   echo "export CLICOLOR=1" >> ~/.zshrc
   echo "export LSCOLORS=GxFxCxDxBxegedabagaced" >> ~/.zshrc
   echo "export EDITOR='code -w'" >> ~/.zshrc
   echo "" >> ~/.zshrc
   echo "alias ls='ls -GFh'" >> ~/.zshrc
   echo "" >> ~/.zshrc
   echo "PROMPT='%F{cyan}%~%f %# '" >> ~/.zshrc
   echo "autoload -Uz compinit && compinit" >> ~/.zshrc
   echo "zstyle ':completion:*' matcher-list '' 'm:{a-zA-Z}={A-Za-z}'" >> ~/.zshrc
   echo "zstyle ':completion:*' menu select" >> ~/.zshrc
   echo "zstyle ':completion:*' completer _expand_alias _complete _ignored" >> ~/.zshrc
   echo "gem: --no-document" >> ~/.gemrc
   echo 'Done!'
   ```
2. Restart your terminal or apply the changes immediately with:
   ```bash
   source ~/.zshrc
   ```

---

## Optional: Install oh-my-zsh for Enhanced Themes and Plugins

To further customize your terminal, you can install **oh-my-zsh** to access additional themes and plugins.

1. Install **oh-my-zsh** by running:
   ```bash
   sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
   ```
2. After installing, open `~/.zshrc` and set a theme by updating the `ZSH_THEME` line (e.g., `ZSH_THEME="agnoster"` or `ZSH_THEME="robbyrussell"`).
3. Restart your terminal or run `source ~/.zshrc` to see the new theme.

This optional setup provides you with advanced themes and plugins that enhance your terminal’s functionality and visual appeal.
