---
title: "[Go(Golang)] Go 문법 - 2차원 배열에 요소 추가하기"
excerpt: Go 언어에서 2차원 배열을 정의하고 요소를 추가하는 방법
layout: single
lang: ko
categories:
  - Go
tags:
  - Go
  - 2Darray
  - slice
  - map
---

# 2차원 배열에 요소 추가하기

2차원 배열에 요소를 추가할 때 3가지 자료형으로 시도해봤다.
`slice`, `array`, `map`


## 1. `slice`에 요소 추가

`slice`는 가변 길이 배열이다. `append` 함수를 사용해 요소 추가가 가능하다.

```go
package main

import "fmt"

func main() {
    // 초기화
    var slice2D [][]string

    // 값 할당
    slice2D = [][]string{
        {"a1", "a2"},
        {"b1", "b2"},
    }

    // 새로운 1차원 배열 추가
    newAnswers := []string{"c1", "c2", "c3"}
    slice2D = append(slice2D, newAnswers)

    // 출력
    fmt.Println("Slice 2D:", slice2D)
}
```

## 2. `array`에 요소 추가

`array`는 고정 길이 배열이라 처음부터 크기를 지정해야만 한다.

`array`의 크기를 동적으로 변경할 수 없기 때문에 새로운 요소를 추가하는 것은 적절하지만, 필요한 경우 `slice`로 변환해 작업하는 게 가능하다.

```go
package main

import "fmt"

func main() {
    // 2차원 배열을 슬라이스로 변환
    var array2D = [2][2]string{
        {"a1", "a2"},
        {"b1", "b2"},
    }

    // array를 slice로 변환
    slice2D := array2D[:]

    // 새로운 1차원 배열 추가 (이 경우 슬라이스 사용)
    newAnswers := []string{"c1", "c2", "c3"}
    slice2D = append(slice2D, newAnswers)

    // 출력
    fmt.Println("Slice 2D:", slice2D)
}
```

## 3. `map`에 요소 추가

`map`은 Key-Value Pair로 저장된다.

**python**에서 Dictionary를 사용하듯 새로운 키를 추가하면 요소 삽입이 가능하다.

```go
package main

import "fmt"

func main() {
    // 초기화
    var map2D map[int]map[int]string
    map2D = make(map[int]map[int]string)

    // 값 할당
    map2D[0] = make(map[int]string)
    map2D[0][0] = "a1"
    map2D[0][1] = "a2"

    map2D[1] = make(map[int]string)
    map2D[1][0] = "b1"
    map2D[1][1] = "b2"

    // 새로운 1차원 배열 추가
    newAnswers := []string{"c1", "c2", "c3"}
    map2D[2] = make(map[int]string)
    for i, answer := range newAnswers {
        map2D[2][i] = answer
    }

    // 출력
    fmt.Println("Map 2D:", map2D)
}
```

## 정리

`slice`가 가장 유연하며, 일반적으로 사용하기 쉽다.
`array`는 고정 크기 때문에 유연성이라곤 찾아볼 수 없지만, 슬라이스로 변환하면 사용 가능하긴 하다.
`map`은 Key-Value Pair로 저장돼 유연하지만, 슬라이스보다 다루기 복잡할 수 있다.



# 2차원 배열에 1차원 배열 직접 할당하기
`array2D[0] = <1차원 배열>`과 같은 방식으로 1차원 배열을 직접 할당할 수 있다.

## 예제
`array2D`의 첫 번째 행을 `{"x1", "x2"}`로 업데이트하는 코드

```go
package main

import "fmt"

func main() {
    var array2D [3][2]string

    // 값 할당
    array2D = [3][2]string{
        {"a1", "a2"},
        {"b1", "b2"},
        {"c1", "c2"},
    }

    // 특정 인덱스에 1차원 배열 직접 할당
    array2D[0] = [2]string{"x1", "x2"}

    // 출력
    fmt.Println("Array 2D:", array2D)
}
```