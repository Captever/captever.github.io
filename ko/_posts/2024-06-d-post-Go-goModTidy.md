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


`go.sum` 파일은 Go 모듈 시스템에서 의존성의 정확한 버전을 기록하는 파일입니다. `go.mod` 파일을 변경할 때, `go.sum` 파일도 함께 업데이트해야 합니다. `go 1.16`으로 변경하면서 발생한 에러는 `go.sum` 파일이 `go.mod` 파일과 일치하지 않기 때문에 발생한 것입니다.

이를 해결하려면 다음 단계를 따르세요:

1. **`go.sum` 파일 업데이트**:
   - `go mod tidy` 명령을 사용하여 `go.mod`와 `go.sum` 파일을 최신 상태로 업데이트할 수 있습니다.

```sh
go mod tidy
```

이 명령은 `go.mod`와 `go.sum` 파일을 동기화하고, 프로젝트에서 필요 없는 모듈을 제거합니다.

2. **`go get` 명령 사용**:
   - 필요한 경우 특정 패키지 버전을 다시 가져오도록 `go get` 명령을 사용할 수 있습니다.

```sh
go get github.com/aws/aws-sdk-go@v1.54.2
go get github.com/gorilla/websocket@v1.5.3
```

3. **에디터나 IDE에서 Quick Fix 적용**:
   - 많은 IDE와 코드 에디터는 `go.sum` 파일의 불일치를 자동으로 수정할 수 있는 Quick Fix 기능을 제공합니다. 해당 기능을 이용할 수도 있습니다.

이 단계를 통해 `go.mod`와 `go.sum` 파일을 동기화하여 문제를 해결할 수 있습니다.

### 예제

#### 1. `go.mod` 파일 변경

`go.mod` 파일에서 Go 버전을 변경합니다.

```go
module websocket

go 1.16

require (
    github.com/aws/aws-sdk-go v1.54.2 // indirect
    github.com/gorilla/websocket v1.5.3 // indirect
)
```

#### 2. `go mod tidy` 명령 실행

터미널에서 프로젝트 루트 디렉토리로 이동한 후 `go mod tidy` 명령을 실행합니다.

```sh
cd path/to/project
go mod tidy
```

이 명령은 `go.mod` 파일과 `go.sum` 파일을 동기화합니다.

#### 3. `go.sum` 파일 확인

`go mod tidy` 명령을 실행한 후, `go.sum` 파일이 업데이트되어 `go.mod` 파일과 일치하게 됩니다.

### 요약

- `go mod tidy` 명령을 사용하여 `go.mod`와 `go.sum` 파일을 동기화합니다.
- 필요한 경우 `go get` 명령을 사용하여 특정 버전을 다시 가져올 수 있습니다.
- IDE나 코드 에디터의 Quick Fix 기능을 사용하여 문제를 자동으로 해결할 수 있습니다.

이 단계를 통해 `go.sum is out of sync with go.mod` 에러를 해결할 수 있습니다.