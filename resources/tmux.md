---
published: false
id: tmux
slug: tmux
title: Tmux
description: tmux
tags: ["resources"]
categories: ["resources"]
createdDate: 2023-08-17
updatedDate: 2024-01-23
---

# Tmux

> My prefix is `Ctrl + a`


## Basic

### with prefix
| key                           | desc                                                  |
|-------------------------------|-------------------------------------------------------|
| :new                          | create new session                                    |
| $                             | rename session                                        |
| w                             | window list                                           |
| ( or )                        | switch session                                        |
| $                             | change session name                                   |
| L                             | go to latest session                                  |
| ,                             | rename window                                         |
| z                             | zoom on/off window                                    |
| link-window -t <session-name> | create link window                                    |
| move-window -t <session-name> | move current window to target session                 |
| ?                             | show binding list                                     |
| [                             | enter to copy mode                                    |
| ]                             | paste clipboard text                                  |
| Space                         | Arrange the current window in the next preset layout. |
| list-keys                     | all keybindings                                       |
| o                             | select next pane                                      |

### cli
```bash
tmux move-window -t mySession:3 # move to specific window in session
```


## Split window from current path

```bash
tmux display-message -p "#{pane_current_path}" # 현재 경로 확인하기
bind s split-window -v -c "#{pane_current_path}" \; resize-pane -y 10 # pane 분리하고 resize하기
```

tmux.conf에 아래와 같이 추가하면 됨
```
bind v split-window -h -c "#{pane_current_path}"
bind s split-window -v -c "#{pane_current_path}"
```


## Vi Mode

| key   | desc                                                                                                                                |
|-------|-------------------------------------------------------------------------------------------------------------------------------------|
| Space | start to visual block                                                                                                               |
| Enter | copy to clipboard                                                                                                                   |
| v     | press v either before or after space to visual block mode (https://superuser.com/questions/395158/tmux-copy-mode-select-text-block) |


## Remapping tmux Pane Zoom to Alt+Z

`-n` : no prefix

```bash
bind-key -n M-z resize-pane -Z # zoom in/out을 alt+z로 바꾸기
bind-key -n M-o select-pane -t :.+ # :는 현재창, +는 다음 pane 즉 현재창의 다음 pane을 선택
```

## Copy using mouse
press shift key and drag, then cmd + c for copy
- https://unix.stackexchange.com/questions/332419/tmux-mouse-mode-on-does-not-allow-to-select-text-with-mouse


## open hyper link
shift key and mouse left click
- https://www.reddit.com/r/tmux/comments/sv6skh/clickable_urls/
