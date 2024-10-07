# IDE-Configs

These are my personal IDE configs for nvim, tmux, and others. This is really just for my own personal use, but if anyone finds it useful, feel free to follow along. Also any feedback is appreciated!

## Fonts

I use Hack-Mono. It has all the icons I need and looks good. I also hate ligatures so it's nice this doesn't have any.

You can go here to see how it's install on different OS: [Hack Font](https://github.com/source-foundry/Hack)

## Terminal

### Linux Terminal

For Linux I'll just user the default GNOME Terminal. It has basically everything I need.

### MacOS Terminal

On MacOS, I'll use iTerm2. The default terminal here doesn't have great font or color support and tends to break with the themes I like.

### Themes

Basically for everything, I install Catpuccin Macchiato. I just find it really nice on the eyes and I love a good pastel theme.

[iterm2](https://github.com/catppuccin/iterm)

[gnome-terminal](https://github.com/catppuccin/gnome-terminal)

I didn't do anything special

## fzf

FZF is a great fuzzy finder tool for the terminal. It's just too good to not install.

You can find it [here](https://github.com/junegunn/fzf)

Make sure you setup the shell integration so it works for ctrl-r motions. That's basically the biggest thing I use it form.

## tmux

Tmux is great, but also can cause a lot of pain if you don't set it up right. It can cause themes within your terminal to get really messed up and also isn't super intuitive.

### Setup

1. Install [tmux](https://github.com/tmux/tmux/wiki/Installing)
2. Install [tmp](git clone <https://github.com/tmux-plugins/tpm> ~/.tmux/plugins/tpm)
3. Install [catppuccin for tmux](https://github.com/catppuccin/tmux)
4. Setup your config as follows in `~/.tmux.conf`

```
unbind r
bind r source-file ~/.tmux.conf

set -g prefix C-s

set -g mouse on

bind-key h select-pane -L
bind-key j select-pane -D
bind-key k select-pane -U
bind-key l select-pane -R

set-option -g status-position top

set -g default-terminal "xterm-256color"
set-option -as terminal-overrides ",xterm-256color:Tc"
set -as terminal-overrides ',*:Setulc=\E[58::2::::%p1%{65536}%/%d::%p1%{256}%/%{255}%&%d::%p1%{255}%&%d%;m'
set -as terminal-overrides ',*:Smulx=\E[4::%p1%dm'
# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'catppuccin/tmux#v1.0.1' # See https://github.com/catppuccin/tmux/tags for additional tags

set -g @catppuccin_window_status_style "rounded"
set -g @catppuccin_window_number_position "right"

set -g @catppuccin_window_default_fill "number"
set -g @catppuccin_window_default_text "#W"

set -g @catppuccin_window_current_fill "number"
set -g @catppuccin_window_current_text "#W"

set -g @catppuccin_status_left_separator  " "
set -g @catppuccin_status_right_separator ""
set -g @catppuccin_status_fill "icon"
set -g @catppuccin_status_connect_separator "no"

set -g @catppuccin_directory_text "#{pane_current_path}"

# Run catppuccin plugin manually or through tpm
# ...

set -g status-left ""
set -g  status-right "#{E:@catppuccin_status_directory}"
set -ag status-right "#{E:@catppuccin_status_session}"

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run '~/.tmux/plugins/tpm/tpm'
```

### Notes

Some big things to note.

- We reset the prefix to `C-s` instead of `C-b`
- We set some terminal overrides so that nvim keeps any theme set by it, instead of doing weird things from the terminal. It also allows for underlining to work inside the terminal.
- We allow movement between panes using vim-esque controls. `hjkl`

## LazyVim

I use LazyVim for my text editor. I also don't do too much fancy to it other than setting up catppuccin for my theme, and a few changes to the defaults. I'll list the files I change below. To set it up, I just use install guide found [here](http://www.lazyvim.org/).

### Custom configs

#### lua/config/keymaps.lua

I like to remap my escape key to "jk" to make it a nice rolling motion without using the pinky.

```lua
vim.keymap.set("i", "jk", "<ESC>", { silent = true })
```

#### init.lua

Set the catpupucin theme

```lua
-- bootstrap lazy.nvim, LazyVim and your plugins
require("config.lazy")
vim.cmd("colorscheme catppuccin-macchiato")
```

#### plugins/colorscheme.lua

Set it here too so the plugin is installed.

```lua
return {
  { "catppuccin/nvim", name = "catppuccin", priority = 1000 },
}
```

#### plugins/lsp.lua

I like to have inlay_hints disabled by default. Then I can enable them with `leader-uh`

```lua
return {
  {
    "neovim/nvim-lspconfig",
    opts = {
      inlay_hints = { enabled = false },
    },
  }
}
```
