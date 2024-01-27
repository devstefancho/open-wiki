---
published: false
id: createContext
slug: createContext
title: CreateContext
summary: createContext
toc: true
tags: ["apis"]
categories: ["apis"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# createContext

Date created: 2023-06-14

```tsx
const SomeContext = createContext(defaultValue)
```

## defaultValue
defaultValue는 변하지 않는 값입니다.


## 함수 및 컴포넌트
createContext는 어떤 Context 인지를 나타내는것으로, Context 자체는 아무정보도 갖고 있지 않습니다.
SomeContext.Provider는 값을 제공합니다.
SomeContext.Consumer으로 값을 읽을 수 있습니다. 단, Legacy이므로 사용하지 마세요.
useContext는 값을 읽을 수 있고, 가장 가까운 Provider에서 값을 읽습니다.

## 파일분리
보통 Context는 여러파일에서 import 하여 사용하므로, 별도의 파일로 분리해둡니다.
```ts
// Contexts.js
import { createContext } from 'react';

export const ThemeContext = createContext('light');
export const AuthContext = createContext(null);
```

