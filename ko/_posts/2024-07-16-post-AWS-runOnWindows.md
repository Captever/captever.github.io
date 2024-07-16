---
title: "[AWS(Amazon Web Service)] 윈도우 환경에서 localhost로 AWS 서비스 이용하기"
excerpt: 윈도우 환경에서 localhost로 AWS CLI 설치 진행하기
layout: single
lang: ko
categories:
  - AWS
tags:
  - AWS
  - localhost
---

# AWS CLI 설치하기

## 설치 파일 다운로드

[AWS CLI 공식 설치 링크](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

위 링크에 접속

### Windows 클릭
![aws_cli_install_1](/assets/resources/AWS/aws_cli_install_1.png)

### `msiexec` 명령어를 이용하거나, `.msi` 하이퍼링크를 통해 설치

![aws_cli_install_2](/assets/resources/AWS/aws_cli_install_2.png)

### CMD 환경을 이용해 `aws --version` 명령어로 정상적으로 설치됐는지 확인

주의해야 할 점은, **Visual Studio Code** 와  **명령 프롬프트(CMD)** 어떤 걸 통해서 진행하든 한 번 종료한 후 다시 실행해야 명령어가 제대로 작동한다.

![aws_cli_install_3](/assets/resources/AWS/aws_cli_install_3.png)

```bash
aws --version
aws configure
```