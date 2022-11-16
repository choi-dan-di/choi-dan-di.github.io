---
title: "[GitHub Blog] #1. 깃헙 블로그 만들기"
excerpt: "깃헙 계정으로 블로그를 만들어보기"

categories:
  - GitHub Blog
tags:
  - [GitHub Blog, github, jekyll, ruby]

permalink: /github-blog/gb-1/

toc: true
toc_sticky: true

date: 2022-11-04 05:05:05+0900
last_modified_at: 2022-11-04 05:05:05+0900
---

## 👻 GitHub Blog 만들기
우선 깃헙 블로그를 만들려면 깃헙 계정이 있어야한다.

[👉 계정 생성 방법은 요기로 👈](/github/create-account)

***

### 🌱 Repository 생성
![Alt Text](/assets/images/posts_img/projects/github-blog/gb-1/repo-1.PNG)   
대시보드 화면에서 좌측에 보면 ```Create repository``` 버튼이 있다. **클릭!!**   

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-1/repo-2.PNG)   
입력할 것은 딱히 없다. 대신 저장소 이름은 규칙이 있어서 이것만 지켜주면 된다.   

> **username.github.io**

계정을 생성하면서 지어줬던 ```username```에 ```.github.io```를 꼭 붙여서 형식에 맞게 입력해줘야한다. 그러면 깃헙이 알아서 페이지로 인식하고 블로그를 만들어주게 된다. 유저네임을 입력하는만큼 계정 당 블로그 하나만 생성 가능하니 참고하도록 하자.   

```Add a README file```도 체크하자. 어차피 새 저장소를 만들면 리드미라는 파일을 생성한 후에 커밋을 하라고 하니 미리 만들어두자.   

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-1/repo-3.PNG)   
위의 화면이 나온다면 벌써 반은 왔다. 이제 잠시 새 탭을 열어 주소창에 ```https://username.github.io```를 입력해보자.   
**README.md** 파일에 있는 내용이 출력되는 것을 볼 수 있을 것이다.

***

### 🌱 Git을 이용해서 로컬 저장소와 연결짓기
깃헙에 저장소(Repository)를 방금 만들었는데, 이건 **원격 저장소**라고 부른다. 깃이 만들어 둔 저장소에 내 파일들이 올라가있으니 현재 내 PC에는 파일들이 없는 상태다. 이제 이 원격 저장소에 있는 파일을 로컬 저장소로 복사를 해보자. **git command**는 ```clone```을 사용한다. (자세한 깃 커맨드는 따로 포스팅 할 예정이다.)   

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-1/repo-4.PNG)   
파일 탐색기를 열기 전 원격 저장소 경로 하나만 복사하고 진행하자.   

이 파일들을 저장할 로컬 경로를 하나 지정한 다음 **cmd 창**을 켜서 해당 폴더로 경로를 옮겨주자. 아니면 해당 폴더 안, **파일 탐색기**에 cmd를 입력해서 창을 열어도 된다. 나는 D드라이브 > GitRepo 폴더 내에서 진행했다.   

> C드라이브에서 D드라이브로 경로를 이동하고 싶으면 ```D:```를 입력하면 된다.

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-1/cmd-1.PNG)   

이렇게 경로를 직접 이동해주거나   

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-1/cmd-2.PNG)   
![Alt Text](/assets/images/posts_img/projects/github-blog/gb-1/cmd-3.PNG)   

이렇게 파일 탐색기에서 직접 접근도 가능하다. (엔터 쳐야함)

이제 커맨드 창에 명령어를 쳐주면 된다.   

```
git clone 복사한 주소
```

그러고 로컬 저장소를 확인해보면   

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-1/local-1.PNG)   

짜잔~ 원격 저장소에 있는 파일이 그대로 복사가 잘 되어 로컬 저장소에 저장이 된 걸 확인할 수 있다.

***

### 🌱 원격 저장소에 파일 push해보기
이제 로컬 저장소에 있는 파일을 수정한 다음 원격 저장소로 올려보자.   
해당 폴더로 들어가서 ```Hello World```가 적힌 ```index.html``` 파일을 하나 만들어준다.   

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-1/cmd-21.PNG)   
![Alt Text](/assets/images/posts_img/projects/github-blog/gb-1/cmd-22.PNG)   

로컬 저장소에 생긴 걸 확인한 후   

```
git add -A
git commit -m "커밋 메세지"
git push -u origin main
```

위 코드를 한 줄씩 차례대로 입력해주자.   

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-1/cmd-23.PNG)   

난 에러가 났다.   
찾아보니 이 게시글을 쓰면서 계정을 새로 하나 만들고 깃 초기세팅을 변경 없이 진행해서 난 권한 에러였다. 지금 포스팅을 하고 있는 깃헙 계정으로 새 저장소에 접근하려니 403 에러가 뜬 것이다. 난 remote url만 변경해서 비밀번호 다시 입력하고 push를 다시 진행시켰다.

> remote url 변경으로 에러 해결   

```
git remote set-url origin https://[USERNAME]@github.com/[USERNAME]/[REPOSITORY].git
```

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-1/repo-5.PNG)   

```index.html```이 성공적으로 올라간 것을 볼 수 있다! (근데 왜 내 본계정으로 된거지..?)

***

## 👻 글을 마치며
아무튼 이렇게 깃헙 계정으로 블로그를 만들고 커밋까지 하는 방법을 공부해봤다. 마지막에 뜬 에러는 조금 더 살펴볼 필요가 있겠다. 권한이 왔다갔다 거린 것 같은데.. 다시 돌려야 할 것 같기도 하고 ㅠㅠ 그래도 깃헙 블로그 만들기로 결정하고 여기저기 구글링 열심히 하면서 만들어두면 되게 뿌듯하다! 블로그 생성하고 테마까지 적용하는 데 20시간은 걸렸던 것 같다.. 부계정으로 다시 잘 할 수 있을 지도 고민이다. 😂 다음 시간엔 jekyll 테마 적용하는 법까지 정리해보도록 하겠다.
