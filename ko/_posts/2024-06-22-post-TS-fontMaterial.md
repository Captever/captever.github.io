---
title: "[Unity(C#)] Trouble Shooting - 작업 환경을 바꿨을 때 Material 깨짐 현상 발생 시"
excerpt: "트러블 슈팅: 작업 환경을 바꿨을 때 TMPro 폰트 깨짐 현상"
layout: single
lang: ko
categories:
  - Unity
tags:
  - Unity
  - TroubleShooting
---


# 상황

업무용 노트북에서 항시 작업을 하다가, 본가에 오게 되면서 작업 환경이 바뀌게 되었다. `git clone` 을 통해 그동안 작업했던 파일들은 `.gitignore` 에 포함된 파일을 제외하고는 전부 받았다.

그런데 갑자기 폰트가 깨지기 시작했다. 본인의 경우 모든 폰트가 분홍색 네모로 바뀌었다.

# 문제

## Font Asset - Material

확인해보니, TMPro의 폰트 애셋(Font Asset)의 Material이 손상되어 있었다.

# 해결

## 손상된 Material 복원

기존 Material의 설정이 손상되었을 경우, 이를 재설정해 해결할 수 있다.

### **Material 속성 재설정**

- 손상된 Material을 선택하고 `Inspector` 창에서 `Shader`를 `TextMeshPro/Distance Field`로 설정합니다.

![FontAssetMaterial.png](/assets/resources/Unity/FontAssetMaterial.png)