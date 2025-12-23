---
title: "[Git] Git Command 정리"
excerpt: "자주 사용하는 Git Command 정리"

categories:
  - GitHub
tags:
  - [GitHub, git, github, command]

permalink: /github/git-command/

toc: true
toc_sticky: true

date: 2022-11-19 13:32:13+0900
last_modified_at: 2022-11-19 13:32:15+0900
---

## 👻 Git Command 정리
간단하게 **Git Command**를 정리해두려고 한다. ~~정리 안 하면 까먹으니까 😋~~

***

### 🌱 Git 유저 정보
- **현재 사용자 아이디 확인**   
```
git config user.name
```

- **현재 사용자 이메일 확인**   
```
git config user.email
```

- **현재 사용자 아이디 설정**   
```
git config user.name '설정할 아이디'
```

- **현재 사용자 이메일 설정**   
```
git config user.email '설정할 이메일'
```

***

### 🌱 디렉토리 관리
- **로컬 저장소 생성**   
```
git init
```

- **로컬에서 수정한 파일 스테이징(Staging)**   
```
git add 파일명
git add .   // 수정한 파일 전체
```

- **스테이징된 파일 내리기**   
```
git reset 파일명
```

- **로컬 커밋(Commit)**   
```
git commit -m "커밋 메세지"
```

- **로컬 스테이징+커밋**   
이 경우 기존에 한 번이라도 커밋한 적이 있는 경우에 사용한다. (add+commit)
```
git commit -am "커밋 메세지"
```

***

### 🌱 GitHub 사용
- **원격 저장소 주소 연동**   
```
git remote add origin 레포지토리 주소
```

- **원격에서 로컬로 복제**   
```
git clone 복제할 레포지토리 URL
```

- **원격 저장소로 첫 푸시(push)**   
이후는 ``` git push ``` 사용   
```
git push origin main
```

- **특정 브랜치에 푸시(push)**   
```
git push origin 브랜치 이름
```

- **특정 브랜치에 풀(pull)**   
```
git pull origin 브랜치 이름
```

***

### 🌱 Branch 사용
- **현재 브랜치 확인**   
```
git branch
```

- **새 브랜치 생성**   
```
git branch 브랜치 이름
```

- **브랜치 이동**   
```
git checkout 이동할 브랜치 이름
```

- **새 브랜치 생성 및 이동**   
```
git checkout -b 새 브랜치 이름
```

- **브랜치 삭제**   
```
git branch -d 삭제할 브랜치 이름
```

- **브랜치 강제 삭제**   
```
git branch -D 브랜치이름
```

- **브랜치 머지(merge)**   
현재 위치한 브랜치에 해당 브랜치를 머지시킨다.   
```
git merge 브랜치 이름
```

- **머지 취소 및 이전 상태로 돌아가기**   
머지를 하다가 충돌이 발생했을 때, 일단 머지 작업을 취소하고 이전 상태로 돌아간다.
```
git merge --abort
```

***

### 🌱 되돌리기 및 취소하기
- **스테이징 전 수정 파일 되돌리기**   
```
git checkout -- 파일이름
```

- **최신 커밋 취소(스테이징도 같이 취소)**   
```
git reset HEAD^
```

- **특정 커밋으로 되돌리기(이후 커밋 삭제)**   
특정 커밋의 해쉬값은 ``` git log ```를 통해 가져오기   
```
git reset --hard SpecificCommitHashValue
```

- **특정 커밋으로 되돌리기(이후 커밋 미삭제)(Revert)**   
특정 커밋의 해쉬값은 ``` git log ```를 통해 가져오기   
```
git revert SpecificCommitHashValue
```

***

## 👻 글을 마치며
일단은 당장 쓸 것 같은 것만 따로 정리해두었다. 안 그러면 계속 까먹고 그 때마다 찾아봐야해서 😅 얼른 이거 다 외우고 다른 것도 또 정리해서 다 외워야지!

***

_출처_

_[Git Command 정리-1](https://ssminji.github.io/2021/07/09/basic-git-command/)_   
_[Git Command 정리-2](https://velog.io/@cco2416/Git-Command-%EC%A0%95%EB%A6%AC)_   