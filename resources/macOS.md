---
published: false
id: macOS
slug: macOS
title: MacOS
description: macOS
tags: ["resources"]
categories: ["resources"]
createdDate: 2023-08-28
updatedDate: 2024-01-23
---

# MacOS

## Increase keyboard cursor speed
keyboard speed

```bash
defaults write -g KeyRepeat -int 1 # 반복 입력속도 조절
defaults write -g InitialKeyRepeat -int 15 # 초기 입력속도 조절(기본이 25인데 바꾸지 말것)
defaults write NSGlobalDomain ApplePressAndHoldEnabled -bool false # 키 누르고 있을때 계속 입력되도록 하기
```

