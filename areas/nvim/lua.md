---
published: true
id: lua
slug: lua
title: Lua
summary: lua를 사용하면서 알게된 것들을 정리한다
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-28
---

# Lua
lua를 사용하면서 알게된 것들을 정리한다

## Neovim
neovim에서는 lua를 별도로 load하지 않고 바로 사용할 수 있다.

### vimscript에서 lua 실행하기
EOF로 vim 파일내에서 lua를 실행할 수 있다.

```vim
lua << EOF
  print('test')
EOF
```

### source lua file
```
:luafile %
```

### how to re source lua config
확인 더 필요함
```bash
:lua package.loaded.bashbunni = nil <cr>:source ~/.config/nvim/init.lua <cr>
```

## Literal String안에 또 다른 Literal String을 넣는 방법

lua에서는 긴 string을 표현할 때 `[[]]`를 사용한다.
근데 간혹 `[[]]`안에 또 다른 `[[]]`를 사용해야 할 때가 있다.
이 경우는 가장 바깥의 `[[]]`의 level을 올려줄 수 있다.
`[[]]`안에 =를 포함시키면 된다.


```lua
[[
  Hello World
]]
```

```lua
[=[
  Hello World
  [[Againg Hello World]]
]=]
```

[lua 문서][lua-doc]에는 다음과 같이 기술되어 있다.
> Literal strings can also be defined using a long format enclosed by long brackets. 
> We define an opening long bracket of level n as an opening square bracket followed by n equal signs 
> followed by another opening square bracket. 
> So, an opening long bracket of level 0 is written as [[, an opening long bracket of level 1 is written as [=[, and so on.


[lua-doc]: https://www.lua.org/manual/5.4/manual.html#3.1
