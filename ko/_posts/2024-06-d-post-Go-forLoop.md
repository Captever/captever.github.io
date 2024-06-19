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


### For 문은 기본적으로 무한 루프이다.


네, Go에서도 C++에서처럼 상수를 `for` 루프에 적용하여 사용할 수 있습니다. `const`로 선언된 상수는 프로그램 전역에서 사용할 수 있으며, 이를 반복문의 조건으로 사용할 수 있습니다.

다음은 `models.MaxImages`를 `for` 루프에 적용하는 예제입니다.

### 상수 선언 (models.go)

```go
package models

import "github.com/gorilla/websocket"

// 상수 선언
const (
    MaxImages  = 5
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

### 상수 사용 (main.go)

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
            "https://example.com/image3.jpg",
            "https://example.com/image4.jpg",
            "https://example.com/image5.jpg",
        },
        Answers: [models.MaxAnswers][2]string{
            {"answer1a", "answer1b"},
            {"answer2a", "answer2b"},
            {"answer3a", "answer3b"},
            {"answer4a", "answer4b"},
            {"answer5a", "answer5b"},
        },
    }

    // 반복문에서 상수 사용
    fmt.Println("Images:")
    for i := 0; i < models.MaxImages; i++ {
        fmt.Printf("Image %d: %s\n", i+1, game.Images[i])
    }

    fmt.Println("\nAnswers:")
    for i := 0; i < models.MaxAnswers; i++ {
        fmt.Printf("Answers for image %d: %v\n", i+1, game.Answers[i])
    }
}
```

### 설명

- `models.MaxImages`와 `models.MaxAnswers`는 상수로 선언되어 있습니다.
- `main` 함수에서 `for` 루프의 조건으로 이 상수를 사용하여 반복 횟수를 설정합니다.
- 반복문이 상수 값만큼 실행되어 각 배열의 요소를 출력합니다.

### 요약

- Go에서 상수를 `for` 루프의 조건으로 사용할 수 있습니다.
- `const`로 선언된 상수는 전역적으로 접근할 수 있으며, 반복문의 조건으로 사용하여 원하는 횟수만큼 반복을 수행할 수 있습니다.
- 이를 통해 반복문에서 상수를 사용하여 코드의 가독성과 유지보수성을 높일 수 있습니다.

위 예제에서는 `models.MaxImages`와 `models.MaxAnswers` 상수를 `for` 루프에 사용하여 각 배열의 요소를 출력합니다. 이는 C++에서 상수를 `for` 루프에 사용하는 것과 유사합니다.