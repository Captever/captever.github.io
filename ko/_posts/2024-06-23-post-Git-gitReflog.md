---
title: "[Git(깃)] Git reflog - 잃어버린 commit을 찾아서..."
excerpt: "Git reflog 명령어: 잃어버린 commit을 찾는 방법"
layout: single
lang: ko
categories:
  - Git
tags:
  - Git
---


# 상황

main 브랜치와 dev_webSocket 브랜치가 따로 존재했다.

main 브랜치에서 커밋을 하나 진행했고, dev_webSocket에서도 커밋을 하나 진행했다.

이후 병합을 위해 `git checkout main` 명령어로 main 브랜치로 이동하려고 하니, dev_webSocket에서 커밋을 하지 않은 다른 파일들을 먼저 처리하라고 지시했다.

그래서 dev_webSocket 브랜치에서 `git stash` 를 진행하고 `git checkout main` 을 다시 실행해 `git merge origin/dev_webSocket` 을 실행했다.

그랬더니 갑자기 `dev_webSocket` 브랜치에서 commit 했던 내용이 증발했다.

`git log` 를 해봐도, `git log --branches` 를 해봐도 온데간데 없었다.

`dev_webSocket` 브랜치로 다시 이동해서 `git log` 를 해봐도 커밋이 사라져있었다.

혹시나  `git stash` 에 빨려들어갔나 싶어 `git stash pop` 을 진행했지만, 내 사라진 코드는 돌아오지 않았다.

# 해결책

`git reflog` 를 통해 그동안 HEAD의 행방을 추적해, 해당 commit이 진행됐던 `commit-hash` 코드를 알아낼 수 있었고, `git checkout <target-commit-hash>` 를 이용해 복구 및 push를 진행할 수 있었다.

![git-reflog.png](/assets/resources/TroubleShooting/git-reflog.png)