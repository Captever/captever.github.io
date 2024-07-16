---
title: "[AWS(Amazon Web Service)] 윈도우 로컬 환경에서 AWS S3 사용하기"
excerpt: 윈도우 로컬 환경에서 9000번 포트로 S3 이용하기
layout: single
lang: ko
categories:
  - AWS
tags:
  - AWS
  - S3
---

# MinIO 서버 다운로드

아래의 링크를 통해 **MinIO** 서버 바이너리 파일을 다운로드한다.

[MinIO 서버 다운로드 공식 홈페이지](https://min.io/download#/windows)

![local_s3_1](/assets/resources/AWS/local_s3_1.png)

# MinIO 서버 실행

## 바이너리 파일 위치 변경

![local_s3_2](/assets/resources/AWS/local_s3_2.png)

다운로드한 바이너리 파일을 원하는 디렉토리에 배치하고, `/data` 디렉터리를 생성한다.

## 바이너리 파일 실행

만든 `/data` 디렉터리를 스토리지로 설정해 minIO 서버를 실행한다.

```bash
minio.exe server ./data
```

# 기본 정보

1. 기본적으로 `http://localhost:9000`에서 실행된다.
2. 콘솔 인터페이스를 통해 웹 브라우저에서 버킷 생성 등 MinIO를 관리할 수 있다.
    - 기본 관리자 사용자 이름: `minioadmin`
    - 기본 관리자 비밀번호: `minioadmin`