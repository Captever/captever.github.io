---
title: "[Go(Golang)] Go 문법 - defer 명령으로 변수 값 바꾸기"
excerpt: "defer 명령으로 변수 값 바꾸기"
layout: single
lang: ko
categories:
  - Go
tags:
  - Go
  - defer
---

다음은 `defer`를 사용하여 `bool` 변수의 값을 변경하는 예제이다.

`bool` 변수의 값은 초기 값이 `false`로 설정된다. 아래 코드와 같이 `defer` 함수 안에 `bool` 변수의 값을 `true`로 변경하게 해줘도 함수 내의 실행은 `false` 값을 반환하는 걸 확인할 수 있다.

이는 중복 실행 방지 코드에 활용될 수 있다. 예를 들어, `isOnCreating` 변수가 있을 때 해당 변수가 `false` 일 때만 함수에 접근할 수 있도록 한다면 아래의 예시처럼 `defer`를 이용할 수 있다.

함수에 처음 접근할 땐 `true` 값으로 변경, `defer` 명령 안에 `false` 값으로 변경을 넣어 함수가 종료될 때 다시 접근할 수 있도록 설정할 수 있따.

```go
package main

import "fmt"

func example() {
    var flag bool

    defer func() {
        flag = true
    }()

    fmt.Println("Before defer:", flag) // 출력: Before defer: false
}

```