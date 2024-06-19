---
title: "[관습(Convention)] JSON - Camel Case? or Snake Case?"
excerpt: JSON의 Case Convention에 대해
layout: single
lang: ko
categories:
  - Convention
tags:
  - Convention
  - JSON
---


[참고 링크](https://stackoverflow.com/questions/5543490/json-naming-convention-snake-case-camelcase-or-pascalcase)


# Case Convention
팀이나 기업 단위에서 정하는 표준 규칙이나, 프로그래밍 언어에 따라 달라지는 게 일반적이다.

하지만 몇 가지 관습(convention)적으로 사용되는 큰 분류는 존재한다.

## Camel Case
Camel Case는 주로 `Node.js, React.js, Angular.js` 같이 자바스크립트와 관련된 프레임워크에서 사용된다.

### 예시
```json
{
    "firstName": "John",
    "lastName": "Doe",
    "phoneNumber": "123-456-7890"
}
```

## Snake Case
Snake Case는 주로 `Django, Flask` 같이 파이썬과 관련된 프레임워크에서 사용된다.

### 예시
```json
{
    "first_name": "John",
    "last_name": "Doe",
    "phone_number": "123-456-7890"
}
```

# Go 언어에서의 JSON Convention
Go 언어를 작업하다보니 JSON Convention을 찾기 시작한 건데, ChatGPT에 물어봐도 Go는 현재 관례는 없는 듯하다. 짧게 알아 본 바로는 Snake Case나 Camel Case를 선택할 수 있다는 입장이다.

하지만 개인적으로는 `JSON`이 기본적으로 **Camel Case**를 지향하기 때문에, 그 선택지를 따르는 게 옳다는 입장이다.

## 사용법
Go에서는 구조체 태그를 사용하여 JSON 필드 이름을 명시적으로 지정 및 읽기/쓰기를 진행할 수 있다.

## Camel Case 예시
```go
type Person struct {
    FirstName   string `json:"firstName"`
    LastName    string `json:"lastName"`
    PhoneNumber string `json:"phoneNumber"`
}
```

## Snake Case 예시
```go
type Person struct {
    FirstName   string `json:"first_name"`
    LastName    string `json:"last_name"`
    PhoneNumber string `json:"phone_number"`
}
```


# 중요한 것은
1. **프로그래밍 언어 및 프레임워크의 표준을 따르기**: 주로 사용하는 언어와 프레임워크의 관습을 따르는 것이 좋다.
2. **일관성 유지**: 프로젝트 내에서 한 가지 스타일을 선택하고 일관되게 사용하는 것이 중요하다.
3. **API 문서화 참조**: 사용하는 API나 라이브러리의 문서화를 참조하여 해당 표준을 따르는 게 필요하다.