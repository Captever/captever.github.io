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


Go에서 C++의 `const`와 유사하게 상수를 선언하려면 `const` 키워드를 사용합니다. 상수는 전역적으로 선언할 수 있으며, 프로그램 내에서 변경할 수 없는 값을 저장할 수 있습니다. 

다음은 `models.go` 파일에 상수를 선언하고 사용하는 예제입니다.

### 상수 선언 예제

#### models.go

```go
package models

import "github.com/gorilla/websocket"

// 상수 선언
const (
    MaxImages  = 10
    MaxAnswers = 5
)

type Room struct {
    Clients      map[*websocket.Conn]bool
    Broadcast    chan Message
    MasterClient *websocket.Conn
}

type Lobby struct {
    Clients map[*websocket.Conn]bool
    Rooms   map[string]*Room
}

type Message struct {
    Action string `json:"action"` // 액션 타입 (예: create_room, join_room)
    RoomID string `json:"roomID"`
    Data   string `json:"data"`
}

type Game struct {
    Images  [MaxImages]string     // 고정 크기 배열
    Answers [MaxAnswers][2]string // 2차원 고정 크기 배열
}
```

### 상수 사용 예제

#### main.go

```go
package main

import (
    "fmt"
    "project/models"
)

func main() {
    // Game 구조체 인스턴스 생성
    game := models.Game{
        Images: [models.MaxImages]string{
            "https://example.com/image1.jpg",
            "https://example.com/image2.jpg",
            // 나머지 초기화 생략
        },
        Answers: [models.MaxAnswers][2]string{
            {"answer1a", "answer1b"},
            {"answer2a", "answer2b"},
            // 나머지 초기화 생략
        },
    }

    // 출력
    fmt.Println("Images:", game.Images)
    fmt.Println("Answers:", game.Answers)
}
```

### 설명

- **상수 선언**:
  - `const` 키워드를 사용하여 상수를 선언합니다.
  - `MaxImages`와 `MaxAnswers`는 프로그램 내에서 변경할 수 없는 상수 값입니다.

- **상수 사용**:
  - `Game` 구조체에서 상수를 사용하여 고정 크기 배열의 크기를 지정합니다.
  - 배열의 크기를 지정할 때 상수를 사용하면 코드의 가독성과 유지보수성이 향상됩니다.

### 요약

- Go에서 상수를 선언하려면 `const` 키워드를 사용합니다.
- 상수는 전역적으로 선언할 수 있으며, 프로그램 내에서 변경할 수 없습니다.
- 상수를 사용하여 배열의 크기를 지정할 수 있습니다.

이를 통해 Go에서 상수를 선언하고 사용하는 방법을 이해할 수 있습니다. 상수를 사용하면 코드의 가독성과 유지보수성을 높일 수 있습니다.