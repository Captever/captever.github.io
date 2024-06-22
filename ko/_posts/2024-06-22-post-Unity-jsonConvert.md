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


# 상황

```go
// 공통 메시지
type MessageWrapper struct {
	Type string          `json:"type"`
	Data json.RawMessage `json:"data"`
}
```

서버에서 `MessageWrapper` 값을 먼저 받아, `Type` 값을 읽은 후 `Data` 값이 적절한 구조체로 연결될 수 있도록 만들고 싶었다.

Unity 환경에서 서버로 `JSON` 형식의 데이터를 보내는 데엔 크게 두 가지가 존재한다.

제일 잦게 사용될 오브젝트이기에, 빠른 메소드를 찾고자 했다.

## JsonUtility.ToJson

- Unity에 내장되어 별도의 설치가 필요 없다.
- 경량화 된 메소드로 속도가 빠른 대신 복잡한 JSON 변환 시 제약이 발생한다.
- 동적 필드 처리나 고급 기능은 정상적으로 변환하지 못한다.

## JsonConvert.SerializeObject

- `Newtonsoft.Json` 패키지를 따로 이용해야 한다.
- 기능이 풍부해 속도가 느린 대신 다양한 JSON 변환 옵션이 있어 복잡한 변환에도 사용이 가능하다.
- 동적 필드 처리나 고급 기능 모두 이용할 수 있다.


# 초기에 사용했던 코드: JsonUtility.ToJson을 이용한 코드

```csharp
public void ConnectToMaster()
{
    SendDataToServer("ConnectToMaster",
	    JsonUtility.ToJson(new ConnectionMessage(GameManager.Instance.PlayerNickname)));
}

// {...}

private void SendDataToServer(string type, string jsonData)
{
    if (ws.ReadyState == WebSocketState.Open)
    {
        var jsonMessage = new MessageWrapper(type, ClientUUID, jsonData).ToJson();
        //Debug.Log("wrapper to json: " + jsonMessage);
        ws.Send(jsonMessage);
    }
    else
    {
        Debug.LogWarning("WebSocket is not open. Data not sent: " + type + " | " + jsonData);
    }
}

// {...}

[System.Serializable]
public class MessageWrapper
{
    public string type;  // 어떤 메시지인지 식별할 수 있는 헤더
    public string clientUUID; // 누가 보낸 메시지인지 식별하기 위함
    public string data;  // 메시지의 실제 데이터

    public MessageWrapper(string type, string clientUUID, string data)
    {
        this.type = type;
        this.clientUUID = clientUUID;
        this.data = data;
    }

    public string ToJson()
    {
        return JsonUtility.ToJson(this);
    }
}
```

`object` 형식의 변수를 사용하면 다양한 클래스의 변수를 입력받을 수 있어 활용이 가능하지만, `JsonUtility` 를 이용하게 되면 `object` 형식의 변수는 아예 제외하고 JSON 형식으로 바꿔주기 때문에, `string` 형식의 멤버 변수로 지정하고 먼저 변환한 후 한 번 더 변환을 거쳐야만 데이터가 옮겨진다.

> **근데 여기서 문제가 발생한다.**
> 

변환을 한 번 거쳐 문자열이 되면 `{"clientNickname":"asdfqwer"}` 이런 꼴로 바뀌는데, 먼저 이 문자열 데이터를 `data` 라는 멤버 변수에 저장한다. 해당 데이터를 저장한 `MessageWrapper` 객체를 한 번 더 `JsonUtility` 를 먹이면 시스템에서 `"` 부분까지 인식하게 하기 위해 자동으로 `\"` 로 변환해 JSON으로 바꿔준다.

예를 들면 이런 식이다.

`{"type":"ConnectToMaster","clientUUID":"","data":"{\"clientNickname\":\"asdfqwer\"}"}`

뒷부분을 자세히 보면 `"{\"clientNickname\":\"asdfqwer\"}"` 로 바뀌어있는 걸 볼 수 있다.

> 이런 식으로 데이터가 전송되면 **서버에선 제대로 값을 받을 수 없다.**
> 

# 결국 무거운 `JsonConvert` 를 이용해야 했다.

```csharp
// JSON 데이터 송신 호출 함수
SendDataToServer("ConnectToMaster", new ConnectionMessage(GameManager.Instance.PlayerNickname));

// 문제가 발생하는 부분
private void SendDataToServer(string type, object jsonData) {
	// ...
	var jsonMessage = new MessageWrapper(type, ClientUUID, jsonData).ToJson();
}

// string 형식을 object로 변경
// JsonConvert를 이용한 ToJson 멤버 함수 구현
public class MessageWrapper
{
    public object data;  // 메시지의 실제 데이터

    public string ToJson()
    {
        return JsonConvert.SerializeObject(this);
    }
}
```

`object` 형식으로 클래스 변수를 고유 형식 그대로 받아들인 뒤 `JsonConvert.SerializeObject` 를 이용해 한 번에 JSON 데이터로 변환한다.