---
published: false
id: git
slug: git
title: Git
description: git
tags: ["resources"]
categories: ["resources"]
createdDate: 2024-01-23
updatedDate: 2024-01-24
---

# Git

## message
git message is for message consistency

```bash
vim ~/.gitmessage.txt # add git message config
git config --global commit.template ~/.gitmessage.txt # set git message as a template 
```

## log

`-p`는 변경된 내용을 보여줌
`--follow`는 파일의 경로가 변경되어도 변경 이력을 보여줌

```bash
git log --follow -p -- [file] # 파일의 변경 이력 확인 (경로 변경은 무시)
git log -p [file] # 파일의 변경 이력 확인
git log -n 500 --oneline # 최근 500개의 commit을 한 줄로 보여줌
git log origin/[BRANCH_NAME]..[BRANCH_NAME] # 로컬에는 있고 리모트에는 없는 커밋들 확인
git log [BRANCH_NAME]..origin/[BRANCH_NAME] # 리모트에는 있고 로컬에는 없는 커밋들 확인
git log [COMMIT_HASH]..[COMMIT_HASH] # commit 끼리 비교
git log [COMMIT_HASH]..[COMMIT_HASH] --author="John" # commit 끼리 비교, author 필터(일부만 들어가도 됨)
```

## show

```bash
git show [commit] # commit의 변경 내용 확인
git show --name-only # filename only
```

## reset

```bash
git reset --hard origin/main # ~/.config main ⇣2⇡1 이런식으로, local과 remote의 차이가 있는 경우 remote로 동기화하기
```

## config
```bash
git config --global user.name "조성진 (LGU/아이들나라Frontend팀)"
# 아래 이메일주소 패턴 관련 (https://github.com/orgs/community/discussions/41101)
git config --global user.email 1012345678+devstefancho-mydomain@users.noreply.github.com

# 저장소 설정 제거
git config --unset user.name
git config --unset user.email

# config 값 확인하기
git config --global user.email

# config 내용확인하기
cat ~/.gitconfig

# 전체 설정확인하기
git config --local -l

# upstream을 current로 지정하기
git config --global push.default current

# global editor 변경
git config --global core.editor "nvim"
```

## describe

```bash
git describe --tags --abbrev=0 # latest tag 확인하기
```

## tag

```bash
git tag v1.0.0 # tag 생성하기
git push origin v1.0.0 # tag push
```

## stash

