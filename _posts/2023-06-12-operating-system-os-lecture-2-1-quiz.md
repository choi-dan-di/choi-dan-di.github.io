---
title: "[Operating System] #2주차 - 퀴즈 풀이"
excerpt: "운영체제 공룡책 Thread & Concurrency 퀴즈 풀이 정리"

categories:
  - Operating System
tags:
  - [Operating System, os, thread, concurrency, quiz]

permalink: /operating-system/os-lecture-2-1-quiz/

toc: true
toc_sticky: true

date: 2023-06-12 20:38:22+0900
last_modified_at: 2023-06-12 20:38:26+0900
published: true
---

## 👻 퀴즈 풀이

***

### 🌱 1
- **문제**

> From exercise 4.2:   
Using Amdahl's Law, calculate the speedup gain of an application that has a 60 percent parallel component for (a) two processing cores and (b) four processing cores.   
위 연습문제의 정답으로 가장 옳은 것은?   
>
> 1) (a) 1.43 (b) 1.81   
2) (a) 1.81 (b) 1.43   
3) (a) 2.56 (b) 2.13   
4) (a) 2.13 (b) 2.56

- **풀이**

정답은 **①**.

문제에서 나오는 60%는 병렬적인 부분을 의미한다. 암달의 법칙에서 S는 시스템의 **연속적인 부분**이므로 100%에서 60%를 제외한 40%가 되며 S는 0.4가 된다. N이 각각 2와 4일 때 계산하면 **1.43**(반올림)과 **1.81**이 나온다.

***

### 🌱 2
- **문제**

> user-thread와 kernel-thread에 대한 설명으로 가장 틀린 것은?   
>
> 1) user thread는 사용자 모드에서 동작하고, kernel thread는 커널 모드에서 동작한다.   
2) Many-to-One 스레드 모델에서는 다수의 kernel thread가 1개의 user thread를 지원한다.   
3) user thread와 kernel thread는 반드시 생성한 process에 결합되어 있어야만 한다.   
4) One-to-One 스레드 모델은 concurrency의 측면에서 Many-to-One 모델보다 우수하다.

- **풀이**

정답은 **②**.

스레드 모델은 **User thread**-to-**Kernel thread**를 의미한다. 즉 Many-to-One은 하나의 kernel thread가 다수의 user thread를 관리하는 모델을 의미한다.

***

### 🌱 3
- **문제**

> From Exercise 4.10:   
Which of the following components of program state are shared across threads in a multithreaded process?   
위 연습문제의 정답에 해당하는 것을 모두 고르시오.   
>
> 1) register values   
2) heap memory   
3) global variables   
4) stack memory

- **풀이**

정답은 **②, ③**.

멀티스레드 프로세스에서 스레드 간 어떠한 정보를 공유하냐는 문제이다. Code, Data, File 등이 있으며 1번의 레지스터 값과 4번의 스택은 각 스레드 별로 생성된다.

***

### 🌱 4
- **문제**

> 멀티스레드 프로그래밍 모델의 장점에 대한 설명으로 가장 틀린 것은?   
1) 반응성이 좋다 : 프로세스가 유저 인터페이스를 처리하느라 blocked 되어 있을 때도 실행을 계속할 수 있다.   
2) 자원 공유에 유리하다 : 다른 프로세스와 shared memory를 사용할 수 있으므로 자원 공유에 유리하다.   
3) 경제성이 좋다 : 프로세스간 context switch보다 스레드간 context switch가 훨씬 가볍다.   
4) 확장성이 좋다 : CPU가 여러 개이거나 core가 여러 개인 경우에 이를 잘 활용할 수 있다.

- **풀이**

정답은 **②**.

자원 공유는 하나의 프로세스 내에서 스레드간의 공유이기 때문에 프로세스간 전달 방법인 shared memory나 message passing 방법 보다 유리하다.

***

### 🌱 5
- **문제**

> Java에서의 멀티스레드 프로그래밍에 대한 설명으로 가장 틀린 것은?   
1) Thread 클래스를 상속하여 public void run() 메소드를 오버라이딩한다.   
2) Runnable 인터페이스를 상속하여 public void run()을 오버라이딩한다.   
3) Thread 클래스의 인스턴스를 생성하여 해당 인스턴스의 run() 메소드를 호출한다.   
4) Thread 클래스 생성자의 매개변수로 Runnable 인터페이스를 상속한 객체 인스턴스를 줄 수 있다.

- **풀이**

정답은 **③**.

Java에서의 멀티스레드 프로그래밍은 Thread 클래스를 상속하거나 Runnable 인터페이스를 구현하여 사용할 수 있다.

