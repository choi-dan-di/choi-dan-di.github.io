---
title: "[Markdown] 마크다운(Markdown) 문법 정리"
excerpt: "마크다운(Markdown) 작성을 위한 문법 정리"

categories:
  - Markdown
tags:
  - [study, Markdown]

permalink: /md/grammar/

toc: true
toc_sticky: true

date: 2022-10-31 10:00:01+0900
last_modified_at: 2022-10-31 10:00:01+0900
---

## 👻 마크다운(Markdown) 이란?
마크다운은 일반 텍스트 기반의 경량 마크업 언어이다. 일반 텍스트로 서식이 있는 문서를 작성하는 데 사용되며, 일반 마크업 언어에 비해 문법이 쉽고 간단한 것이 특징이다.
   2004년에 존 그루버에 의해 만들어졌으며 특수기호와 문자를 이용한 매우 간단한 구조의 문법을 사용하기 때문에 보다 빠르게 컨텐츠를 작성하고 직관적으로 인식할 수 있다.
   Github 덕분에 최근 각광받기 시작했으며 설치방법, 소스코드 설명, 이슈 등을 간단하게 기록하고 가독성을 높일 수 있다는 강점이 부각되면서 점점 널리 퍼지게 되었다.
   마크다운 가이드는 이 곳 ["Markdown Guide"](https://www.markdownguide.org/) 에서 볼 수 있다.

***

## 👻 마크다운 사용법(문법)

### 🌱 헤더(Header)
- 큰 제목 : 문서 제목   

```
This is a H1   
============= (개수는 1개 이상만 되면 가능)
```

적용예 :   

&nbsp;This is an H1   
=============

***

- 작은 제목 : 문서 부제목

```
This is an H2   
------------- (개수는 1개 이상만 되면 가능)
```

적용예 :   

![Alt text](/assets/images/posts_img/basics/md/grammer/h2_example.PNG "정상적으로 출력되나 우측 TOC 때문에 이미지로 대체..^^")

***


- 글머리 : 1~6까지만 지원

```
# This is a H1
## This is a H2
### This is a H3
#### This is a H4
##### This is a H5
###### This is a H6
```

적용예 :   

![Alt text](/assets/images/posts_img/basics/md/grammer/header_example.PNG "정상적으로 출력되나 우측 TOC 때문에 이미지로 대체..^^")

***

### 🌱 블럭 인용(BlockQuote)
이메일에서 사용하는 <code>></code> 블럭인용문자를 이용한다.   

```
> This is a first blockquote.
>   > This is a second blockquote.
>   >   > This is a third blockquote.
```

적용예 :   

> This is a first blockquote.
>   > This is a second blockquote.
>   >   > This is a third blockquote.

이 안에서는 다른 마크다운 요소를 포함할 수 있다.

***

### 🌱 목록(List)
- 순서 있는 목록   
순서 있는 목록은 숫자와 점을 사용한다.

```
1. 첫 번째
2. 두 번째
3. 세 번째
```

적용예 :   
1. 첫 번째
2. 두 번째
3. 세 번째

현재까지는 어떤 번호를 입력해도 순서는 <u>내림차순</u>으로 정의된다.   

***

- 순서 없는 목록(글머리 기호 : *, +, - 지원)   

```
* 빨강
    * 녹색
        * 파랑

+ 빨강
    + 녹색
        + 파랑

- 빨강
    - 녹색
        - 파랑
```

적용예 :

* 빨강
    * 녹색
        * 파랑

+ 빨강
    + 녹색
        + 파랑

- 빨강
    - 녹색
        - 파랑

혼합 사용도 가능

***

### 🌱 코드 블럭(Code Block)
- 들여쓰기   
4개의 공백 또는 하나의 탭으로 들여쓰기를 만나면 변환되기 시작하여 들여쓰지 않은 행을 만날 때까지 변환이 계속된다.

```
This is a normal paragraph:

    This is a code block.

end code block.
```

적용예 :   

This is a normal paragraph:

    This is a code block.

end code block.

여기서 한 줄을 더 띄어쓰지 않으면 인식이 제대로 되지 않는 문제가 발생한다.   

```
This is a normal paragraph:
    This is a code block.
end code block.
```

적용예 : 

This is a normal paragraph:
    This is a code block.
end code block.

***

- ```<pre><code>{code}</code></pre>``` 이용

```
<pre>
<code>
public class BootSpringBootApplication {
  public static void main(String[] args) {
    System.out.println("Hello World!");
  }
}
</code>
</pre>
```

적용예 : 

<pre>
<code>
public class BootSpringBootApplication {
  public static void main(String[] args) {
    System.out.println("Hello World!");
  }
}
</code>
</pre>

***

- 코드블럭코드(```)를 이용

````
```
public class BootSpringBootApplication {
  public static void main(String[] args) {
    System.out.println("Hello World!");
  }
}
```
````

적용예 : 

```
public class BootSpringBootApplication {
  public static void main(String[] args) {
    System.out.println("Hello World!");
  }
}
```

Github에서는 코드블럭코드(```) 시작점에 사용하는 언어를 선언하면 문법강조(Syntax highlighting)가 가능하다.

````
```java
public class BootSpringBootApplication {
  public static void main(String[] args) {
    System.out.println("Hello World!");
  }
}
```
````

적용예 : 

```java
public class BootSpringBootApplication {
  public static void main(String[] args) {
    System.out.println("Hello World!");
  }
}
```

***

### 🌱 수평선(horizontal line)
아래 줄은 모두 수평선을 만든다. 마크다운 문서를 미리보기로 출력할 때 페이지 나누기 용도로 많이 사용한다.

```
* * *
***
*****
- - -
-----------------------------------
```

적용예 : 

* * *
***
*****
- - -
-----------------------------------

***

### 🌱 링크(Hyperlink)
- 참조 링크   
같은 링크 URL을 여러번 입력해야하거나 글 안의 링크를 따로 관리하고 싶을 때 편리한 기능

```
[link keyword][id]   
[id]: URL "Optional Title here"

// 적용예로 쓸 코드
Link: [Google][googlelink]

[googlelink]: https://google.com "Go google"
```

적용예 : 

Link: [Google][googlelink]

[googlelink]: https://google.com "Go google"

***

- 외부 링크

```
사용 문법 : [Title](link 설명)
적용예로 쓸 코드 : [Google](https://google.com)
```

적용예 : 

[Google](https://google.com)

***

- 자동 연결   
일반적인 URL 혹은 이메일주소인 경우 적절한 형식으로 링크를 형성한다.

```
* 외부 링크: <http://example.com/>
* 이메일 링크: <address@example.com>
```

적용예 : 

* 외부 링크: <http://example.com/>
* 이메일 링크: <address@example.com>

***

### 🌱 강조(emphasis)

```
*single asterisks*
_single underscores_
**double asterisks**
__double underscores__
~~cancelline~~
```

적용예 : 

*single asterisks*   
_single underscores_   
**double asterisks**   
__double underscores__   
~~cancelline~~

```
문장 중간에 사용할 경우에는 **띄어쓰기** 를 사용하는 것이 좋다.
```

적용예 : 

문장 중간에 사용할 경우에는 **띄어쓰기** 를 사용하는 것이 좋다.

***

### 🌱 이미지(Image)

```
![Alt text](/path/to/img.jpg)
![Alt text](/path/to/img.jpg "Optional title")  // 마우스 오버 시 나오는 문구
```

적용예 : 

![Alt text](/assets/images/posts_img/basics/md/grammer/짱구사진.jpg)
![Alt text](/assets/images/posts_img/basics/md/grammer/짱구사진.jpg "마우스 올리면 짱구사진이라는 글이 나옵니다.")

사이즈 조절 기능은 없기 때문에 ```<img width="" height="">``` 를 이용한다.

적용예로 쓸 코드
```
<img src="/assets/images/posts_img/basics/md/grammer/짱구사진.jpg" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="짱구"><br/>
<img src="/assets/images/posts_img/basics/md/grammer/짱구사진.jpg" width="40%" height="30%" title="%(비율) 크기 설정" alt="짱구">
```

<img src="/assets/images/posts_img/basics/md/grammer/짱구사진.jpg" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="짱구"><br/>
<img src="/assets/images/posts_img/basics/md/grammer/짱구사진.jpg" width="40%" height="30%" title="%(비율) 크기 설정" alt="짱구">

***

### 🌱 줄바꿈(new line)
3칸 이상 띄어쓰기(```   ```)를 하면 줄이 바뀐다.

```
* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸 이상을 띄어쓰기 해야한다.
이렇게

* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸 이상을 띄어쓰기 해야한다.___// 띄어쓰기
이렇게
```

적용예 : 

* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸 이상을 띄어쓰기 해야한다.
이렇게

* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸 이상을 띄어쓰기 해야한다.   
이렇게

***

### 🌱 표(table)
<span style="font-size: 0.7em; color: gray;">2022.11.03 추가</span>

- 일반적인 표

```
|제목|내용|설명|
|------|---|---|
|테스트1|테스트2|테스트3|
|테스트1|테스트2|테스트3|
|테스트1|테스트2|테스트3|
```

적용예 : 

|제목|내용|설명|
|------|---|---|
|테스트1|테스트2|테스트3|
|테스트1|테스트2|테스트3|
|테스트1|테스트2|테스트3|

***

- 정렬
```:```로 정렬을 할 수 있다.   

```
|제목|내용|설명|
|:---|---:|:---:|
|왼쪽정렬|오른쪽정렬|중앙정렬|
|왼쪽정렬|오른쪽정렬|중앙정렬|
|왼쪽정렬|오른쪽정렬|중앙정렬|
```

적용예 : 

|제목|내용|설명|
|:---|---:|:---:|
|왼쪽정렬|오른쪽정렬|중앙정렬|
|왼쪽정렬|오른쪽정렬|중앙정렬|
|왼쪽정렬|오른쪽정렬|중앙정렬|

***

- 셀 확장

```
|제목|내용|설명|
|:---|:---:|---:|
||중앙에서확장||
|||오른쪽에서 확장|
|왼쪽에서확장||
```

적용예 : 

|제목|내용|설명|
|:---|:---:|---:|
||중앙에서확장||
|||오른쪽에서 확장|
|왼쪽에서확장||

***

- 셀 강조

```
|제목|내용|설명|
|---|---|---|
|테스트1|*강조1*|테스트3|
|테스트1|**강조2**|테스트3|
|테스트1|<span style="color:red">강조3</span>|테스트3|
```

적용예 : 

|제목|내용|설명|
|---|---|---|
|테스트1|*강조1*|테스트3|
|테스트1|**강조2**|테스트3|
|테스트1|<span style="color:red">강조3</span>|테스트3|

***

## 👻 글을 마치며
처음엔 익숙하지 않아 글을 써내려가면서 헷갈리기도 하고 마음처럼 한 번에 잘 되지 않았었는데,   
마크다운 문법 정리 포스팅을 하다보니 자연스레 문법을 익히게 된 것 같다.   
앞으로 생각날 때마다 들여다보면서 꾸준히 포스팅을 하게 된다면 이 글 필요없이도 쑥쑥 써내려 갈 수 있지 않을까 싶다!   
신기하고 재미있고 새로운 시간이었음!

***

_출처_   
_[ihoneymon/how-to-write-by-markdown.md](https://gist.github.com/ihoneymon/652be052a0727ad59601)_   
_[참조 링크만 살짜쿵 👀](https://lynmp.com/ko/article/title/markdown-link-ua811c9dc59o)_