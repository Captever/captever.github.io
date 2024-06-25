---
title: "[Go(Golang)] Go 문법 - defer 키워드"
excerpt: Go 언어에서 등장하는 생소한 defer 키워드
layout: single
lang: ko
categories:
  - Go
tags:
  - Go
  - defer
---


# `defer`란

Go 언어에서 `defer` 키워드는 함수의 나머지 코드가 실행된 후에 지정된 함수나 메소드를 실행하도록 예약하는 역할을 한다.

`defer`로 호출된 함수는 현재 함수가 **return**되기 바로 직전에 실행된다.

# `defer`로 리소스 관리하기

`defer`는 주로 리소스 관리에 사용된다. 함수에서 파일과 같은 리소스를 열었을 때, 꼭 닫아주어야만 한다. 이때 `defer`를 사용하면 좋다. 중간에 **return**되거나, 에러가 발생해 함수가 종료돼도 실행을 보장해준다.

# `defer`가 필요한 이유는 무엇인가?

- **자원 정리**: `defer`는 함수가 종료될 때 필요한 청소 작업을 보장해준다. 덕분에 코드의 안정성, 리소스 누수 방지에 매우 효과적이다.
- **가독성 향상**: 리소스 사용 시작과 종료를 비슷한 위치에서 가능하게 함으로써 코드의 가독성이 향상된다.
- **에러 처리**: 에러 발생 시에도 `defer`를 사용한 리소스 해제 코드가 실행되기 때문에, 프로그램의 안정성이 유지된다.

# 예제

파일 열기 및 읽기 후 닫기

```go
package main

import (
    "fmt"
    "io/ioutil"
    "os"
)

func main() {
    // 파일 열기
    file, err := os.Open("example.txt")
    if err != nil {
        panic(err)
    }
    // main 함수가 종료될 때 파일 닫기를 보장
    defer file.Close()

    // 파일의 내용 읽기
    data, err := ioutil.ReadAll(file)
    if err != nil {
        panic(err)
    }

    // 파일 내용 출력
    fmt.Println(string(data))
}
```

## 핵심

`defer file.Close()`를 사용함으로써, `os.Open`으로 열린 파일을 함수 종료 시 안전하게 닫아준다.