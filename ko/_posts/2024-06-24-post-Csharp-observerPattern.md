---
title: "[Unity(C#)] C# 문법 - Observer 패턴"
excerpt: C# 에서 Observer 패턴 활용하기
layout: single
lang: ko
categories:
  - Csharp
tags:
  - Csharp
  - Observer Pattern
---


# 계기
UI를 토글해야 했다. `enum` 객체를 하나 두고, 값이 바뀔 때마다 UI를 토글로 껐다 켜는 작업을 해야했다.

한 스크립트에서 한다면 간단했겠지만, 싱글턴 인스턴스로 지정된 `GameManager` 스크립트에서 Scene이 바뀌어야만 비로소 보이는 각 Scene별 `SceneManager` 스크립트에 접근해야 했다.

나에겐 두 가지 선택지 밖엔 생각나지 않아, 찾아보았다. 먼저 내가 생각한 선택지는 이거였다.

## 1번째 - GameManager에서 MainSceneManager 컴포넌트를 불러와 UI를 토글하는 방법

싱글턴 스크립트 객체인 `GmaeManager`에서 MainScene을 관리하는 `MainSceneManager` 스크립트 컴포넌트를 불러와 씬 매니저 스크립트의 멤버 함수를 실행하는 것이다.

하지만 아무리 생각해도 싱글턴 오브젝트가 여러 Scene에 걸쳐 유지되는데, `MainSceneManager` 컴포넌트를 물고 다녀야 된다는 게 찝찝했고, 메모리에 있어서 낭비라고 생각이 들었다.

## 2번째 - MainSceneManager에서 enum 값이 바뀌는 걸 감지해 UI를 토글하는 방법

`MainSceneManager`에서 `Update` 함수 등을 이용해 `enum` 값이 변경됐는지 매 프레임마다 확인해, 변경됐다면 현 상태에 맞게 UI를 토글한다.

이 방법은 말만 들어도 부하도 크고 비효율적이다.

# Observer 패턴

더 효율적인 방법으로 Observer 패턴을 사용하면 된다.

Observer 패턴을 사용하면 상태 변화가 있을 때만 이벤트를 실행해 불필요한 성능 저하를 막을 수 있다.

## 구현 방법

1. **사전 설정**

GameManager가 상태 변경 이벤트(`event`)를 발생시키도록 설정하고, `MainSceneManager`가 해당 이벤트를 **핸들(handle)** 하도록 설정한다.
   
2. **Event 선언**

```csharp
public enum GameState
{
    InLobby,
    InRoom,
    InGame
}

public class GameManager : SingletonObj<GameManager>
{
    public delegate void StateChangedHandler(GameState newState);
    public event StateChangedHandler OnStateChanged;

    // About GameState
    public GameState CurrentState { get; private set; }
    public void SetState(GameState newState)
    {
        if (CurrentState != newState)
        {
            CurrentState = newState;
            OnStateChanged?.Invoke(newState);
        }
    }
}
```

3. **MainSceneManager에서 이벤트 핸들링(Handling)**

```csharp
public class MainSceneManager : MonoBehaviour
{
    [SerializeField] private GameObject grpInLobby;
    [SerializeField] private GameObject grpInRoom;
    [SerializeField] private GameObject grpInGame;

    private void OnEnable()
    {
        GameManager.Instance.OnStateChanged += HandleStateChanged;
    }

    private void OnDisable()
    {
        GameManager.Instance.OnStateChanged -= HandleStateChanged;
    }

    private void HandleStateChanged(GameState newState)
    {
        grpInLobby.SetActive(newState == GameState.InLobby);
        grpInRoom.SetActive(newState == GameState.InRoom);
        grpInGame.SetActive(newState == GameState.InGame);
    }
}
```

### 구현 정리

`GameManager`에서 상태가 변경될 때마다 `OnStateChanged` 이벤트를 발생시키고, `MainSceneManager`에서는 이 이벤트를 유지해 상태 변화에 따라 UI를 업데이트할 수 있다.

**Observer 패턴**을 사용함으로써 이벤트 호출이 발생했을 때 딱 필요한 작업만 수행하게 했고, 성능 저하를 최소화할 수 있었다.


## 한 줄 한 줄 해부하기

좀 더 자세히 코드를 살펴보자.

### GameManager.cs

```csharp
public delegate void StateChangedHandler(GameState newState);
```

`delegate`는 C#에서 함수의 형식을 정의하는 타입이다. `StateChangedHandler`라는 이름의 `delegate`를 정의하고, 이는 `GameState` 타입의 인자를 받을 수 있도록 사전 구성을 한다.

```csharp
public event StateChangedHandler OnStateChanged;
```

`event`는 `delegate` 인스턴스를 다루기 위한 타입으로, `OnStateChanged` 이벤트는 `StateChangedHandler`는 `delegate` 형식이며, 이 이벤트는 상태 변경 시 호출될 함수들을 등록하는 데 사용한다.

```csharp
public void SetState(GameState newState)
{
    if (CurrentState != newState)
    {
        CurrentState = newState;
        OnStateChanged?.Invoke(newState);
    }
}
```

`SetState` 함수는 `CurrentState` 값을 변경하며, 상태가 변경되면 `Invoke`를 호출해 `OnStateChanged` 이벤트를 발생시킨다. 그러면 등록된 모든 함수들을 호출하며, `OnStateChanged?`를 사용해줌으로써 `OnStateChanged` 이벤트가 null이 아닐 때만 호출이 발생한다.

### MainSceneManager.cs

```csharp
private void OnEnable()
{
    GameManager.Instance.OnStateChanged += HandleStateChanged;
}
```
`OnEnable` 메서드는 `MonoBehaviour`의 기본 메서드로써, 객체가 활성화될 때 호출된다.

여기서는 `GameManager`의 `OnStateChanged` 이벤트에 `HandleStateChanged` 함수를 추가한다. 

이 작업을 함으로써 상태가 변경될 때 `HandleStateChanged` 메서드가 호출된다.

```csharp
private void OnDisable()
{
    GameManager.Instance.OnStateChanged -= HandleStateChanged;
}
```

`OnDisable` 메서드는 객체가 비활성화될 때 호출되며, 여기서는 `OnStateChanged` 이벤트에서 `HandleStateChanged` 함수를 제거한다.

이 작업을 함으로써 메모리 누수나 불필요한 호출을 방지할 수 있다.

```csharp
private void HandleStateChanged(GameState newState)
{
    grpInLobby.SetActive(newState == GameState.InLobby);
    grpInRoom.SetActive(newState == GameState.InRoom);
    grpInGame.SetActive(newState == GameState.InGame);
}
```

`HandleStateChanged` 메서드는 `GameManager`에서 상태가 변경될 때 호출되며, `newState` 값을 받아와 각 상태에 맞게 UI 요소들을 활성화하거나 비활성화하는 실질적인 작업을 진행한다.