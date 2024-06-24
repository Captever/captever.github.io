---
title: "[Unity(C#)] 기초 환경 설정 - WebSocket 통신을 위한 WebSocketSharp 설치하기"
excerpt: "WebSocketSharp - WebSocket 패키지를 이용하기 위한 기초 환경 설정"
layout: single
lang: ko
categories:
  - Unity
tags:
  - Unity
  - WebSocket
---


# 기본 환경
`Visual Studio 2022`를 기준으로 진행합니다.

# 1. `Package Manager Console` 켜기

![PackageManagerConsole.png](/assets/resources/Unity/PackageManagerConsole.png)

# 2. `Install-Package` 명령어 입력

```
PM> NuGet\Install-Package WebSocketSharp -Version 1.0.3-rc11
```

# 3. 작업 디렉터리에 `websocket-sharp.dll` 파일 옮기기

프로젝트의 `Packages` 폴더 → `WebSocketSharp.<version>/lib`  폴더 하위에 있는 `websocket-sharp.dll` 파일을 프로젝트 `Assets` 폴더 내부로 옮기기

저는 `Assets/Scripts/AddOns` 에 저장했습니다.

## 최종 경로

```
<Unity_Project_Path>\Packages\WebSocketSharp.1.0.3-rc11\lib\websocket-sharp.dll
```