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

2차원 배열에 요소를 추가할 때 각 `slice`, `array`, `map` 자료형에 대해 설명드리겠습니다. 특히, 1차원 배열을 2차원 배열의 마지막 요소로 삽입하는 방법을 중점으로 설명하겠습니다.

### 1. `slice`에 요소 추가

`slice`는 가변 길이 배열이므로 `append` 함수를 사용하여 요소를 추가할 수 있습니다.

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

### 2. `array`에 요소 추가

`array`는 고정 길이 배열이므로, 처음부터 크기를 지정해야 합니다. `array`의 크기를 동적으로 변경할 수 없기 때문에, 크기가 고정된 `array`에 새로운 요소를 추가하는 것은 적절하지 않습니다. 대신, 필요한 경우 `slice`로 변환하여 작업하는 것이 일반적입니다.

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

### 3. `map`에 요소 추가

`map`은 키-값 쌍으로 저장되므로, 새로운 키를 추가하여 요소를 삽입할 수 있습니다.

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

### 요약

- **`slice`**: `append` 함수를 사용하여 새로운 1차원 배열을 2차원 배열의 마지막 요소로 추가할 수 있습니다.
- **`array`**: 고정 크기 배열이므로 새로운 요소를 동적으로 추가할 수 없습니다. 대신, 배열을 슬라이스로 변환하여 작업하는 것이 일반적입니다.
- **`map`**: 새로운 키를 추가하고, 해당 키에 대한 값을 설정하여 1차원 배열을 2차원 배열의 마지막 요소로 추가할 수 있습니다.

이러한 방법들을 통해 각 자료형에 대해 새로운 요소를 추가하는 방법을 이해할 수 있습니다. `slice`가 가장 유연하며, 일반적으로 사용하기 쉽습니다. `array`는 고정 크기 때문에 유연성이 떨어지지만, 슬라이스로 변환하여 사용할 수 있습니다. `map`은 키-값 쌍으로 저장되므로 유연하게 사용할 수 있지만, 슬라이스보다 다루기 복잡할 수 있습니다.



---
# 나에게 필요한 2차원 배열 활용 예시
네, `array2D[0]`와 같은 방식으로 1차원 배열을 직접 할당할 수 있습니다. 하지만 약간의 문법적인 수정이 필요합니다. Go에서 2차원 배열의 특정 인덱스에 1차원 배열을 직접 할당하려면, 올바른 문법을 사용해야 합니다.

다음은 `array2D[0]`에 1차원 배열을 직접 할당하는 예제입니다.

### 예제

#### 2차원 배열에 1차원 배열 직접 할당

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

이 코드는 `array2D`의 첫 번째 행을 `{"x1", "x2"}`로 업데이트합니다.

### 요약

- Go에서는 2차원 배열의 특정 인덱스에 1차원 배열을 직접 할당할 수 있습니다.
- 1차원 배열을 직접 할당할 때는 해당 배열의 타입과 크기를 명시적으로 지정해야 합니다.

이를 통해 2차원 배열의 특정 요소에 1차원 배열을 쉽게 할당할 수 있습니다.