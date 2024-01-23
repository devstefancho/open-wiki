---
published: false
id: react-hook-form
slug: react-hook-form
title: React Hook Form
description: react hook form
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2023-09-11
updatedDate: 2024-01-23
---


# React Hook Form

handleSubmit 구현부를 보면 stopPropagation이 없다.
이 부분을 수정하면 2 중폼을 사용해도 문제가 없다(추측)

```typescript
  const handleSubmit: UseFormHandleSubmit<TFieldValues> =
    (onValid, onInvalid) => async (e) => {
      if (e) {
        e.preventDefault && e.preventDefault();
        e.persist && e.persist();
      }
      let fieldValues = cloneObject(_formValues);

      _subjects.state.next({
        isSubmitting: true,
      });

      if (_options.resolver) {
        const { errors, values } = await _executeSchema();
        _formState.errors = errors;
        fieldValues = values;
      } else {
        await executeBuiltInValidation(_fields);
      }

      unset(_formState.errors, 'root');

      if (isEmptyObject(_formState.errors)) {
        _subjects.state.next({
          errors: {},
        });
        await onValid(fieldValues as TFieldValues, e);
      } else {
        if (onInvalid) {
          await onInvalid({ ..._formState.errors }, e);
        }
        _focusError();
        setTimeout(_focusError);
      }

      _subjects.state.next({
        isSubmitted: true,
        isSubmitting: false,
        isSubmitSuccessful: isEmptyObject(_formState.errors),
        submitCount: _formState.submitCount + 1,
        errors: _formState.errors,
      });
    };
```

확인결과 중첩 form 문제는 아니었음(Dialog의 onSubmit에 closeModal을 넣어둔게 문제였음)
- cms #287

```mermaid
%% b1, m1 : ProductPage.tsx
%% m2, f1: ProductFieldTemplate.tsx
%% f2 MappingContentDetailIdTemplate.tsx

flowchart TD
Index --> b1(Board) & m1(Modal) %% 
b1(Board) --> m2(Modal) & f1(Form) %% 
m2(Modal) --> f2(Form)
```

## Ref
- https://github.com/react-hook-form/react-hook-form/issues/10573
