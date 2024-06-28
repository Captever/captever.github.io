---
title: "[Go(Golang)] Go 문법 - iota: Go 에서 enum 활용하기"
excerpt: Go 언어에 존재하지 않는 enum을 대체하는 방법
layout: single
lang: ko
categories:
  - Go
tags:
  - Go
  - enum
  - iota
---

# 개요

Go에서는 전통적인 `enum` 키워드를 제공하지 않는다. 하지만, `iota`라는 게 존재하며 이를 통해 열거형 값을 정의할 수 있다.

`iota`는 Go에서 상수를 열거할 때 사용하는 식별자로, 상수 선언이 있을 때마다 값이 0부터 시작해서 1씩 증가한다. `C 계열` 기준 `enum`과 거의 일치한다고 볼 수 있다.

# 특징

`iota`는 상수 선언을 단순화하고 가독성을 높이는 데 유용하다. 열거형 타입은 주로 코드 내에서 특정 상태나 옵션을 표현할 때 사용된다.

# 예제

```go
package main

import "fmt"

type Day int

const (
    Sunday Day = iota  // 0
    Monday             // 1
    Tuesday            // 2
    Wednesday          // 3
    Thursday           // 4
    Friday             // 5
    Saturday           // 6
)

func main() {
    var today Day = Wednesday
    fmt.Println(today) // Output: 3
}
```
이 예제에서 `Day` 타입은 열거형을 정의하기 위해 사용되었고, `iota`를 통해 각 요일에 대한 상수 값을 자동으로 할당하였다. `Wednesday`는 3이라는 값을 가지게 되고, `fmt.Println(today)`는 3을 출력하게 된다.