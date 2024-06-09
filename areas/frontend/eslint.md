---
published: false
id: eslint
slug: eslint
title: Eslint
summary: eslint
toc: true
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2024-06-07
updatedDate: 2024-06-08
---

# Eslint

## How to debug
yarn info --all 을 하면 현재 디펜던시 전체정보를 확인할 수 있다.

## typescript-eslint
typescript-eslint의 디펜던시는 아래와 같이 나온다.
따라서 이것만 설치하면 parser랑 plugin은 설치할 필요가 없다.
```
❯ yarn info typescript-eslint
└─ typescript-eslint@npm:7.12.0
   ├─ Instances: 1
   ├─ Version: 7.12.0
   │
   └─ Dependencies
      ├─ @typescript-eslint/eslint-plugin@npm:7.12.0 → npm:7.12.0
      ├─ @typescript-eslint/parser@npm:7.12.0 → npm:7.12.0
      └─ @typescript-eslint/utils@npm:7.12.0 → npm:7.12.0
```

## Works well
vite 프로젝트에서 eslint 이상없는 버전 설정은 아래와 같다.
eslint는 8버전이어야하고, typescript-eslint는 7.2 버전이어야한다.
```json
{
  "dependencies": {
    "@types/react": "18.3.3",
    "@types/react-dom": "18.3.0",
    "@vitejs/plugin-react": "4.3.0",
    "@xterm/addon-fit": "^0.10.0",
    "@xterm/xterm": "^5.5.0",
    "express": "^4.17.1",
    "react": "18.3.1",
    "react-dom": "18.3.1",
    "vite": "^5.2.12",
    "ws": "^7.4.6"
  },
  "devDependencies": {
    "@eslint/js": "8.42.0",
    "@types/eslint__js": "8.42.3",
    "eslint": "^8.57.0",
    "typescript": "5.4.5",
    "typescript-eslint": "^7.12.0"
  }
}
```

## Errors

### Invalid Options 에러가 발생했을 때
아래 옵션들은 실제로 eslint 설정파일에 넣지 않았는데도 발생했었다.
이유를 찾아보니 eslint 버전이 9 이상으로 올라가면서 발생한 것으로 추측되었다.
eslint 버전을 8 아래로 낮춰줬다.
https://typescript-eslint.io/users/dependency-versions/
```
1. Error: Invalid Options:
   - Unknown options: extensions, ignorePath, reportUnusedDisableDirectives, resolvePluginsRelativeTo, rulePaths, useEslintrc
   - 'extensions' has been removed.
   - 'resolvePluginsRelativeTo' has been removed.
   - 'ignorePath' has been removed.
   - 'rulePaths' has been removed. Please define your rules using plugins.
   - 'reportUnusedDisableDirectives' has been removed. Please use the 'overrideConfig.linterOptions.reportUnusedDisableDirectives' option instead.
```


### Class extends value undefined is not a constructor or null 라는 에러가 발생했을 때
정확한 원인을 모르는 에러이다. typescript-eslint 버전을 지웠다가 조정하고 반복하다보니 해결되었다..


