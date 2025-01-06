# Ubuntu Development Setup

This guide provides step-by-step instructions to set up a development environment on Ubuntu, including `zsh`, `oh-my-zsh`, and `tmux`.

---

## Table of Contents

1. [Install Zsh](#install-zsh)  
2. [Install Curl](#install-curl)  
3. [Install Oh-My-Zsh](#install-oh-my-zsh)  
4. [Setup Plugins for Oh-My-Zsh](#setup-plugins-for-oh-my-zsh)  
   - [fzf Plugin](#install-fzf-plugin)  
   - [zsh-syntax-highlighting](#install-zsh-syntax-highlighting)  
5. [Setup Tmux](#setup-tmux)

---

## 1. Install Zsh

```bash
sudo apt install zsh -y
zsh --version
```

### Set Zsh as Default

```bash
chsh -s $(which zsh)
```

- Log out and back in for the change to take effect.  
- When prompted on opening the terminal, choose option `0`.

---

## 2. Install Curl

```bash
sudo apt install curl
```

---

## 3. Install Oh-My-Zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

---

## 4. Setup Plugins for Oh-My-Zsh

### Install fzf Plugin

```bash
sudo apt install fzf -y
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

- Press `y` to enable key bindings.  
- Press `y` to enable fuzzy auto-completion.  
- Add `fzf` to the `plugins` section in your `.zshrc` file.

---

### Install zsh-syntax-highlighting

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

- Add `zsh-syntax-highlighting` to the `plugins` section in your `.zshrc` file.

---

## 5. Setup Tmux

```bash
sudo apt install tmux -y
touch ~/.tmux.conf
```

### Tmux Configuration

Copy the following into `~/.tmux.conf`:

```bash
# Set prefix to Ctrl-Space
unbind C-b
set -g prefix C-Space
bind C-Space send-prefix

# Enable mouse support
set -g mouse on

# Mouse wheel bindings for scrolling in copy mode
bind -n WheelUpPane if-shell -F -t = "#{mouse_any_flag}" "send-keys -M" "if -Ft= '#{pane_in_mode}' 'send-keys -M' 'select-pane -t=; copy-mode -e; send-keys -M'"
bind -n WheelDownPane select-pane -t= \; send-keys -M
bind -n C-WheelUpPane select-pane -t= \; copy-mode -e \; send-keys -M
bind -T copy-mode-vi    C-WheelUpPane   send-keys -X halfpage-up
bind -T copy-mode-vi    C-WheelDownPane send-keys -X halfpage-down
bind -T copy-mode-emacs C-WheelUpPane   send-keys -X halfpage-up
bind -T copy-mode-emacs C-WheelDownPane send-keys -X halfpage-down

# Use vim keybindings in copy mode
setw -g mode-keys vi

# Bind 'y' to copy selection to clipboard using xclip
unbind -T copy-mode-vi Enter
bind-key -T copy-mode-vi Enter send-keys -X copy-pipe-and-cancel "xclip -selection c"
bind-key -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel "xclip -in -selection clipboard"
```

---

### Automatically Start or Attach to a Tmux Session

Add the following to the end of your `.zshrc` file:

```bash
if [ -z "$TMUX" ]; then
  SESSION_NAME="session-$(date +%Y%m%d%H%M%S)"
  tmux new-session -As "$SESSION_NAME"
fi
```

---

### Final Steps

Run the following to apply changes:

```bash
source ~/.zshrc
tmux source-file ~/.tmux.conf
```

---

