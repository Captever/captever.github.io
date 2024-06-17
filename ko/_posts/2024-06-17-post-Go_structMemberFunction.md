---
title: Go(Golang) | Go 문법 - 구조체의 멤버 함수(struct member function)
lang: ko
categories:
  - Go
tags:
  - Go
  - Struct
---

# 간단 코멘트
**Unity** 게임 클라이언트가 이용할 서버를 구축하기 위해 **Go lang**을 배우기 시작했다.
기존 **C++, C#** 등 프로그래밍 지식이 탄탄했기에, 순탄하게 배워가던 중에 생소한 느낌이 드는 주제가 보여서 포스팅하게 되었다.
**멤버 함수**를 구현하려다보니, **C 계열**로 생각하면 보통 구조체(struct)나 클래스(class) 안에 함수가 선언되는데 **Go**는 함수 밖에 선언된다.
그저 아무렇지 않게 구조체를 **파라미터(parameter)**로 입력 받는 한 함수인 것처럼 내동댕이 되어있는데, 신기하게도 **구조체 변수**에 `.`을 찍어 접근할 수 있다.


# 예제 코드
예제 상황은 이렇다. 이름과 나이 값을 저장할 `Name`과 `Age` 멤버를 가진 `Person` 구조체가 있다.
각 사람은 `Greet()` 함수를 통해 인삿말을 전달할 수 있다.
```Go
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
// (p *Person)는 리시버로, 메서드가 Person 타입의 구조체에 속함 나타내줌
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
