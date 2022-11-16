---
title: "[GitHub] 깃(Git)과 깃헙(GitHub)의 차이"
excerpt: "깃(Git)과 깃헙(GitHub)의 차이에 대해서 알아보기"

categories:
  - GitHub
tags:
  - [GitHub, git, github]

permalink: /github/difference-git-github/

toc: true
toc_sticky: true

date: 2022-11-09 21:06:39+0900
last_modified_at: 2022-11-09 21:06:42+0900
---

## 👻 GitHub이란?
![Alt Text](/assets/images/posts_img/basics/github/difference-git-github/github.png)   

개발자라면 한 번 쯤은 들어봤을 것이다. **깃헙(GitHub)**은 [루비 온 레일즈(Ruby on Rails)](https://ko.wikipedia.org/wiki/%EB%A3%A8%EB%B9%84_%EC%98%A8_%EB%A0%88%EC%9D%BC%EC%A6%88)로 작성된 _분산 버전 관리툴_ 인 **깃** 저장소 호스팅을 지원하는 웹 서비스이다. 한 마디로 저장소를 관리하기 위한 웹이라고 볼 수 있다. 

대부분의 규모가 큰 프로젝트는 혼자로는 부족해 협업을 통해 진행하는 게 대다수이다. 그러다보니 자연스레 **프로젝트의 버전 관리**도 뒤따라오게 되었는데 여기서 나올법한 여러가지 문제점을 깃과 깃헙이 해소시켜준 것이다. 로컬 파일을 깃헙 클라우드에 push(업로드)하여 서로 다른 위치에 있는 여러 사용자가 작업할 수 있다.

***

## 👻 Git이란?
**깃(Git)**은 컴퓨터 파일의 변경사항을 추적하고 여러 명의 사용자들 간에 해당 파일들의 작업을 조율하기 위한 [스냅샷 스트림](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F#:~:text=more%20like%20a-,stream%20of%20snapshots,-.) 기반의 **분산 버전 관리 시스템**이다. 또는 이러한 명령어를 가리킨다.

> 💡 **분산 버전 관리 시스템(VCS : Version Control System)**   
소프트웨어 버전 관리를 위한 시스템이다. 이 시스템은 각 개발자가 중앙 서버에 접속하지 않은 상태에서도 코드 작업을 할 수 있는 것이 특징이다.

***

## 👻 깃과 깃헙은 같을까?
**둘은 엄연히 다르다.**   
파일 관리를 한다는 점에선 동일하지만 범위가 다르다. 깃으로 로컬 저장소에 있는 파일을 관리하고, 깃헙에 저장하여 여러 사람들과 분산 업무를 할 수 있게 해주는 식이라고 보면 될 것 같다.   
<span style="font-size: 0.7em; color: gray;">깃이 커피라면 깃헙은 커피샵이다.</span>

![Alt Text](/assets/images/posts_img/basics/github/difference-git-github/github2.png)   

***

## 👻 글을 마치며
이름에 둘 다 깃이 들어가서 헷갈릴법도 하지만 허브(Hub)라는 단어의 의미를 잘 생각해보면 구분하기는 쉽다. 깃으로 버전을 관리한 후에 깃헙에 올려두고, 깃헙에 있는 파일을 다른 사용자가 내려받아 또다시 깃으로 관리를 하는 식이라고 이해하면 될 것 같다. 처음엔 두 단어의 차이점을 정확히 알지못해 혼용해서 썼었는데 이제는 정확히 구분지어 사용할 수 있을 것 같다. 오늘도 이렇게 지식 하나 적립 완료!