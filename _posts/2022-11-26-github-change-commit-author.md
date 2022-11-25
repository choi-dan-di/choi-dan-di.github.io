---
title: "[Git] Push된 Commit Author 변경하기"
excerpt: "커밋하고나니 작성자가 잘못된 걸 알았지 모얌.. 그래서 쓰는 포스트"

categories:
  - GitHub
tags:
  - [GitHub, git, github, command, rebase, change, author]

permalink: /github/change-commit-author/

toc: true
toc_sticky: true

date: 2022-11-26 02:22:21+0900
last_modified_at: 2022-11-26 02:22:25+0900
---

## 👻 Commit Author 변경하기
전에 잠시 밖에 나가야 할 일이 있어서 1일 1커밋 못 할까봐 노트북으로 포스팅을 한 적이 있다. 그 때 설정도 안 하고 가서 에러 투성이에 고친다고 진땀 뺐었는데, 지킬 서버 돌아가는 거 확인하고 바로 덮어버린 것이 문제였다.

결국 시간에 쫓겨 11시 조금 넘어 포스트를 다 썼지만 웬 걸.. 커밋이 안 되더라는..^^...

그래서 최대한 빨리 고친다고 했지만 자정을 넘겨버리고.. 잔디에 빵꾸가 나버렸다.

시간을 변경해서 채울 수도 있지만 그렇게까진 하기 싫어서 그냥 남겨뒀다. ~~어차피 또 나중에 하루 못 할 일이 생길 건데 그 때마다 스트레스 받기 싫다!!~~

근데 그건 그렇고 결국 올리긴 했는데.. 그 땐 몰랐는데 다른 계정으로 커밋 푸시 됐더라고..? 4일전에 난 건데 이걸 지금 안 나도 참 웃기다 ㅎ..

무튼 이건 도저히 봐줄 수가 없어서 작성자를 변경해보기로 했다.

![Alt Text](/assets/images/posts_img/basics/github/change-commit-author/commit-history.PNG)   

위 이미지는 현재 내 커밋 히스토리 상황이다. 중간에 간첩이 하나 숨겨져 있는 것을 확인할 수 있다.

***

### 🌱 Git User 변경
혹시 모르니 유저를 확실히 한 번 더 변경해주었다.

```
git config --global user.name "username"
git config --global user.email "email@email.com"
```

![Alt Text](/assets/images/posts_img/basics/github/change-commit-author/set-user.PNG)   

***

### 🌱 Rebase
rebase와 변경할 커밋해쉬값을 이용해 명령어를 입력해주었다.

```
git rebase -i 해쉬값^
```

![Alt Text](/assets/images/posts_img/basics/github/change-commit-author/rebase.PNG)   

***

### 🌱 edit 변경
그러면 **vi 편집기**로 들어와지는데 ``` i ```를 눌러 **Insert** 모드로 바꿔준 다음 변경할 커밋해쉬값 앞의 ``` pich ```을 ``` edit ```나 ``` e ```로 변경해준다.

![Alt Text](/assets/images/posts_img/basics/github/change-commit-author/pick.PNG)   
![Alt Text](/assets/images/posts_img/basics/github/change-commit-author/e.PNG)   

**ESC**를 누른 후 ``` :wq ```를 입력한 다음 빠져나오면 된다.

***

### 🌱 Author 변경
다시 돌아오면 이제 변경할 유저명과 이메일을 입력해준다.

```
git commit --amend --author "username <email@email.com>"
```

![Alt Text](/assets/images/posts_img/basics/github/change-commit-author/amend.PNG)   

해당 명령어를 입력하면 다시 **vi 편집기**로 들어가는데 ``` :wq ```를 눌러 나와주면 된다.

***

### 🌱 rebase continue
변경할 커밋이 추가적으로 더 있다면 다음 명령을 이용해 다음 커밋으로 넘어갈 수 있다.

```
git rebase --continue
```

![Alt Text](/assets/images/posts_img/basics/github/change-commit-author/continue.PNG)   

그런다음 변경할 커밋 횟수만큼 반복하면된다. 나는 하나만 변경하면 됐어서 한 번만 하고 바로 빠져나왔다.

***

### 🌱 Push
마지막으로 ``` push ```를 통해 작성자 변경을 완료해주면 된다.

```
git push <remote> <branch>
```

![Alt Text](/assets/images/posts_img/basics/github/change-commit-author/push.PNG)   

branch 앞에 ``` + ```를 붙이는 이유는 rebase 작업 이후에는 **강제로 진행**하여야 하기 때문이라고 한다.   
``` <branch> -> <branch> ```가 나오면 성공적으로 된 것이다.

***

### 🌱 결과

![Alt Text](/assets/images/posts_img/basics/github/change-commit-author/result.PNG)   

성공적으로 바뀐 것을 확인할 수 있다! ~~근데 날짜는 왜..바뀐거지~~

아마 직접 수정은 하지 않아도 rebase 작업이 실행되면서   
뒤의 작업 모두 오늘 날짜로 바뀐 것 같다. 😂

***

## 👻 글을 마치며
Contributors에 나타나는 내 계정을 없애고 싶어서 찾아봤던 기능이다. 커밋 작성자 정보는 바꼈지만 정작 없애고 싶은 Contributors는 사라지지 않는데 왜.. 내일 자고 일어나면 없어져있겠지? 그러길 바라면서 오늘도 하나 배워간다.. 역시 깃은 어려워..😭

***

_출처_

_[push된 commit author 변경하기(커밋된 작성자 변경 방법)](https://eggwhite0.tistory.com/93)_   
