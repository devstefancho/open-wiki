---
published: false
id: nestjs
slug: nestjs
title: Nestjs
summary: nestjs
toc: true
tags: ["backend"]
categories: ["backend"]
createdDate: 2024-01-16
updatedDate: 2024-01-23
---


# NestJS

## Dependency Injection

test시에 의존성 주입(Dependency Injection)을 해야한다.
new ContentService()를 하지 않고, module에서 ContentService를 가져와서 사용한다.
```ts
    contentService = new ContentService(); // -> 의존성 주입되지 않음
    contentService = module.get<ContentService>(ContentService); // -> 의존성 주입
```

## Error: Cannot use spyOn on a primitive value; undefined given

contentService를 생성하지 않아서 발생한 문제

```ts
import { Test, TestingModule } from '@nestjs/testing';
import { ContentController } from './content.controller';
import { ContentService } from './content.service';

describe('ContentController', () => {
  let contentController: ContentController;
  let contentService: ContentService;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [ContentService],
      controllers: [ContentController],
    }).compile();

    contentService = module.get<ContentService>(ContentService); // -> 이 코드를 누락함
    contentController = module.get<ContentController>(ContentController);
  });

  it('should return one content', async () => {
    const result = ['test-result'];
    jest.spyOn(contentService, 'findOne').mockImplementation(() => result); // -> 에러발생
    expect(await contentController.findOne()).toEqual(result);
  });
});
```

