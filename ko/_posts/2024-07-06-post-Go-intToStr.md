---
title: "[Go(Golang)] Go 문법 - 형식 캐스팅: int To string"
excerpt: "Go 언어에서 타입 캐스팅(Type Casting): int에서 string 형식으로"
layout: single
lang: ko
categories:
  - Go
tags:
  - Go
  - common
  - package
---


Go에서 `int`를 `string`으로 변환하는 방법은 `strconv` 패키지를 사용하는 것이다. `strconv` 패키지의 `Itoa` 함수를 사용하면 `int` 값을 `string`으로 변환할 수 있다.

예를 들어, `num`이라는 `int` 변수를 `str`이라는 `string` 변수로 변환하려면 다음과 같이 한다.

```go
package main

import (
    "fmt"
    "strconv"
)

func main() {
    num := 123
    str := strconv.Itoa(num)
    fmt.Println(str)  // 출력: "123"
}
```