***

### 🌱 6
- **문제**

> OpenMP에 대한 설명으로 가장 틀린 것은?   
1) 컴파일러 지시어(directive)를 이용하여 병렬 처리를 할 수 있다.   
2) 병렬 처리 가능한 코드 영역을 #pragma omp parallel로 지정할 수 있다.   
3) omp_set_num_threads() 함수로 병렬 처리할 스레드 개수를 설정할 수 있다.   
4) OpenMP는 스레드를 미리 생성하여 pool에 저장해 놓기 때문에 thread 생성 시간을 절약한다.

- **풀이**

정답은 **④**.

OpenMP는 병렬 처리할 블록을 미리 지정하는 방식이므로 스레드 풀과는 관련없다. 스레드 풀은 implicit threading 방식을 구현하는 방법 중 하나이다.

***

### 🌱 7
- **문제**

> 다음 Pthread 예제의 출력으로 가장 옳은 것은?   
>
> ```c
> int x = 10;
> 
> void *runner(void *param) {
>     x += 10;
>     pthread_exit(0);
> }
> 
> int main() {
>     int i;
>     pid_t pid1, pid2;
>     pthread_t tid;
>     pid1 = fork();
>     if (pid1 == 0) {
>         pthread_create(&tid, NULL, runner, NULL);
>         pthread_join(tid, NULL);
>         pid2 = fork();
>         if (pid2 > 0) {
>             wait(NULL);
>             x += 10;
>         }
>         printf("%d ", x);
>     }
>     else {
>         wait(NULL);
>         printf("%d ", x);
>     }
> }
> ```
>
> 1) 10 20 30   
2) 20 30 10   
3) 30 20 10   
4) 10 30 20

- **풀이**

정답은 **②**.

1. 부모 프로세스가 fork()로 자식 프로세스 생성(pid1 = 자식 pid)
2. else문으로 들어가 wait(NULL) 만나며 대기 상태로 변환
3. 자식 프로세스는 if문으로 들어가 스레드 생성(자식 프로세스의 x = 20)
4. 자식 프로세스가 fork()로 손자 프로세스 생성(pid2 = 손자 pid)
5. if문으로 들어가 wait(NULL) 만나며 대기 상태로 변환
6. 손자 프로세스의 printf 실행(손자는 자식 프로세스의 x를 그대로 복사 x = 20) 👉 **첫 번째 출력**
7. 자식 프로세스의 x += 10 및 printf 실행(x = 30) 👉 **두 번째 출력**
8. 부모 프로세스의 printf 실행(x = 10) 👉 **세 번째 출력**

***

### 🌱 8
- **문제**

> 다음 Java 프로그램 예제의 출력으로 가장 옳은 것은?   
>
> ```java
> class Runner implements Runnable {
>     public void run() {
>         for (int i = 0; i < 3; i++)
>             System.out.printf("A ");
>     }
> }
> 
> public class ThreadQuiz {
>     public static final void main(String[] args) throws Exception {
>         Thread thread = new Thread(new Runner());
>         System.out.printf("B ");
>         thread.start();
>         System.out.printf("C ");
>         thread.join();
>         System.out.printf("D ");
>     }
> }
> ```
>
> 1) B C A A A D   
2) B C D A A A   
3) B A A A C D   
4) B C A D A A

- **풀이**

정답은 **①**.

1. 스레드 생성
2. System.out.printf("B "); 실행 👉 **첫 번째 출력**
3. Runner의 run() 호출
4. System.out.printf("C "); 실행 👉 **두 번째 출력**
5. Runner의 run() 실행 👉 **세 번째 출력으로 A가 세 번 출력됨**
6. 스레드 대기 상태로 변경
7. System.out.printf("D "); 실행 👉 **마지막 출력**

``` thread.start() ```는 c의 ``` fork() ```와 같다. 따라서 호출 즉시 context switching이 이루어지는 게 아닌 **실행 대기 상태**에 있다가 context switching되어 스레드가 작업을 수행하게된다.

***

## 👻 글을 마치며
이번 시간에는 퀴즈를 풀어보았다. 처음에 풀었을 때 8문제 중에 4문제를 틀렸었는데... 해설도 없어서 약간 막막했던 것 같다. 그래도 오답 노트를 정리하고 다시 문제를 천천히 살펴보니 내가 어떤 부분을 몰랐는지 알 수 있었고, 그 부분에 대해 깊게 공부하게 된 계기가 된 것 같다.

***

_출처_   
_[인프런 주니온님 강의 : 운영체제 공룡책 강의](https://inf.run/Fbcj)_   