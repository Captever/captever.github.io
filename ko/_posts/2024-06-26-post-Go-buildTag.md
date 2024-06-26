---
title: "[Go(Golang)] Go 문법 - 빌드 태그(Build Tag)"
excerpt: Go 언어에서의 조건부 컴파일
layout: single
lang: ko
categories:
  - Go
tags:
  - Go
  - build
  - compile
---


# 빌드 태그란?

: Go 소스 파일에 조건부 컴파일을 적용할 수 있는 기능

어떤 파일로 컴파일 할지 특정 조건에 따라 선택할 수 있게 해준다. 덕분에 다양한 빌드 환경에 맞춰 소스 코드를 유연하게 관리할 수 있다는 이점이 크다.

## 빌드 태그 사용 이유

1. **조건부 컴파일**:
   - 특정 플랫폼이나 환경에 따라 코드를 선택적으로 포함 또는 제외
   - ex) 운영체제별 코드 나누기, 디버깅용 코드 포함 여부 설정 등

2. **모듈화**:
   - 공통 코드나 특정 기능 별도 관리 가능
   - 특정 상황에 필요할 때만 선택해 포함 가능
   - 프로젝트 유지보수성 증가 및 불필요한 코드 컴파일 방지

## 빌드 태그의 작동 방식

빌드 태그는 파일 상단에 `// +build` 주석으로 정의한다.

빌드 과정에서 `-tags` 옵션을 사용하여 특정 태그를 활성화하면, 해당 태그가 있는 파일을 포함해 컴파일한다.

## 예제

### 공통 패키지 파일 - `common.go`

```go
// +build common

package main

import (
    "log"
    "net/http"
    "encoding/json"
    "github.com/gorilla/websocket"
)
```

### 사용 파일 - `main.go`

```go
// +build common

package main

func main() {
    http.HandleFunc("/ws", handleConnections)
    log.Println("Server started on :8080")
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

### 컴파일 명령

이 명령어는 `common` 태그가 있는 파일만 포함하여 컴파일한다.

```sh
go build -tags common
```

- 태그가 파일에 미치는 영향

특정 태그를 활성화하면, 그 태그가 있는 모든 파일이 컴파일에 포함된다.

예를 들어, `common` 태그가 있는 파일은 `go build -tags common` 명령어를 사용하여 컴파일할 때만 포함된다.

- 요약
  - 파일 조건부 컴파일
  - 특정 환경이나 플랫폼에 적합한 소스 코드 선택적 포함
  - 공통 패키지 파일 정의 후 필요한 파일에서 태그를 사용해 손쉽게 공통 패키지 포함