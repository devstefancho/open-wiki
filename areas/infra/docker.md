---
published: false
id: docker
slug: docker
title: Docker
description: docker
tags: ["infra"]
categories: ["infra"]
createdDate: 2024-01-17
updatedDate: 2024-01-23
---

# docker

github action을 테스트하기 위해서는 로컬에 docker 환경을 구축하여
yml파일에 있는 run을 순차적으로 실행해본다.

- docker내 기본 패키지 설치 (cli 사용하기 위해서는 Docker Desktop을 설치해야함)
```bash
docker run -it --name github-actions-test ubuntu bash
apt-get update
apt-get install -y nodejs npm curl
npm install --global yarn

# nvm 설치 (nodejs 버전 관리)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
nvm install 18
```

- docker 내에 프로젝트 옮기기 및 실행
```bash
docker cp /User/chosungjin/project github-actions-test:/project
docker start -ai github-actions-test
```

- docker 기본 명령어
```bash
docker ps -a # 모든 컨테이너 목록
docker logs github-actions-test # 컨테이너 로그
```

