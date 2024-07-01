---
title: "[Unity(C#)] TMPro_InputField 오브젝트에서 'Enter' 키 입력 받기"
excerpt: "TextMeshPro의 InputField 오브젝트에서 엔터 키 입력 핸들링하기"
layout: single
lang: ko
categories:
  - Unity
tags:
  - Unity
  - Key
---

```csharp
using UnityEngine;
using TMPro;

public class InputFieldEnterHandler : MonoBehaviour
{
    public TMP_InputField inputField;

    void Start()
    {
        if (inputField == null)
            inputField = GetComponent<TMP_InputField>();

        // Enter 키 입력을 감지하는 콜백 추가
        inputField.onEndEdit.AddListener(HandleEndEdit);
    }

    void HandleEndEdit(string text)
    {
        if (Input.GetKeyDown(KeyCode.Return) || Input.GetKeyDown(KeyCode.KeypadEnter))
        {
            Debug.Log("엔터 눌림");
            // 입력 처리 후 다시 포커스 설정
            inputField.ActivateInputField();
        }
    }
}
```