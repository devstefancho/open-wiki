---
published: true
id: git
slug: git
title: Git
summary: Git과 관련된 명령어와 Github 인증방식 그리고 Slack 연동등에 대해서 정리
toc: true
tags: ["resources"]
categories: ["resources"]
createdDate: 2024-01-23
updatedDate: 2024-02-21
---

# Git

Git의 명령어는 자세하게 알수록 과거 이력을 추적하는데에 도움을 준다.

추가적으로 Github 인증방식 그리고 Slack 연동등에 대해서 정리해보았다.
GPG로는 사용자 서명을 남긴다. SSH는 Github에 접속하기 위함이다. (push, pull을 하기 위함)
git-user는 사용자를 스위치하기 위함이다. git-user는 서명과 user.name, user.email은 변경해주지만
SSH등록은 git-user와 별개로 처리해줘야한다.
여러계정으로 push, pull을 하기 위해서는 SSH 등록도 계정개수만큼 해줘야한다.
SSH로 등록된 계정은 각각의 remote 주소를 갖는다. `git@github.com:<owner>/<repo>.git`,
`git@github-devstefancho.com:<owner>/<repo>.git` 이런식으로 해당 ssh 키에 맞는 remote 주소를
등록해주어야한다.


## Git 기본명령어
### message
git message는 message의 일관성을 위해서 등록해둔다.

```bash
vim ~/.gitmessage.txt # add git message config
git config --global commit.template ~/.gitmessage.txt # set git message as a template 
```

### log
- log를 보는 방법은 여러가지가 있다.
  - `-p`는 변경된 내용을 보여줌
  - `--follow`는 파일의 경로가 변경되어도 변경 이력을 보여줌

```bash
git log --follow -p -- [file] # 파일의 변경 이력 확인 (경로 변경은 무시)
git log -p [file] # 파일의 변경 이력 확인
git log -n 500 --oneline # 최근 500개의 commit을 한 줄로 보여줌
git log origin/[BRANCH_NAME]..[BRANCH_NAME] # 로컬에는 있고 리모트에는 없는 커밋들 확인
git log [BRANCH_NAME]..origin/[BRANCH_NAME] # 리모트에는 있고 로컬에는 없는 커밋들 확인
git log [COMMIT_HASH]..[COMMIT_HASH] # commit 끼리 비교
git log [COMMIT_HASH]..[COMMIT_HASH] --author="John" # commit 끼리 비교, author 필터(일부만 들어가도 됨)
```

### show
- 변경내역을 상사하게 볼때 사용한다.
```bash
git show [commit] # commit의 변경 내용 확인
git show --name-only # filename only
```

### reset
- 현재 commit을 리셋하거나 remote 상태로 동기화 시킬때 사용한다.
```bash
git reset --hard origin/main # ~/.config main ⇣2⇡1 이런식으로, local과 remote의 차이가 있는 경우 remote로 동기화하기
git reset HEAD~1 # 바로 직전 commit을 리셋한다.
git reset {commit-hash} # 해당 commit 까지는 유지하고, 이후 커밋들을 리셋한다.
```

### config
- user 정보를 설정할때 사용한다.
```bash
git config --global user.name "조성진"
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

### describe
- 가장 최신 tag를 확인하는 방법
```bash
git describe --tags --abbrev=0
git tag | grep -v '_rc' | sort -V | tail -n 1
```

### tag
- tag 생성 및 push 하기
```bash
git tag v1.0.0 # tag 생성하기
git push origin v1.0.0 # tag push
```

### stash
- [how-to-restore-git-stash](https://stackoverflow.com/questions/89332/how-do-i-recover-a-dropped-stash-in-git)
```bash
git stash apply $stash_hash # drop 처리한 stash commit을 알고 있다면, stash로 다시 적용할 수 있다.
git branch recovered $stash_hash # 또는 이 stash commit으로부터 새로운 branch로 분기시킬 수도 있다.
```

### worktree

```bash
git worktree add {경로} -b {새 브랜치 이름}
```


## Slack 연동

- Github notification을 [Slack에 연동](https://github.com/integrations/slack#readme)하는 방법이다.

### Usages
```bash
/github signin # login 하기
/github subscribe list features # 구독중인 리스트들
/github subscribe owner/repo comments reviews # comment와 review에 대해서 구독하기
/github unsubscribe owner/repo commits # commit은 구독하지 않기
/github unsubscribe owner/repo issues pulls commits releases deployments # default로 들어가는 것들 구독하지 않기
```

## GPG 인증

GPG 인증은 commit이나 tag에 서명하여 해당 commit, tag가 특정개인에 의해 생성되었음을 증명해준다.
github에서 commit옆에 초록색  태그가 있는 경우가 GPG 인증이 되었음을 의미하는 것이다.

### 인증키가 이미 있는 경우 Github에 등록하기
먼저 현재 등록되어 있는 key들에 대해서 확인해보자
현재 등록된 키가 있다면 아래명령어의 결과값으로 GPG Key값을 확인할 수 있다.
만약없다면 인증키를 생성하면 된다.
나는 gh cli를 사용해서 등록하였다.
등록을 마쳤으면 Settings > SSH and GPG keys에서 등록된 키를 확인할 수 있다.
```bash
gpg --list-secret-keys --keyid-format=long # gpg key 확인하기
gpg --armor --output mykey.asc --export {GPG Key} # GPG Key로 asc 파일 생성하기
gh auth refresh -s write:gpg_key # gh-cli를 사용하여 gpg key 등록할수 있도록 권한설정
gh gpg-key add mykey.asc # github에 gpg키 등록하기(https://cli.github.com/manual/gh_gpg-key_add)
```

list-secret-keys로 출력되는 값은 아래와 같고 각 항목에 대해 간략히 설명해보았다.
```
sec   rsa4096/{GPG Key} {등록일 YYYY-MM-DD} [SC]
      {Finger Print}
