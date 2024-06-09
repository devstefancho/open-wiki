---
published: false
id: zsh
slug: zsh
title: Zsh
summary: zsh
toc: true
tags: ["resources"]
categories: ["resources"]
createdDate: 2024-06-09
updatedDate: 2024-06-09
---

# Zsh

## zsh tab 선택된 항목 색상 적용
https://stackoverflow.com/questions/29196718/zsh-highlight-on-tab 를 참고하였다.
```
zstyle ':completion:*' menu select
```

## ctrl-m으로 실행하지 않고 항목 선택하기

Caveat: zsh에서 ctrl-m 키에 대해서 설명한다. 이게 정확히 어떤 키인지 zsh에서만 되는건지는 몰라서 여기에 적는게 맞는지는 모르겠다.

ctrl-m의 경우 tab에 나온리스트에서 선택항목을 완전히 선택할 때 유용하다. 확인한바로는 iTerm2와 alacritty에서는 동일하게 동작한다.
예를들어, cat을 하고 tab을 누르면 선택가능한 파일, 디렉토리가 나오는데, 하나를 선택하고 엔터를 누르면 실행이 되게 된다.
파일이 있는 디렉토리에서 cat을 하면 상관없지만, 디렉토리 내부를 더 들어가야하면 바로 실행하면 안된다.
```bash
cat foo/bar/hello.json # 이 경우 cat foo/ 에서 엔터를 눌렀을때 실행되지 않아야한다.
```

이때 방법은 cat foo까지 선택되었을때, `/` 키를 누르면 되지만, 뭔가 선택을 위해서 `/`를 누르는건 어색하다고 생각되었다.
chatgpt에게 물어보니 ctrl-m을 말해줬다.

```bash
# cat을 입력하고 tab을 누르면 아래와 같이 선택가능한 파일, 디렉토리가 나온다.
$ cat
foo foo2 foo3

# tab을 눌러 foo를 선택한 상태에서 ctrl-m을 누르니 foo 디렉토리가 완전히 선택되었다.
$ cat foo

# 이 상태에서 tab을 누르니 아래와 같이 다시 자동완성리스트들이 나오고 선택할 수 있게 되었다.
cat foo/
bar bar2 bar3
```

근데 이때 cat foo 까지 직접 입력을 하고 ctrl-m을 누르면 Enter와 동일하게 실행이 되게 된다.
따라서 자동완성에서 선택항목을 완전히 선택할때만 ctrl-m을 사용하도록 한다.
