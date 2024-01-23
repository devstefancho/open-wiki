---
published: false
id: font
slug: font
title: Font
description: font
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Font

## Unicode
font 텍스트는 unicode 조합으로 이루어진다.
발음기호(diacritic)의 예를 보자.
아래와 같은 turned V에다가 accent가 있는 경우이면

### Accent
turnedV + accent 의 조합으로 만들어진다.
```
--- turnedV인 경우
text: 'ʌ',
unicode: '\u028C',

--- turnedV에 accent까지 있는 경우
text: 'ʌ́',
unicode: '\u028C\u0301',
```

### Grave Accent
grave accent의 경우는 아래와 같다.
```
text: 'ǝ̀',
unicode: '\u01DD\u0300',
```
따라서 특정 latin 문자열에 accent나 grave accent가 필요한 경우, latin 문자열뒤에 추가로 unicode를 붙여주면 된다.

### All in one
accent까지 있는게 하나의 unicode인 경우도 있다.
```
text: 'á',
unicode: '\u00E1',
```

### JSX
jsx에서 표시할때는 그냥 넣어주면 된다.
```typescriptreact
return (
  <div>{'\u00E1'}</div>
)
```

