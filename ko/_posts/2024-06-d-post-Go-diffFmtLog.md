---
title: [Go(Golang)] Go 문법 - 공통 패키지 관리(Common 테크닉)
excerpt: Go 언어에서 공통 기능을 별도의 패키지로 분리하고 싶을 때
layout: single
lang: ko
categories:
  - Go
tags:
  - Go
  - common
  - package
---


Go 언어에서 `fmt`와 `log` 패키지는 각각 다른 용도로 사용되며, 주된 차이점은 다음과 같습니다.

### `fmt` 패키지
`fmt` 패키지는 기본적인 입출력 작업을 위한 포맷팅 기능을 제공합니다. 주로 문자열이나 값을 표준 출력에 출력하거나, 문자열로 변환할 때 사용합니다.

#### 주요 기능
- **Print 함수**: 표준 출력에 값을 출력합니다.
  - `fmt.Print()`, `fmt.Println()`, `fmt.Printf()`
- **Sprint 함수**: 값을 문자열로 변환합니다.
  - `fmt.Sprint()`, `fmt.Sprintf()`, `fmt.Sprintln()`
- **Scan 함수**: 표준 입력으로부터 값을 읽습니다.
  - `fmt.Scan()`, `fmt.Scanf()`, `fmt.Scanln()`

#### 예시
```go
package main

import (
    "fmt"
)

func main() {
    name := "Alice"
    age := 30

    fmt.Printf("Name: %s, Age: %d\n", name, age)
    str := fmt.Sprintf("Name: %s, Age: %d", name, age)
    fmt.Println(str)
}
```

### `log` 패키지
`log` 패키지는 로깅을 위한 기능을 제공합니다. 프로그램의 실행 상태나 에러 메시지 등을 기록하는 데 주로 사용됩니다.

#### 주요 기능
- **기본 로깅**: 표준 로그 출력 (기본적으로 표준 오류 출력)
  - `log.Print()`, `log.Println()`, `log.Printf()`
- **Fatal 로그**: 메시지를 출력한 후 프로그램을 종료합니다.
  - `log.Fatal()`, `log.Fatalf()`, `log.Fatalln()`
- **Panic 로그**: 메시지를 출력한 후 `panic`을 발생시킵니다.
  - `log.Panic()`, `log.Panicf()`, `log.Panicln()`

#### 예시
```go
package main

import (
    "log"
    "os"
)

func main() {
    name := "Alice"
    age := 30

    log.Printf("Name: %s, Age: %d", name, age)

    file, err := os.Open("nonexistentfile.txt")
    if err != nil {
        log.Fatalf("Error opening file: %v", err)
    }
    defer file.Close()
}
```

### 차이점 요약
1. **기능 목적**:
   - `fmt`: 기본적인 포맷팅과 출력 작업을 수행합니다.
   - `log`: 프로그램의 상태 및 에러를 로깅하고, 심각한 에러 발생 시 프로그램을 종료하거나 패닉을 발생시킬 수 있습니다.

2. **출력 위치**:
   - `fmt`: 기본적으로 표준 출력(stdout)에 출력합니다.
   - `log`: 기본적으로 표준 오류(stderr)에 출력합니다.

3. **에러 처리**:
   - `fmt`: 단순히 출력만 수행합니다.
   - `log`: 에러 발생 시 프로그램을 종료(`log.Fatal*`)하거나 패닉(`log.Panic*`)을 발생시킬 수 있습니다.

4. **사용 편의성**:
   - `fmt`는 간단한 출력 작업에 적합합니다.
   - `log`는 로깅 작업과 에러 처리를 포함하는 경우에 적합합니다.

이 차이점을 이해하면 상황에 맞게 적절한 패키지를 선택하여 사용할 수 있습니다.