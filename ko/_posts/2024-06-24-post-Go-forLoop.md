---
title: "[Go(Golang)] Go 문법 - For 문의 다양한 사용법"
excerpt: Go 언어에서 For 문을 다양한 문법으로 사용하는 방법
layout: single
lang: ko
categories:
  - Go
tags:
  - Go
  - for
---


# For 문 기초
for 문은 기본적으로 무한 루프이며, **C++** 처럼 정수 변수 하나를 선언한 뒤 순차적으로 증가시키며 작업하는 것도 가능하다.

**C#** 의 `foreach`처럼 리스트 하나를 던져주고, 각 멤버에 순회 접근하는 하는 방법도 가능하다.


## 환경 설정 (models.go)

```go
package models

import "github.com/gorilla/websocket"

// 상수 선언
const (
    MaxImages  = 5
    MaxAnswers = 5
)

type Game struct {
    Images  [MaxImages]string     // 고정 크기 배열
    Answers [MaxAnswers][2]string // 2차원 고정 크기 배열
}
```

## 사용 예제 (main.go)

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

## for ~ range 사용 예제
```go
package main

import "fmt"

func main() {
    numbers := []int{1, 2, 3, 4, 5}

    for index, value := range numbers {
        fmt.Printf("Index: %d, Value: %d\n", index, value)
    }
}
```