uid                 [ultimate] {이름} <{메일주소}>
ssb   rsa4096/{Sub Key} {등록일 YYYY-MM-DD} [E]
```
- `{}`부분은 변수값
- sec는 secret key
- rsa4096은 유형과 길이
- SC는 사용목적을 나타내고 S는 Sign(서명), C는 Certify(암호화)를 의미
- Finger Print는 키의 전체 해시 값
- uid 는 user id를 의미하고, 키 소유자의 이름과 이메일 주소가 포함됨
- ssb는 subkey를 나타내고, 특정목적에 사용되는 추가키임
  - E는 Encryption으로 암호화를 의미하고, 이 subkey가 암호화에 사용될 수 있음을 나타냄

### 인증키 신규생성하기

```bash
gpg --full-generate-key # key 생성하기
```
위 명령어를 입력하면 key를 생성할 수 있다. 나는 옵션을 아래와 같이 선택하였다.
- RSA and RSA 선택
- 4096 선택
- 0 선택 (not expired)
- Real Name: commit에 표시될 이름 설정
- Email Address : email 주소 설정 (noreply 메일로 설정해도 된다)
- Comment : 추가 정보인것 같은데 빈값으로 두어도 된다

### mykey.asc로 변환하기
- 생성한 GPG키를 Github에 등록하기 위해서 ASCII-armor 방식으로 변환한다.
- noreply 이메일은 계정은 Github > settings > Emails > Primary email address 에서 확인할 수 있다.
```bash
gpg --armor --output mykey.asc --export 142373285+devstefancho-mydomain@users.noreply.github.com
gh auth refresh -s write:gpg_key # gh-cli를 사용하여 gpg key 등록할수 있도록 권한설정
gh gpg-key add mykey.asc # github에 gpg키 등록하기(https://cli.github.com/manual/gh_gpg-key_add)
```

### GPG 키 수정, 삭제하기
```bash
gpg --edit-key ABC1234567890ABC # 키 수정하기
gpg --delete-secret-key ABCDEFG012345 # 키 삭제하기
```

### 설정된 값 확인하기
```bash
git log --show-signature # commit에서 GPG 서명을 확인할 수 있다. (merge commit에는 서명되지 않는것 같음)
git config --global user.signingkey # 현재 서명값 확인하기
git config --global user.signingkey {GPG Key 값} # 서명값 다시 설정하기
git config --global commit.gpgsign # commit에 서명유무에 대해 확인
git config --global commit.gpgsign true # commit에 서명하는 것에 대해서 true로 설정
cat ~/.gitconfig # 결과 확인
```

### 환경변수 export
- GPG를 정상적으로 사용하기 위해서는 환경변수를 설정해줘야한다.
- .zshrc 첫번째 line에 아래 코드를 넣고 `source ~/.zshrc`를 한다.
```bash
export GPG_TTY=$(tty)
```

### Passphrase 설정
commit할때 passphrase를 입력하라고 나오는데, 나는 깃헙 password로 설정하였다.(Mac의 Passwords에 저장해둠)


### Passphrase cache 설정 변경하기
기본 cache 시간이 10분이기 때문에, 계속 passphrase를 입력해줘야 하는 불편함이 있다.
그래서 기본 시간을 변경해주도록 한다

```bash
vi ~/.gnupg/gpg-agent.conf # 파일이 없다면 생성해준다.
## 아래 내용을 입력하고 저장한다.
# default-cache-ttl 3600
# max-cache-ttl 7200

