---
published: false
id: reactjs
slug: reactjs
title: Reactjs
summary: reactjs
toc: true
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# React
## Click Event without passing parameter

```typescript
const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
  console.log(e.currentTarget.name);
};

return (
  <div>
    <button name="ok" onClick={handleClick}>
      Ok
    </button>
    <button name="cancel" onClick={handleClick}>
      Cancel
    </button>
  </div>
);
```

## router 구조

```md
src/theme.ts --> styled-component theme
src/App.tsx
src/Router.tsx
src/index.tsx

src/routes/Coin.tsx
src/routes/Coins.tsx
```

## Reset all react styles
React component that all styles are rest

https://www.npmjs.com/package/styled-reset
