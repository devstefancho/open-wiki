---
published: false
id: iTerm2
slug: iTerm2
title: ITerm2
description: iTerm2
tags: ["resources"]
categories: ["resources"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Iterm2

using font-patcher is not working in iterm2
You should setup in iterm2 preference

## Step
1. download your favor from [patched-fonts](https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts) and save to any directory then double click `<any-font>.ttf`, then it will automatically install font to your system
2.  Open iTerm2.
3.  Go to the iTerm2 preferences. You can access the preferences by going to `iTerm2` > `Preferences`, or by using the keyboard shortcut `⌘,`.
4.  In the iTerm2 preferences, go to the `Profiles` tab.
5.  Select the profile you want to modify and click the `Edit Profile` button.
6.  In the profile editor, go to the `Text` tab.
7.  In the `Font` section, click the `Change Font` button.
8.  In the font selector, go to the `Custom` tab.
9.  Click the `Use a different font for non-ASCII text` checkbox.
	- `Use a different font for non-ASCII text` examples are icon and Korean
10.  Click the `Import` button and select the Nerd Font you downloaded in step 1.
11.  Select the Nerd Font in the font selector and click the `OK` button.
12.  Close the profile editor and the iTerm2 preferences.
(I choose [Jetbrain Mono](https://github.com/ryanoasis/nerd-fonts/blob/master/patched-fonts/JetBrainsMono/Ligatures/Regular/complete/JetBrains%20Mono%20Nerd%20Font%20Complete%20Mono%20Regular.ttf) Nerd Font Regular, it is recommended from someone in [reddit](https://www.reddit.com/r/linuxquestions/comments/kz8d27/what_is_your_favorite_nerd_font_if_any/))

## Screenshot
![[iterm2-nerd-font.png]]

## Notes
Check Font list in the terminal
```shell
> fc-list # all font of your system
> fc-list | grep "JetBrains" # get font from font list using regular expression
> fc-cache # clear font cache (when installed font is not working you can try this) 
> fc-cache -r # more powerful than fc-cache
```


## Ref
- [How to download nerd-font](https://www.chiarulli.me/Linux/05-nerd-fonts/)
- [How to install font](https://www.youtube.com/watch?v=LJ7CEhnS0OM)