[how-to-restore-git-stash](https://stackoverflow.com/questions/89332/how-do-i-recover-a-dropped-stash-in-git)
```bash
git stash apply $stash_hash # Once you know the hash of the stash commit you dropped, you can apply it as a stash
git branch recovered $stash_hash # Or, you can create a separate branch for it with
```

## worktree

```bash
git worktree add ../feat/readingtf-1261 -b feat/readingtf-1261/english-toast
git worktree add ../feat/readingtf-1261 -b feat/readingtf-1261/english-toast
```



## github notification setup in Slack

- https://github.com/integrations/slack#readme

### Usages
```bash
/github signin # login 하기
/github subscribe <owner>/<repo> reviews comments # 알림방식 추가
/github subscribe list # 구독중인 리스트
```



## github gpg 인증

### 인증키 추가하기
```bash
gpg --full-generate-key # key 생성하기
gpg --armor --output mykey.asc --export 142373285+devstefancho-mydomain@users.noreply.github.com # mykey.asc에 키 생성하기
gh auth refresh -s write:gpg_key # gpg key 등록할수 있도록 권한설정
gh gpg-key add mykey.asc # github에 gpg키 등록하기(https://cli.github.com/manual/gh_gpg-key_add)

git log --show-signature # 시그니쳐 확인하기
gpg --list-secret-keys --keyid-format=long | pbcopy # key 확인하기

git config --global user.signingkey 89EC458DD21ABCDE # sign key 적용하기
git config --global commit.gpgsign true # commit에 gpgsign 설정하기
cat ~/.gitconfig # 결과 확인

gpg --edit-key BC063F7388FFFFFF # 키 수정하기
```
- https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key
- https://docs.github.com/en/authentication/managing-commit-signature-verification/about-commit-signature-verification
- https://www.lainyzine.com/ko/article/how-to-set-git-repository-username-and-email/

### 환경변수 export
.zshrc 첫번째 line에 아래 코드를 넣는고 `source ~/.zshrc`를 한다
```bash
export GPG_TTY=$(tty)
```

### Passphrase
commit할때 passphrase를 입력하라고 나오는데, 나는 깃헙 password로 설정하였다.(Mac의 Passwords에 저장해둠)


### cache 시간 변경하기
기본 cache 시간이 10분이기 때문에, 계속 passphrase를 입력해줘야 하는 불편함이 있다.
그래서 기본 시간을 변경해주도록 한다

```bash
vi ~/.gnupg/gpg-agent.conf # 파일이 없다면 생성해준다.
## 아래 내용을 입력하고 저장한다.
# default-cache-ttl 3600
# max-cache-ttl 7200

gpgconf --reload gpg-agent # reload 해준다.
```



## github ssh 인증

### link
- [ssh 생성하기](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent)
- [github에 등록하기](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

### step
```bash
 ls -al ~/.ssh # ssh 키가 있는지 확인
 ssh-keygen -t ed25519 -C "devstefancho@mydomain.co.kr" # 없다면 key 생성
 touch ~/.ssh/config # 없는 경우 생성
 vi ~/.ssh/config # 수정하기(내용은 link의 ssh 생성하기 링크 참고)
 ssh-add --apple-use-keychain ~/.ssh/id_ed25519 # keychain에 등록하기
 pbcopy < ~/.ssh/id_ed25519.pub # public key 복사 후 https://github.com/settings/keys에서 SSH 생성하기
 
 # git push시에 로그인 하라는 메시지가 뜨는 경우 현재 remote가 https로 되어있어서 그런것이므로 ssh로 변경필요
 git remote -v # remote 주소확인
 git remote set-url origin git@github.com:<owner>/<repo>.git # ssh 주소로 변경
 gp # git push
```

### trouble shooting
git push에서 아래와 같은 error 메시지가 뜨는 경우 yes를 해주면 된다.
```
The authenticity of host 'github.com (20.200.245.247)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```



## 1개 디바이스에 여러개 ssh 등록하기

### step

1. ssh 생성 및 추가
  ```bash
  ssh-keygen -t rsa -b 4096 -C "your-email@example.com" -f ~/.ssh/id_rsa_newaccount # 이미 id_rsa 이름이 있다면 다른 파일명으로 지정해줘야함
  ssh-add ~/.ssh/id_rsa_newaccount # ssh 키 추가
  ```

2. 새로 생성된 공개 키 (id_rsa_newaccount.pub)의 내용을 복사하여 GitHub 계정의 SSH 키 설정에 추가합니다.
3. ~/.ssh/config 수정
  ```
  # 기본 계정
  Host github.com
      HostName github.com
      User git
      IdentityFile ~/.ssh/id_rsa

  # 새 계정
  Host github-newaccount
      HostName github.com
      User git
      IdentityFile ~/.ssh/id_rsa_newaccount
  ```
4. clone test 해보기 
  ```
  git clone git@github-newaccount:username/repository.git
  ```
5. ssh 연결 확인하기
  ```bash
  # 매칭되는 이메일이 결과에 나와야한다.
  ssh -T git@github.com # 명령을 실행했을 때 "Hi {id_rsa에 등록된 email주소}! You've successfully 
  ssh -T git@github-newaccount.com # 명령을 실행했을 때 "Hi {id_rsa에 등록된 email주소}! You've successfully 
  ```

### trouble shooting
Permission Deny 혹은 not found(404)가 발생한다면, ssh-add 된것을 다 지우고 다시 .ssh/config에 추가한 순서대로 ssh-add를 해준다.
```bash
eval "$(ssh-agent -s)" # agent 초기화
ssh-add -D # ssh-add 된것들 모두삭제
ssh-add ~/.ssh/id_rsa # .ssh/config에 있는 첫번째 항목 추가
ssh-add ~/.ssh/id_rsa_mydomain # 두번째 항목추가
```



## github slack

- [Link](https://github.com/integrations/slack)

### Example
```bash
/github subscribe list features # 구독중인 리스트들
/github subscribe owner/repo comments # 구독하기
/github unsubscribe owner/repo commits # commit은 구독하지 않기
/github unsubscribe owner/repo issues pulls commits releases deployments # default로 들어가는 것들 구독하지 않기
```

