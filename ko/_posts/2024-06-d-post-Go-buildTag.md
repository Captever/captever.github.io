---
title: [Go(Golang)] Go 문법 - 빌드 태그(Build Tag)
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


빌드 태그를 사용하는 이유와 그 작동 방식을 더 자세히 설명드리겠습니다.

### 빌드 태그란?

빌드 태그는 Go 소스 파일에 조건부 컴파일을 적용할 수 있는 기능입니다. 이는 특정 조건에서만 컴파일되도록 파일을 선택할 수 있게 해줍니다. 이를 통해 다양한 빌드 환경에 맞춰 소스 코드를 유연하게 관리할 수 있습니다.

### 빌드 태그 사용 이유

1. **조건부 컴파일**:
   - 특정 플랫폼이나 환경에 따라 코드를 선택적으로 포함하거나 제외할 수 있습니다.
   - 예를 들어, 운영체제별 코드 분기, 디버깅용 코드 포함 등을 설정할 수 있습니다.

2. **모듈화**:
   - 공통 코드나 특정 기능을 별도로 관리할 수 있으며, 필요할 때만 포함할 수 있습니다.
   - 프로젝트의 유지보수성을 높이고, 불필요한 코드의 컴파일을 방지합니다.

### 빌드 태그의 작동 방식

빌드 태그는 파일의 상단에 `// +build` 주석으로 정의됩니다. 빌드 과정에서 `-tags` 옵션을 사용하여 특정 태그를 활성화하면, 해당 태그가 있는 파일이 포함되어 컴파일됩니다.

#### 예제

##### 공통 패키지 파일 정의 (`common.go`)

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

##### 사용 파일 정의 (`main.go`)

```go
// +build common

package main

func main() {
    http.HandleFunc("/ws", handleConnections)
    log.Println("Server started on :8080")
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

##### 컴파일 명령

```sh
go build -tags common
```

이 명령어는 `common` 태그가 있는 파일만 포함하여 컴파일합니다.

### 태그가 파일에 미치는 영향

빌드 태그는 해당 태그를 포함한 주석이 있는 파일을 컴파일할 때 조건부로 포함되도록 만듭니다. 즉, 특정 태그를 활성화하면, 그 태그가 있는 모든 파일이 컴파일에 포함됩니다.

`common` 태그가 있는 파일은 `go build -tags common` 명령어를 사용하여 컴파일할 때만 포함됩니다. 이는 `common` 태그가 있는 파일에서 공통된 패키지를 임포트하고, 이를 다른 파일에서도 사용할 수 있도록 합니다.

### 빌드 태그 사용 이유 요약

- 빌드 태그를 사용하면 파일을 조건부로 컴파일할 수 있습니다.
- 이를 통해 특정 환경이나 플랫폼에 맞게 소스 코드를 선택적으로 포함할 수 있습니다.
- 공통 패키지 파일을 정의하고, 필요한 파일에서 해당 태그를 사용하여 공통 패키지를 쉽게 포함할 수 있습니다.

### 공통 패키지 관리를 위한 대안

빌드 태그 외에도 공통 패키지를 관리하기 위한 몇 가지 다른 방법도 있습니다:

1. **공통 패키지를 별도의 파일로 관리**:
   - 모든 파일에서 공통으로 사용하는 패키지를 `common_imports.go`와 같은 파일에 모아두고, 각 파일에서 해당 파일을 임포트합니다.

2. **패키지 구조화**:
   - 공통 기능을 별도의 패키지로 분리하여, 필요한 곳에서 임포트하여 사용합니다.

예를 들어, 공통 패키지를 `common`이라는 패키지로 정의할 수 있습니다:

#### common/common.go

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

#### main.go

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

이 방법은 빌드 태그를 사용하는 것보다 직관적이며 유지보수하기 쉽습니다. 공통 기능을 별도의 패키지로 분리하여, 필요할 때 임포트하여 사용하는 방식입니다.