---
layout: post
title: "My tmux Config"
date: 2014-01-15 07:35
comments: true
categories:
---
## My tmux Config

Feel free to use as you like...

```bash
# use vi mode
setw -g mode-keys vi

# remap prefix to Control + a
#set -g prefix C-a
#unbind C-b
#bind C-a send-prefix

# force a reload of the config file
#unbind r
#bind r source-file ~/.tmux.conf

# quick pane cycling with Ctrl-a
#unbind ^A
#bind ^A select-pane -t :.+

# move around panes like in vim (only in tmux 1.6)
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R
bind h select-pane -L

# Sane scrolling
set -g mode-mouse on

# color options
#set-window-option -g utf8 on
#set -g default-terminal screen-256color

# colors
setw -g mode-bg black
set-option -g default-terminal "screen-256color" #"xterm-256color" # "screen-256color"
set-option -g pane-active-border-fg blue

# Titles (window number, program name, active (or not)
set-option -g set-titles on
set-option -g set-titles-string '#H:#S.#I.#P #W #T'
#set -g status-bg colour240

# disable mouse control by default - change 'off' to 'on' to enable by default.
setw -g mode-mouse on
set-option -g mouse-resize-pane off
set-option -g mouse-select-pane off
set-option -g mouse-select-window off
# toggle mouse mode to allow mouse copy/paste
# set mouse on with prefix m
bind m \
    set -g mode-mouse on \;\
    set -g mouse-resize-pane on \;\
    set -g mouse-select-pane on \;\
    set -g mouse-select-window on \;\
    display 'Mouse: ON'
# set mouse off with prefix M
bind M \
    set -g mode-mouse off \;\
    set -g mouse-resize-pane off \;\
    set -g mouse-select-pane off \;\
    set -g mouse-select-window off \;\
    display 'Mouse: OFF'
# zoom this pane to full screen
bind + \
    new-window -d -n tmux-zoom 'clear && echo TMUX ZOOM && read' \;\
    swap-pane -s tmux-zoom.0 \;\
    select-window -t tmux-zoom
# restore this pane
bind - \
    last-window \;\
    swap-pane -s tmux-zoom.0 \;\
    kill-window -t tmux-zoom


#### COLOUR (Solarized 256)

# default statusbar colors
set-option -g status-bg colour235 #base02
set-option -g status-fg colour136 #yellow
set-option -g status-attr default

# default window title colors
set-window-option -g window-status-fg colour244 #base0
set-window-option -g window-status-bg default
#set-window-option -g window-status-attr dim

# active window title colors
set-window-option -g window-status-current-fg colour166 #orange
set-window-option -g window-status-current-bg default
#set-window-option -g window-status-current-attr bright

# pane border
set-option -g pane-border-fg colour235 #base02
set-option -g pane-active-border-fg colour240 #base01

# message text
set-option -g message-bg colour235 #base02
set-option -g message-fg colour166 #orange

# pane number display
set-option -g display-panes-active-colour colour33 #blue
set-option -g display-panes-colour colour166 #orange

# clock
set-window-option -g clock-mode-colour colour64 #green

bind-key J command-prompt -p "join pane from:"  "join-pane -s '%%'"
bind-key S command-prompt -p "send pane to:"  "join-pane -t '%%'"
```