gpgconf --reload gpg-agent # reload 해준다.
```

## SSH 인증

SSH 인증은 로컬 Git 클라이언트에서 SSH를 사용하여 Github 저장소에 접근하는 방식이다.
Github 서버에 안전하게 접근하고 push, pull 하기 위해서 사용된다.
인증에는 public key, private key 두개가 쌍으로 필요한데, public key는 Github에 등록하고
이후에 Github에 접속할때 사용자의 private key로 자동인증을 하게 된다.

- [ssh 생성하기](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent)
- [github에 등록하기](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

### Steps

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

### Trouble shooting
git push에서 아래와 같은 error 메시지가 뜨는 경우 yes를 해주면 된다.
```
The authenticity of host 'github.com (20.200.245.247)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```



## 1개 디바이스에 여러개의 ssh 등록하기

### Steps

1. ssh 생성 및 추가
  ```bash
  # 이미 id_rsa 이름이 있다면 다른 파일명으로 지정해줘야함
  ssh-keygen -t rsa -b 4096 -C "your-email@example.com" -f ~/.ssh/id_rsa_newaccount
  ssh-add ~/.ssh/id_rsa_newaccount # ssh 키 추가 (newaccount는 본인에 맞게 바꾼다.)
  ```

2. 새로 생성된 공개 키 (~/.ssh/id_rsa_newaccount.pub)의 내용을 복사하여 GitHub 계정의 SSH 키 설정에 추가한다.
  - 이때 Github 계정은 1번에서 email 설정한 계정으로 접속하면 된다.
  - Settings > SSH and GPG Keys에서 New SSH key로 등록해준다.
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
- Host의 github-newaccount는 별칭이다.
- HostName은 실제 github 주소를 의미한다.
- User git은 ssh연결에 사용되는 사용자명이다.
- IdentityFile은 위에서 생성한 ssh 키의 경로이다.

  
5. push 해보기
  ```bash
  # 신규로 clone 받는 경우
  git clone git@github-newaccount:username/repository.git
  
  # 이미 clone은 받아져 있어서, origin 경로만 바꾸는 경우
  git remote set-url origin git@github-newaccount:{Owner}/{Repository Name}.git
  ```
6. ssh 연결 확인하기
  ```bash
  # 매칭되는 이메일이 결과에 나와야한다.
  ssh -T git@github.com # 명령을 실행했을 때 "Hi {id_rsa에 등록된 email주소}! You've successfully 
  ssh -T git@github-newaccount.com # 명령을 실행했을 때 "Hi {id_rsa에 등록된 email주소}! You've successfully 
  ```

### Permission Deny 혹은 not found(404)가 발생하는 경우
ssh-add 된것을 다 지우고 다시 .ssh/config에 추가한 순서대로 ssh-add를 해준다.
```bash
eval "$(ssh-agent -s)" # agent 초기화
ssh-add -D # ssh-add 된것들 모두삭제
ssh-add ~/.ssh/id_rsa # .ssh/config에 있는 첫번째 항목 추가
ssh-add ~/.ssh/id_rsa_mydomain # 두번째 항목추가
```

### not found 404가 발생하는 경우
우선 github에 해당 ssh키의 pub키가 등록되어있는지 확인이 필요하다.
`.ssh/id_rsa_mydomain.pub | pbcopy` 이렇게 복사하여 이 값을
Github > Settings > SSH and GPG Keys에서 New SSH Key로 등록해본다.
이미 ssh키가 있는 경우 중복 등록은 안된다.
ssh-add list를 확인하도록 한다. 

```bash
### .ssh/config에 아래와 같이 두개의 ssh가 등록되어 있는경우 ###
# ------------------------------------------
# Host github.com
#   AddKeysToAgent yes
#   UseKeychain yes
#   IdentityFile ~/.ssh/id_ed25519
# 
# Host github-dev
#     HostName github.com
#     User git
#     IdentityFile ~/.ssh/id_rsa_mydomain
# ------------------------------------------

ssh-add -l # list 확인
ssh-add ~/.ssh/id_rsa_mydomain # 리스트에 없다면 새로 추가
ssh -T git@github-dev # ssh 연결확인
```

## git-user로 계정 스위칭하기

- git 계정을 여러개 등록해두고 스위칭할 때 사용한다.
- 필요한것은 이름, 이메일, GPG 인증키이다.
- 등록후에 commit할때 gpg 오류가 난다면, user를 삭제하고 다시 등록하도록 한다.

```bash
sudo npm i -g git-user-switch # 명령어 설치하기
git-user # 유저 변경하기
git-user -d # 유저 삭제하기
```

git-user로 계정을 변경후에 아래와 같은 gpg 에러가 발생할 수 있다.
이 경우는 gpg 키가 빈값으로 들어간 듯 하다. 유저를 삭제하고 git-user에 다시 등록해주면 된다.
```
error: gpg failed to sign the data:
gpg: skipped "": Invalid user ID
[GNUPG:] INV_SGNR 0
[GNUPG:] FAILURE sign 37
gpg: signing failed: Invalid user ID

fatal: failed to write commit object
```
