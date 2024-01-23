---
published: false
id: NerdFont
slug: NerdFont
title: NErdFont
description: NerdFont
tags: ["resources"]
categories: ["resources"]
createdDate: 2023-08-10
updatedDate: 2024-01-23
---

# NerdFont

## Download with curl

```bash
# curl -fLo <저장경로/폰트명.ttf> <github-url>
curl -fLo "$HOME/Library/Fonts/JetBrainsMonoNerdFontMono-Bold.ttf" https://github.com/ryanoasis/nerd-fonts/raw/master/patched-fonts/JetBrainsMono/Ligatures/Bold/JetBrainsMonoNerdFontMono-Bold.ttf
```
- github url은 https://github.com/ryanoasis/nerd-fonts?search=1 에서 검색
- 위 명령어로 다운받으면 [Font Book](https://support.apple.com/en-gb/guide/font-book/welcome/mac)에서 User 폴더에 저장됨을 확인함

## Path
macOS에서는 여러 위치에 폰트를 저장가능
- 시스템 폰트 (/System/Library/Fonts/)
  - macOS 시스템 전체에서 사용되며, 기본적으로 사용자가 변경할 수 없음
- 로컬 사용자 폰트 (~/Library/Fonts/)
  - 해당 사용자만 사용가능
- 컴퓨터 폰트 (/Library/Fonts/) 
  - 컴퓨터에 로그인하는 모든 사용자가 사용가능

## Caveats
Font Book 애플리케이션을 사용하여 폰트를 설치할 때, 보통 로컬 사용자 폰트 디렉터리(~/Library/Fonts/)에 저장되지만, 
경우에 따라 다른 위치에 저장될 수도 있습니다.
폰트가 ~/Library/Fonts/에 없다면 /Library/Fonts/에 있을 가능성이 있습니다. 
혹은 Font Book 애플리케이션 내에서 해당 폰트를 선택한 후 "파일 정보"를 확인하면 정확한 저장 위치를 알 수 있습니다.

font cache 업데이트가 필요한 경우
```bash
fc-cache -fv
```
