---
title: [Go(Golang)] Go 문법 - 구조체의 멤버 함수(struct member function)
excerpt: Go 언어의 구조체 멤버 함수에 대해
layout: single
lang: ko
categories:
  - Go
tags:
  - Go
  - Struct
---

# 간단 코멘트



```go
package main

import (
    "fmt"
)

// Person 구조체 정의
type Person struct {
    Name string
    Age  int
}

// 메서드 정의
// (p *Person)는 리시버로, 메서드가 Person 타입의 구조체에 속함을 나타냅니다.
func (p *Person) Greet() {
    fmt.Printf("Hello, my name is %s and I am %d years old.\n", p.Name, p.Age)
}

func main() {
    // Person 구조체 인스턴스 생성
    person := Person{Name: "John", Age: 30}

    // Greet 메서드 호출
    person.Greet()
}
```
