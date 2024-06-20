---
title: "[Unity(C#)] Trouble Shooting - get_isActiveAndEnabled can only be called from the main thread."
excerpt: "Unity 트러블 슈팅 - get_isActiveAndEnabled can only be called from the main thread."
layout: single
lang: ko
categories:
  - Csharp
tags:
  - CSharp
  - TroubleShooting
---


# 상황

```csharp
[WebSocketClient.cs]
public void ConnectToServer()
{
    //ws = new WebSocket($"ws://{serverUrl}/ws?roomId={currRoomName}");
    ws = new WebSocket($"ws://{serverUrl}/ws");

    ws.OnOpen += (sender, e) =>
    {
        Debug.Log("WebSocket connection opened");
    };

    ws.OnMessage += (sender, e) =>
    {
        Debug.Log("Message received: " + e.Data);
        // Handle incoming message
        _mainSceneManager.ShowTextNewTestMessage(e.Data);
    };

    ws.OnError += (sender, e) =>
    {
        Debug.LogError("WebSocket error: " + e.Message);
    };

    ws.OnClose += (sender, e) =>
    {
        Debug.Log("WebSocket connection closed.");
    };

    ws.Connect();
}

// ==========

[MainSceneManager.cs]
public void ShowTextNewTestMessage(string newMessage)
{
    txtTestMessages.text += newMessage + '\n';
}
```

서버에서 메시지를 보낸다. 그럼 클라이언트에서는 `ws.OnMessage` 함수가 작동해 응답을 받게 되는데, 이 메시지를 받아서 화면에 표시하려고 `txtTestMessages` 라는 텍스트 UI에 추가하면, 아래의 에러가 발생한다.

```markdown
Error processing message: get_isActiveAndEnabled can only be called from the main thread.
Constructors and field initializers will be executed from the loading thread when loading a scene.
Don't use this function in the constructor or field initializers, instead move initialization code to the Awake or Start function.
```

# 원인

Unity에서 클라이언트가 운영되는 thread와 Websocket 서버에서 신호를 받아오는 thread가 공존한다. 개발 환경이 Unity이므로, **Unity 엔진 API**를 담당하는 건 당연히 Unity Main Thread이다.

Websocket의 메시지를 처리하는 쓰레드에서 갑자기 Main Thread가 담당하는 **Unity 엔진 API** 에 접근하려고 하니 이런 상황이 발생하는 거다.

**Main Thread: “어? Unity 엔진 API 내 거야. 선 넘지마.”**

# 해결 방법

## 큐잉 시스템 도입

먼저 큐(Queue) 객체를 만든다.

```csharp
private ConcurrentQueue<string> messageQueue = new ConcurrentQueue<string>();
```

WebSocket `OnMessage` 핸들러에서 메시지를 받으면 큐에 저장한다.

```csharp
[WebSocketClient.cs] void ConnectToServer()
ws.OnMessage += (sender, e) =>
{
    Debug.Log("Message received: " + e.Data);
    // Handle incoming message
    messageQueue.Enqueue(e.Data);
};
```

Unity의 `Update` 함수를 이용해 Main thread에서 큐에 메시지가 있는지 확인해 처리한다.

```csharp
[WebSocketClient.cs]
void Update()
{
    while (messageQueue.TryDequeue(out string message))
    {
        // Handle incoming message
        _mainSceneManager.ShowTextNewTestMessage(message);
    }
}
```