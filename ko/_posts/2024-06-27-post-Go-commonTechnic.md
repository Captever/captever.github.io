---
title: "[Go(Golang)] Go 문법 - 공통 패키지 관리(Common 테크닉)"
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

# 패키지 구조화

- 패키지를 구조화하면 공통 패키지를 관리할 때 편리하다.
- 공통 기능을 별도의 패키지로 분리하고, 필요한 곳에서 임포트하여 사용한다.
- 빌드 태그를 사용하는 것보다 직관적이고 유지보수하기 쉽다.

## 예제

공통 패키지의 이름을 `common`로 가정

### common/common.go

```go
package common

import (
    "log"
    "net/http"
    "encoding/json"
    "github.com/gorilla/websocket"
)

var (
    Log = log.Println
    JSON = json.Marshal
    HttpGet = http.Get
    WebsocketDialer = websocket.DefaultDialer
)
```

### main.go

```go
package main

import (
    "common"
)

func main() {
    common.Log("Server started")
    http.HandleFunc("/ws", handleConnections)
    common.Log("Server started on :8080")
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```