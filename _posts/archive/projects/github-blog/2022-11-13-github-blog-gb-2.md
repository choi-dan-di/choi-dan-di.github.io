---
title: "[GitHub Blog] #2. 블로그에 jekyll 테마 적용하기"
excerpt: "깃헙 블로그에 jekyll 테마 적용해보기"

categories:
  - GitHub Blog
tags:
  - [GitHub Blog, github, jekyll, theme, chirpy, minimal-mistakes, ruby]

permalink: /github-blog/gb-2/

toc: true
toc_sticky: true

date: 2022-11-13 15:36:47+0900
last_modified_at: 2022-11-13 16:47:53+0900
---

## 👻 jekyell이란?
지난 시간엔 깃헙 블로그를 만들어 배포해서 접속하는 것까지 만들어보았다. 이번 시간엔 jekyll 테마를 적용해보자.

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-2/jekyll-logo.png)   

**jekyll(지킬)**은 **정적 사이트 생성기**이다. 깃헙 자체적으로 Jekyll Contents Management System을 내장하고 있어서 호스팅에 적합하다. 지킬은 개발자들이 애용하는 깃헙에서 개발한 툴로 마크업 언어로 글을 작성하면 이것을 미리 정의해 놓은 규칙에 따라 다양한 레이아웃으로 포장하여 정적 웹사이트를 만들어 준다. 빠르고 가벼운 것이 특징이며 기본적으로 초기 설정을 해둔 다음 마크다운 언어만으로 블로그 포스팅이 가능하기 때문에 한 번 익혀놓으면 아주 편리하다.

***

### 🌱 테마 정하기
지킬 테마를 모아둔 사이트가 여러개 있다. 그 중에서 자기가 마음에 드는 테마를 정하면 되는데, 나는 초기에 [chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) 테마를 적용했다가 더 이쁜 걸 발견해버려서 한 번 갈아엎었었다.

> 지킬 테마 추천 사이트   
- <https://jamstackthemes.dev/ssg/jekyll/>
- <http://jekyllthemes.org/>
- <https://jekyllthemes.io/>
- <https://jekyll-themes.com/>

***

### 🌱 테마 다운로드
원하는 테마를 찾았으면 해당 테마의 깃헙으로 이동한다. 그다음 **압축파일을 다운로드** 하고 압축을 풀어줘야한다.

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-2/download-zip.PNG)   

압축은 아무 곳에나 풀어도 상관없다.

> 💡 **Fork를 하지 않은 이유**   
깃헙 기능 중엔 **Fork**도 있는데 해당 레포지토리를 내 레포지토리로 찍어서 그대로 가져오는 기능이다. 훨씬 더 간편하긴한데 그렇게 하려면 아예 블로그 레포지토리가 없는 상태에서 시작해야한다. ~~포크 후 기존의 블로그에 적용해보려했으나 실패로 인하여 어쩔 수 없이 압축파일을 다운로드 했다.~~

압축을 푼 폴더를 연 후 전체 선택하고 내 블로그 레포지토리에 전부 복사하자.

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-2/copy.PNG)   

***

### 🌱 로컬에서 열기
이제 다음으로 블로그 레포지토리에서 **우클릭** 후 **Git Bash Here**을 클릭해 깃 배쉬를 열어주자. (**Start Command Prompt with Ruby**로 열어도 상관없지만 개인적으로는 깃 배쉬가 윈도우에선 제일 편한 것 같다.)

이제 명령어를 입력해줘야한다.   

지킬 테마를 적용하려면 우선 지킬 번들러를 설치해야한다. 루비의 명령어인 gem을 이용해 쉽게 설치할 수 있다. 블로그 레포지토리에서 **우클릭** 후 **Git Bash Here**을 클릭해 깃 배쉬를 열어서 해당 명령어를 입력해주면 설치된다. (**Start Command Prompt with Ruby**로 열어도 상관없지만 개인적으로는 깃 배쉬가 윈도우에선 제일 편한 것 같다.)

```
gem install jekyll bundler
bundle install
```

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-2/gem-install.PNG)   
![Alt Text](/assets/images/posts_img/projects/github-blog/gb-2/bundle-install.PNG)   

둘 다 설치가 성공적으로 완료되었으면 지킬 서버를 실행시키는 명령어를 입력해주자.

```
bundle exec jekyll serve
```

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-2/127.PNG)   

이렇게 서버 주소가 나오면서 서버가 돌아가면 성공적으로 연 것이다. 해당 주소를 입력하여 접속해보자.

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-2/jekyll-theme.PNG)   

성공적으로 로컬에서 접속한 것을 알 수 있다. 여기서 배포하고 싶다면 처음에 블로그 레포지토리에 올렸을 때처럼 push로 올리면 된다.

> 💡 **깃헙에 올리기**   
```
git add -A
git commit -m "커밋 메세지"
git push
```

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-2/git-push.PNG)   

푸시가 성공적으로 됐다면 이제는 로컬 주소가 아닌 _io 주소_ 로 접속해보자.

![Alt Text](/assets/images/posts_img/projects/github-blog/gb-2/blog.PNG)   

테마가 성공적으로 적용된 것을 확인할 수 있다.

> 💡 갓 배포한 블로그가 적용이 완료됐는지 알려면 **Actions** 탭을 확인하면 된다.   
![Alt Text](/assets/images/posts_img/projects/github-blog/gb-2/action.PNG)   
깃헙에서 제공해주는 서비스로 빌드부터 배포까지 모두 해주는데 시간이 조금 걸린다.   
![Alt Text](/assets/images/posts_img/projects/github-blog/gb-2/actions2.PNG)   

***

## 👻 글을 마치며
이번 시간에는 만들어둔 깃헙 블로그에 지킬 테마를 적용해보았다. 처음에 할 때는 8시간 걸렸던 것 같은데 복습 겸 포스팅 겸 다시 해보니 생각보다 에러없이 금세 적용한 것 같다. 로컬 저장소까지 과정에 끼어있어 이리갔다 저리갔다 헷갈리는 게 많았지만 점점 머릿속에서 정리가 되어가는 것 같아 기분이 좋다. 까먹지 않도록 꾸준히 복습하고 들여다봐야겠다 :)