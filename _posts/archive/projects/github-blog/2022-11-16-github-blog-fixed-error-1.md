---
title: "[GitHub Blog] 포스팅 타임존 수정하기"
excerpt: "갑자기 포스팅 날짜가 이상해졌다..."

categories:
  - GitHub Blog
tags:
  - [GitHub Blog, github, jekyll, ruby]

permalink: /github-blog/fixed-error-1/

toc: true
toc_sticky: true

date: 2022-11-16 20:46:23+0900
last_modified_at: 2022-11-16 20:46:25+0900
---

## 👻 문제 발생
갑자기 포스팅 날짜가 이상해졌다. 분명히 그저께까지는 잘 됐던 것 같은데 오늘(2022년 11월 16일 오후) 포스팅을 한 글들이 모두 2022년 11월 17일로 올라간 것이다. md파일의 이름에도 이상이 없는데 +9시간이 더 더해져서 보이는 것이다.

음, 이미 config 파일의 타임존도 ``` Asia/Seoul ```로 되어있는데.. 뭐가 문제지?

![Alt Text](/assets/images/posts_img/projects/github-blog/fixed-error-1/posting.PNG)   

![Alt Text](/assets/images/posts_img/projects/github-blog/fixed-error-1/error2.PNG)   

뭔가 계산이 중복으로 되고 있는 것 같다. 왜 그러는지 구글링을 통해 찾아보았다.

***

## 👻 원인
거의 1시간 넘게 구글링을 통해 찾아보았다. 다른 해결법은 타임존을 설정하라는 말들이 많았는데 난 이미 타임존이 아시아/서울로 되어있어서 찾는데 애 좀 먹었다.


_[👉 해당 문제에 관한 이슈-1 👈](https://github.com/jekyll/jekyll/issues/1069)_   
_[👉 해당 문제에 관한 이슈-2 👈](https://github.com/jekyll/jekyll/issues/7550)_

위 사이트를 통해 이슈가 예전부터 많았다는 것을 알 수 있지만.. 정확한 원인은 봐도 모르겠다 😂 아마도 데이트와 데이트타임 형식이 나눠져서 거기서 오는 혼동 때문이 아닌가 싶다. 날짜를 다루는 타입은 아주 다양하니..

결론을 보자면 시간대를 무조건 설정해주라는 해결법이 다수였다. 그런걸 보면 위에서 말한대로 날짜 포맷과 날짜, 시간 포맷이 서로 다르게 작동되는 게 이번 문제의 원인이지 않았나 싶다.

***

## 👻 해결
_[👉 내가 찾은 해결법 👈](https://nnoco.github.io/blog/2020/05/02/Jekyll-%ED%83%80%EC%9E%84%EC%A1%B4-%EC%9D%B4%EC%8A%88.html)_

![Alt Text](/assets/images/posts_img/projects/github-blog/fixed-error-1/fixed.PNG)   

나는 md 파일의 Front Matter에 ``` date ```를 수정하는 것으로 이 문제를 해결했다. 원래는 타임존 오프셋 없이 ``` 2022-11-16 21:34:32 ``` 포맷을 사용하였으나 ``` 2022-11-16 21:34:38+0900 ``` 이렇게 뒤에 오프셋을 붙임으로써 문제를 해결할 수 있었다.

![Alt Text](/assets/images/posts_img/projects/github-blog/fixed-error-1/result.PNG)   

내가 설정한 시간대로 게시글이 올라가있는 것을 확인할 수 있었다.

***

## 👻 글을 마치며
분명히 그저께까지는 잘 올라왔었던 것 같은데.. 이게 하루 아침에 이상해질 수 있나? 하고 많은 생각이 들었다. 어쩌면 처음부터 이러한 이슈가 있었지만 발견 못 했을 수도 있고.. 그래도 결국은 원인을 찾고 문제를 해결할 수 있어서 다행이었다. 아무래도 소스코드로 이루어진 블로그다보니 문제가 발생하면 어디에서 원인을 찾아야 하는지가 좀 힘든 부분인 것 같다. 코드를 뜯어볼 수 있으면 좋으련만.. 그래도 오늘 원인 찾아본다고 site 데이터도 찍고 post 데이터도 찍어보았다. ~~건진 건 없지만~~   
후..이제 전체 포스팅 시간 오프셋 수정하러 가야겠다 ^^