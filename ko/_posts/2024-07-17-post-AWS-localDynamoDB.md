---
title: "[AWS(Amazon Web Service)] 윈도우 로컬 환경에서 AWS DynamoDB 사용하기"
excerpt: 윈도우 로컬 환경에서 8000번 포트로 DynamoDB 이용하기
layout: single
lang: ko
categories:
  - AWS
tags:
  - AWS
  - DynamoDB
---

# DynamoDB 로컬 다운로드

## Download DynamoDB for local

[컴퓨터에 로컬로 DynamoDB 배포](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html#DynamoDBLocal.DownloadingAndRunning.title)

1. 위 링크에 접근해 아카이브(`.tar.gz` 또는 `.zip`) 파일을 다운로드한다.
2. 압축을 해제한 후 원하는 디렉터리로 이동시킨다.
3. JRE 버전 확인
    
    ```bash
    java -version
    ```
    
    ![local_dynamoDB_1](/assets/resources/AWS/local_dynamoDB_1.png)
    
4. **자바 런타임 환경(Java Runtime Environment)** 버전 **JRE 17.x** 이상의 버전이 필요하므로 자신의 버전과 비교해 부족하다면 아래의 링크를 참고해 설치한다.
    1. [JRE 다운로드 공식 홈페이지](https://www.java.com/ko/download/ie_manual.jsp?locale=ko)
5. 압축을 해제한 디렉터리에 접근해 아래의 명령을 실행한다.
    
    ```bash
    java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -sharedDb
    ```
    
    1. 아래와 같이 `UnsupportedClassVersoinError` 에러가 발생한다면 **JDK** 버전을 업그레이드 해야한다. 아래의 항목을 참고해 설치하고 PowerShell 명령어로 설치한 경로에 맞게 환경 변수로 추가해준다.
        
        ![local_dynamoDB_2](/assets/resources/AWS/local_dynamoDB_2.png)
        
        1. [JDK 다운로드 공식 홈페이지](https://www.oracle.com/java/technologies/downloads/#jdk22-windows)
            
            ```powershell
            $env:JAVA_HOME="<installed_path>"
            $env:PATH="$env:JAVA_HOME\bin;$env:PATH"
            ```
            
6. 아래와 같이 나오면 성공이며, 이제부터 localhost의 8000 포트에 접근하면 DynamoDB 사용이 가능하다.
    
    ![local_dynamoDB_3](/assets/resources/AWS/local_dynamoDB_3.png)