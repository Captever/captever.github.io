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


Go에서 패키지 내의 함수를 외부에서 접근할 수 있게 하려면 해당 함수의 이름이 대문자로 시작해야 합니다. 대문자로 시작하는 함수는 Go에서 "exported"라고 하여 다른 패키지에서 접근할 수 있습니다. 따라서 `handleConnections` 함수의 이름을 `HandleConnections`로 변경해야 합니다.

또한, `server` 패키지를 임포트할 때 실제 패키지 경로를 지정해야 합니다.

### 1. `server/connections.go` 파일 수정

`handleConnections` 함수를 `HandleConnections`로 변경합니다.

```go
package server

import (
    "log"
    "net/http"
    "project/models"
    "github.com/gorilla/websocket"
)

func HandleConnections(w http.ResponseWriter, r *http.Request) {
    ws, err := upgrader.Upgrade(w, r, nil)
    if err != nil {
        log.Fatalf("Error upgrading connection: %v", err)
        return
    }
    defer ws.Close()

    models.LobbyInstance.Clients[ws] = true
    log.Println("New client connected to the lobby")

    for {
        var msg models.Message
        err := ws.ReadJSON(&msg)
        if err != nil {
            log.Printf("Error reading message: %v", err)
            delete(models.LobbyInstance.Clients, ws)
            break
        }

        switch msg.Action {
        case "create_room":
            createRoom(ws, msg.RoomID)
        case "join_room":
            joinRoom(ws, msg.RoomID)
        default:
            log.Println("Unknown action")
        }
    }
}

func createRoom(ws *websocket.Conn, roomID string) {
    if _, exists := models.LobbyInstance.Rooms[roomID]; exists {
        ws.WriteJSON(models.Message{Data: "Room already exists"})
        return
    }

    models.LobbyInstance.Rooms[roomID] = &models.Room{
        Clients:      make(map[*websocket.Conn]bool),
        Broadcast:    make(chan models.Message),
        MasterClient: ws,
    }
    go handleMessages(roomID)
    log.Printf("Room created: %s", roomID)
    ws.WriteJSON(models.Message{Data: "Room created"})
}

func joinRoom(ws *websocket.Conn, roomID string) {
    room, exists := models.LobbyInstance.Rooms[roomID]
    if !exists {
        ws.WriteJSON(models.Message{Data: "Room does not exist"})
        return
    }

    room.Clients[ws] = true
    delete(models.LobbyInstance.Clients, ws)
    log.Printf("Client joined room: %s", roomID)

    for {
        var msg models.Message
        err := ws.ReadJSON(&msg)
        if err != nil {
            log.Printf("Error reading message: %v", err)
            delete(room.Clients, ws)
            break
        }
        room.Broadcast <- msg
    }
}

func handleMessages(roomID string) {
    room := models.LobbyInstance.Rooms[roomID]
    for {
        msg := <-room.Broadcast
        for client := range room.Clients {
            err := client.WriteJSON(msg)
            if err != nil {
                log.Printf("Error writing message: %v", err)
                client.Close()
                delete(room.Clients, client)
            }
        }
    }
}
```

### 2. `main.go` 파일 수정

`main.go` 파일에서 `server` 패키지를 임포트하고, `HandleConnections` 함수를 사용합니다.

```go
package main

import (
    "log"
    "net/http"
    "project/server"
)

func main() {
    http.HandleFunc("/ws", server.HandleConnections)
    log.Println("Server started on :8080")
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

### 요약

- `server` 패키지 내에서 함수 이름을 대문자로 시작하도록 변경하여 외부에서 접근할 수 있게 합니다.
- `main.go` 파일에서 `server` 패키지를 임포트하고, `HandleConnections` 함수를 사용합니다.

이제 `http.HandleFunc("/ws", server.HandleConnections)`에서 `undefined: server` 에러가 발생하지 않을 것입니다.