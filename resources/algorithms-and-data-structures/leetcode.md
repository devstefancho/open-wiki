---
published: false
id: leetcode
slug: leetcode
title: Leetcode
summary: leetcode
toc: true
tags: ["algorithms-and-data-structures"]
categories: ["algorithms-and-data-structures"]
createdDate: 2024-01-23
updatedDate: 2024-03-31
---

# Leetcode

## leetcode-cli 적용
- https://github.com/clearloop/leetcode-cli?tab=readme-ov-file
```
cargo install leetcode-cli
```

edit `.leetcode/leetcode.toml`
```toml
[code]
editor = 'nvim'
lang = 'javascript'

[cookies]
csrf ='' # leetcode에서 Application -> Cookies -> csrf로 확인
session = '' # leetcode에서 Application -> Cookies -> LEETCODE_SESSION로 확인

[storage]
cache = 'Problems'
code = 'code' # ~/.leetcode/code 에 코드 저장됨
root = '~/.leetcode'
scripts = 'scripts'
```

```bash
leetcode edit 160 # 160번 문제 생성
leetcode test 160 # 160번 문제 테스트
```

## leetcode cli 적용방법 (deprecated)

```bash
npm -g install HwangTaehyun/leetcode-cli
leetcode version # dependency를 설치해줌
leetcode plugin -i cookie.chrome # 플러그인 추가 설치

# github으로 로그인 
# node wants to access key "Chrome Safe Storage" in your keychain
# 이라는 팝업이 뜨면 Mac 로그인시 사용하는 비밀번호 입력
leetcode user -l 

# 쿠키로 로그인 (leetcode user -l 이 안될때 사용)
# leetcode user -c가 안될때는 vscode의 leetcode extension에서 cookie로 로그인하면됨
leetcode user -c 

# logout
leetcode user -L
```

## 명령어

```bash
leetcode session # 로그인한 세션을 확인
leetcode list two # list에서 two로 검색함
```

```bash
# 3번 문제를 template과 문제내용을 담아서 javascript 언어로 생성함
# -g: generate source file
# -x: add question description in file
# -l: choose program language
leetcode show 3 -gx -l javascript 

leetcode test {파일명} -t '[1,2,3,4]' # parameter가 1개인 경우
leetcode test {파일명} -t '[1,2,3,4]\n7' # parameter가 2개인 경우
leetcode test # default test케이스로 실행함
leetcode submit {파일명} # 파일을 제출한다.
```

## leetcode cli 제거

```bash
npm -g uninstall leetcode-cli

# uninstall로 삭제가 제대로 안되는 경우, leetcode 사용경로 확인
which leetcode
sudo rm <which leetcode 경로>
```

## Trouble Shooting

### leetcode test
npm -g install leetcode-cli 대신에 [HwangTaehyun/leetcode-cli](https://github.com/HwangTaehyun/leetcode-cli) 로 설치한다.
현재 leetcode test에 버그가 있는데 기존 프로젝트가 관리가 되지 않아서 다른분의 repo로 설치해야함
(https://github.com/skygragon/leetcode-cli/issues/187)

### leetcode stat
`leetcode stat -c` 로 하면 아래 에러발생
`RangeError [ERR_OUT_OF_RANGE]: The value of "offset" is out of range. It must be an integer.`
```bash
vi /opt/homebrew/lib/node_modules/leetcode-cli/lib/commands/stat.js
# 172 line을 if (j >= 0) buf.write(MONTHS[d.month()], j|0); 로 수정
```
[링크 내용으로](https://github.com/skygragon/leetcode-cli/issues/168#issuecomment-545823896) 조건문 수정



## Ref
- [공식문서](https://skygragon.github.io/leetcode-cli/install)
- [Leetcode top problems](https://leetcode.com/problemset/all/)
