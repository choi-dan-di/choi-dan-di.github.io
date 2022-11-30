---
title: "[GitHub] 다른 환경(Mac)에서 GitHub Blog 포스팅하기"
excerpt: "맥북에서 포스팅하는 방법 정리"

categories:
  - GitHub
tags:
  - [GitHub, git, github, command, pull, remote, mac]

permalink: /github/change-posting-env/

toc: true
toc_sticky: true

date: 2022-11-30 21:35:45+0900
last_modified_at: 2022-11-30 21:35:50+0900
---

## 👻 다른 환경에서 포스팅 하기
매일 집에서만 포스팅 했었는데 가끔 외부에서 작업해야 할 일이 종종 생긴다. 외부에서 작업할 땐 맥북을 이용한다. 그래서 맥북에서 원격 저장소에 접근하고 로컬 저장소와 연결하는 방법을 정리해 둘 것이다.

***

### 🌱 clone
**clone**을 이용하여 로컬 저장소를 추가 및 **remote**를 해준다.

```
git clone [Repository URL]
```

***

### 🌱 Ruby 설치
Jekyll은 루비를 기반으로 만들어져서 루비를 설치해야한다. 하지만 맥에는 이미 루비가 설치되어있으나 해당 루비는 지킬을 지원하지 않을 수도 있다. 그래서 따로 설치해줘야한다.

***

#### 🪐 Homebrew 설치
[Homebrew](https://brew.sh/index_ko.html)를 이용해 루비를 설치해 줘야한다. Homebrew는 **맥OS 용 패키지 관리자**이다. 해당 스크립트를 그대로 복사해서 터미널에 입력해준다.

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

***

#### 🪐 rbenv 설치
**rbenv(Ruby Version Manage)**는 루비를 버전별로 관리해주는 툴이다. 루비 버전을 독립적으로 사용할 수 있으며 자유롭게 오갈 수 있다.

```
brew install rbenv ruby-build
```

> 💡 **Error:homebrew-core is a shallow clone.**   
해당 에러가 뜨면 아래의 명령어를 이용하여 해결할 수 있다. 에러가 발생하는 원인은 2020년 10월부터 Homebrew가 더 이상 shallow clone을 생성하지 않기 때문이라고 한다.   
> 
> ```
> git -C /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core fetch --unshallow
> ```
> 
> 레포지토리 크기가 클수록 시간이 오래 걸리니 참고 기다려주자.

버전 확인은 ``` rbenv versions ```로 한다.

***

#### 🪐 Ruby 설치
알맞은 버전의 루비를 설치해야한다.

```
rbenv install -l
rbenv install [version]
rbenv versions
rbenv global [version]
```

``` -l ```로 설치 가능한 루비 버전을 확인하고 최신 버전을 설치해주었다. 포스팅 시점에선 **3.1.3**버전이 가장 최신이라 해당 버전으로 설치해주었다.

그런다음 **global**로 해당 버전으로 변경해주었다.

***

#### 🪐 PATH 설정
**rbenv PATH**를 추가해주어야한다. 쉘 종류에 따라 맞춰서 해주면된다. 난 맥이니까 **zsh** 버전의 경로를 추가해주었다.

```
vim ~/.zshrc
```

```
[[ -d ~/.rbenv ]] && \
export PATH=${HOME}/.rbenv/bin:${PATH} && \
eval "$(rbenv init -)"
```

코드를 추가했다면 **source**로 코드를 적용해야한다.

```
source ~/.zshrc
```

***

### 🌱 bundler 설치
이제 **gem**을 이용하여 번들러를 설치해주면된다.

```
gem install bundler
```

***

### 🌱 pull
그 이후부터는 **pull**을 이용하여 원격 저장소에 있는 내용으로 업데이트 해준 후 작업하면 된다. 브랜치 따는 거 잊지 말기!

```
git pull
```

***

## 👻 글을 마치며
전에 한 번 급하게 했었어서 정리를 못 했더니 그새 또 까먹어서 정리했다. 할 때마다 헷갈려서 급하게 하다가 잘못하면 어떡하나 걱정이 돼서 정리를 할 수 밖에 없는..🥲 그래도 계속 하다보니 머릿속에서 깃 사용법이 조금 더 정돈되는 느낌이 들어서 좋다. ~~근데 또 까먹을 듯~~

***

_출처_

_[Mac에서 Gem::FilePermissionError 에러 발생시 해결 방법](https://jojoldu.tistory.com/288)_   
_[Jekyll이란? 맥북에 설치하기](https://lamarr.dev/jekyll/2020/03/03/01.html)_