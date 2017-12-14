---
layout: post
title: git 자주 쓰는 명령어 모음 
categories: git
tags: [git]
---

이 포스트는 자주 쓰는 git 명령어를 적어놓는 용도이다.

계속 수정될 것 같은 포스트이다.

## 저장소 추가
```bash
$ git remote add [저장소 이름] [저장소 주소]
```

## pull > refusing to merge unrelated histories 에러 발생 시
```bash
$ git pull origin master [브랜치 명] --allow-unrelated-histories
```
## branch 생성
브랜치만 생성
```bash
$ git branch [브랜치 명]
```

브랜치 체크아웃
```bash
$ git checkout [브랜치 명]
```

브랜치 생성 후 체크아웃
```bash
$ git checkout -b [브랜치 명]
```


- 참고
  - [git-scm.com > Git의-기초-리모트-저장소](https://git-scm.com/book/ko/v1/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EB%A6%AC%EB%AA%A8%ED%8A%B8-%EC%A0%80%EC%9E%A5%EC%86%8C)
  - [http://cpdev.tistory.com > git pull 에러](http://cpdev.tistory.com/51)
