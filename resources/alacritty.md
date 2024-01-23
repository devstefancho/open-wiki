---
published: false
id: alacritty
slug: alacritty
title: Alacritty
description: alacritty
tags: ["resources"]
categories: ["resources"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Alacritty

## Install - 방법1
https://github.com/alacritty/alacritty/releases
설치 - 최신 Release에서 dmg 파일을 다운로드 및 실행 -> Application 으로 Alacritty 이동
경로 - `.config/alacritty/alacritty.yml`로 저장해야지 설정이 적용되었다. (~/alacritty/alacritty.yml은 안되었음)

## Install - 방법2
```
brew install --cask alacritty
```

## Font Config
font의 경우 fc-list로 검색한 폰트를 직접 넣어줘야했다.
```shell
fc-list | grep Jet 
```
output은 아래처럼 나온다. 여기서 JetBrainsMono Nerd Font Mono가 실제 폰트 이름이다.
`/Users/stefancho/Library/Fonts/JetBrains Mono Nerd Font Complete Mono Regular.ttf: JetBrainsMono Nerd Font Mono:style=Regular`

Font download
- https://www.nerdfonts.com/font-downloads

## Config
```yml
font:
  # Normal (roman) font face
  normal:
    family: JetBrainsMono Nerd Font Mono
    # The `style` can be specified to pick a specific face.
    style: Regular

  # Bold font face
  bold:
    family: JetBrainsMono Nerd Font Mono
    # The `style` can be specified to pick a specific face.
    style: Bold

  # Italic font face
  italic:
    family: JetBrainsMono Nerd Font Mono
    # The `style` can be specified to pick a specific face.
    style: Italic

  # Point size
  size: 12
```

## Multi Tab
> MacOS users can add tabs using Prefer tabs when opening documents
Setting -> Desktop & Dock -> Windows -> Prefer tabs when opening documents -> Always

## alacritty cursor blink 설정

- [github issue](https://github.com/alacritty/alacritty/issues/302)

```
cursor:
  style:
    shape: Beam
    blinking: Always
  vi_mode_style:
    shape: Block
    blinking: Always
  blink_interval: 500
  blink_timeout: 10 # stop after 10 times blinking
```

## Trouble Shooting
### Alacritty 한글입력 문제해결
한글 delete시에 키를 더 많이 눌러야하는 문제점

```
강
이라고 입력을 하고나서 delete로 지울때 세번이면 모두 삭제되어야함
강 -> 가 -> ㄱ -> 다 지워짐

근데 총 4네번을 해야다 지워짐
강 -> 가 -> ㄱ -> 직사각형 커서에서 얇은 줄 형태의 커서가 되면서 오른쪽에 붙음 -> 다 지워짐

직사각형 커서에서 얇은 줄 형태의 커서가 되면서 오른쪽에 붙음
이게 무슨말이냐면 (| -> 이게 커서라고 치면)
ㄱ| 
이렇게 됨
```

#### 해결방법
Alacritty 제거 및 재설치 (2023/07/19)
```
brew install --cask alacritty
```

### alacritty 한글입력 후 특수문자 입력했을때 키 씹히는 현상
- 깃헙이슈(https://github.com/alacritty/alacritty/issues/6942) 참고

#### 해결방법: 구름입력기를 설치
1. `brew install --cask gureumkim`
2. PC 리부팅
3. setting -> keyboard -> input source -> `+-` -> 구름입력기 추가

### 같은 자리에서 커서입력이 계속 되는 문제
간헐적으로 발생하며 한글입력을 하고 있으면 생각보다 꽤 자주 발생한다.
빠져나오는 방법은 space를 누르거나 delete를 하면 되는데, space를 누르는게 가장 직관적인 방법인거 같다.
재현도 잘안되어서 issue에 등록하기도 어렵다.
한글입력 할때만 다른 에디터 쓸까 했는데, 에디터 왔다갔다하는게 더 귀찮은 일이니까  그냥 번거롭더라도 space로 빠져나오도록 하자.

#### 해결방법
alacritty 0.13으로 업데이트 하니까 해결됨 (0.12.2, 0.12.3에서는 발생했었음)
