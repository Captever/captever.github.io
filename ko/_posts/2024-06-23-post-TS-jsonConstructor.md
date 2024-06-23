---
title: "[Unity(C#)] Trouble Shooting - JsonConvert.DeserializeObject 에러"
excerpt: "트러블 슈팅: Unity 클라이언트에서 Json 데이터를 역직렬화할 때 발생하는 에러"
layout: single
lang: ko
categories:
  - Json
  - Unity
tags:
  - Unity
  - TroubleShooting
---


# 상황

Go 서버에서 `MessageWrapper` 구조체를 통해 JSON 데이터를 클라이언트에 전달하고, 클라이언트에선 JSON 데이터를 받아 디시리얼라이즈(Deserialize)해 값에 접근한다. 근데 클라이언트가 websocket에서 값을 받아오려고만 하면 에러가 발생한다.

## 에러

![WebSocketOnMessageError.png](/assets/resources/TroubleShooting/WebSocketOnMessageError.png)

## 코드

```csharp
public void ConnectToServer()
{
    ws = new WebSocket($"ws://{serverUrl}/ws");

    ws.OnOpen += (sender, e) =>
    {
        Debug.Log("WebSocket connection opened");
    };

    ws.OnMessage += (sender, e) =>
    {
        Debug.Log("Message received: " + e.Data);

        // Handle incoming message
        HandleServerMessage(e.Data);
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

void HandleServerMessage(string jsonMessage)
{
    messageQueue.Enqueue(jsonMessage); // 메시지 표시

    MessageWrapper smw = new MessageWrapper(jsonMessage);

    switch (smw.action)
    {
        case "OnConnectedToMaster":
            JObject parsedMessage = JObject.FromObject(smw.data);

            string clientUUID = parsedMessage["clientUUID"].ToString();
            ClientUUID = clientUUID; // 변수에 저장
            messageQueue.Enqueue("clientUUID: " + clientUUID);
            break;
        default:
            Debug.Log("Unknown action from server message");
            break;
    }
}

[System.Serializable]
public class MessageWrapper
{
    public string action;  // 어떤 메시지인지 식별할 수 있는 헤더
    public string clientUUID; // 누가 보낸 메시지인지 식별하기 위함
    public object data;  // 메시지의 실제 데이터

    public MessageWrapper(string action, string clientUUID, object data)
    {
        this.action = action;
        this.clientUUID = clientUUID;
        this.data = data;
    }
    public MessageWrapper(string json)
    {
        MessageWrapper mw = JsonConvert.DeserializeObject<MessageWrapper>(json);
        action = mw.action;
        clientUUID = mw.clientUUID;
        data = mw.data;
    }

    public string ToJson()
    {
        return JsonConvert.SerializeObject(this);
    }
}
```

```go
func (s *Server) ConnectToMaster(ws *websocket.Conn, clientNickname string) {

	clientUUID := uuid.New().String()
	client := &models.Client{Conn: ws, ClientNickname: clientNickname}

	log.Printf("Current Client(%s) Info: %v", clientUUID, client)

	// 클라이언트에게 현 client의 정보를 보내서 기억할 수 있도록
	mw := models.MessageWrapper{
		Action:     "OnConnectedToMaster",
		ClientUUID: clientUUID,
		Data:       []byte(`{"clientUUID":"` + clientUUID + `"}`),
	}

	err := ws.WriteJSON(mw)
	if err != nil {
		log.Println("Error sending clientUUID:", err)
		delete(s.Lobby.Clients, clientUUID)
		return
	}

	s.JoinLobby(clientUUID, client) // MasterServer에 연결됐다면 자동으로 로비 입장
}

// 공통 메시지
type MessageWrapper struct {
	Action string        `json:"action"`    // 수행해야 할 Action
	ClientUUID string 	 `json:"clientUUID"` // 요청자 클라이언트 ID
	Data json.RawMessage `json:"data"`       // 실 데이터
}
```

# 해결 과정

```csharp
WebSocket error: An exception has occurred during an OnMessage event.
```

저 에러 문구만 보고는 당최 해결할 겨를이 없었다. 어디서 문제가 발생하는 건지도 알 수 없었고, `Debug.Log` 를 이용해 `MessageWrapper smw = new MessageWrapper(jsonMessage);` 이 부분에서 코드가 더 진행되지 않는다는 걸 알았지만, 자세한 문제를 알 순 없었다.

## Try ~ catch 로 에러를 더 자세히 확인해보기

`MessageWrapper` 객체를 `jsonMessage` 를 통해 생성할 때 `DeserializeObject` 함수를 실행하는데, 이 코드에서 문제가 생김이 틀림없었다.

`try ~ catch` 문을 이용해 자세한 `Exception` 을 검토하기로 했다.

### 변경된 코드

```csharp
public MessageWrapper(string json)
{
    try
    {
        MessageWrapper mw = JsonConvert.DeserializeObject<MessageWrapper>(json);
        action = mw.action;
        clientUUID = mw.clientUUID;
        data = mw.data;
    }
    catch (Exception ex)
    {
        Debug.LogError("Error deserializing MessageWrapper: " + ex.Message);
    }
}
```

### 확인된 에러

```csharp
Error deserializing MessageWrapper: Unable to find a constructor to use
for type WebSocketClient+MessageWrapper.
A class should either have a default constructor,
one constructor with arguments or a constructor marked with
the JsonConstructor attribute. Path 'action', line 1, position 10.
```

`constructor` 를 식별할 수 없는 문제였다. constructor는 함수의 생성자를 의미한다.

## 기본 생성자에 `[JsonConstructor]` 속성(attribute) 추가하기

`MessageWrapper` 객체를 생성할 때 쓰는 주 생성자에 `[JsonConstructor]` 속성을 추가해줌으로써 해결할 수 있었다.

```csharp
[JsonConstructor]
public MessageWrapper(string action, string clientUUID, object data)
{
    this.action = action;
    this.clientUUID = clientUUID;
    this.data = data;
}
```