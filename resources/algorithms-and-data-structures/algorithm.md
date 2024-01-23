---
published: false
id: algorithm
slug: algorithm
title: Algorithm
description: algorithm
tags: ["algorithms-and-data-structures"]
categories: ["algorithms-and-data-structures"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Algorithm
학습한 알고리즘에 대해서 정리해보기

## String algorithm
url을 브라우저에 입력하는 경우를 생각해보자, 이 경우 url을 입력할 때마다 자동완성 결과가 나온다.
string 패턴이 맞으면 그 결과를 보여주는 것이다.
terminal에서도 directory의 prefix를 입력하고 tab을 입력하면 자동완성 결과를 보여준다.

## Brute Force Method
모든 가능성이 있는 position을 체크한다. 즉 모든 경우의 수를 체크한다.
T라는 string(길이가 n)에서 P라는 pattern(길이가 m)을 찾는 경우이다.
총 n - m + 1의 경우의 수가 있다.
(It is also known as an exhaustive search algorithm¡)
