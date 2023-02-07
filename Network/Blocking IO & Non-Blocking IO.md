# Blocking I/O & Non-Blocking I/O

> I/O 작업 간, 사용자의 프로세스 유휴 방식에 따른 구분



## 0. I/O 작업

> I/O : Input/Out의 약자로, 한 커뮤터에서 출력(send)을 하고, 다른 컴퓨터에서 입력(read)을 받는 통신 작업

- I/O 작업은 User(유저 레벨)에서 직접 수행할 수 없고, 실제 IO 작업을 수행하는 것은 Kernel(커널 레벨)에서만 가능하다.

  - 유저 프로세스(스레드)는 커널에게 요청을 하고 작업 완료 후 커널이 반호나하는 결과를 대기

  

## 1. Blocking Model

> 가장 기본적인 I/O 모델로, **I/O 작업이 진행하는 동안 유저 프로세서는 자신의 작업을 중단한 채 대기**

![img](https://user-images.githubusercontent.com/41428527/51266321-4ade9700-19fe-11e9-9b23-30bca4faccfd.png)

**작업 순서**

1. 유저가 커널에 read 작업을 요청 (유저 -> 커널)
2. 유저의 프로세스(스레드)는 데이터가 입력될 때까지 작업을 중단한 후 대기
3. 데이터가 입력되어 유저에게 결과가 전달되면 유저의 프로세스가 작업을 재개

**특징**

- 어플리케이션에서 다른 작업이 수행하지 못하고 대기하게 되므로 Resource 낭비가 심하다

- Blocking I/O 방식으로 여러 Client가 접속하는 서버를 구현시

  - I/O 작업을 진행하는 작업을 중지
  - 만일 접속자 수가 많아지면, 다른 client로 인한 작업 중지를 막기 위해 client별 스레드 생성

  => 스레드 증가로 인한 context switching 횟수가 증가하는 비효율성 증가

  

> **context swithcing**
>
> 하나의 프로세스가 CPU를 사용 중인 상태에서 다른 프로세스가 CPU를 사용하도록 하기 위해, 이전의 프로세스 상태(context)를 보관하고 새로운 프로세스의 상태를 적재하는 작업
>
> 한 프로세스의 문매은 그 프로세서의 프로세스 제어 블록에 기록





## 2. Non-Blocking Model

> **I/O 작업이 진행되는 동안 유저 프로세스의 작업을 유지하는 방식**, Blocking I/O의 비효율성을 극복하는 방식

![img](https://user-images.githubusercontent.com/41428527/51266324-4e721e00-19fe-11e9-900a-809ff39e40c1.png)

**작업 순서**

1. 유저가 커널에 read 작업을 요청(유저의 Process가 recvform 함수 호출)
2. 데이터 유무와 상관없이, 바로 결과를 반환
   - 유저에게 보내야하는 데이터가 recvBuffer에 있을 경우 해당 데이터를 반환
     - 커널은 recvBuffer로부터 데이터를 복사하여, 복사한 데이터의 길이와 함께 반환
   - 데이터가 없을 경우 입력 데이터가 없다는 결과(**EWOULDBLOCK**)를 반환

3. 입력 데이터가 있을 때까지 1, 2 과정을 반복
   - 입력 요청 과정을 반복하는 동안 유저 프로세서는 결과(데이터 유뮤 상관없이)를 받은 후, 자신의 작업을 진행한다.

**특징**

- 입력과 관계없이 유저의 작업을 진행할 수 있다.

- 계속해서 동일한 내용의 요청, 즉 반복적인 시스템 호출로 인한 자원 낭비가 발생



## 3. I/O 이벤트 통지 방식

1. **Sync Blocking I/O : 함수가 바로 리턴하지 않고, 호출하는 함수는 작업 완료 여부를 신경쓰는 경우**

   - I/O가 실행되는 동안 어플리케이션이 다른 일을 하지 못하고 있다가, I/O가 끝나고 나서 이어 작업을 처리하는 경우
   - system call 마다 Thread를 생성하기 때문에 I/O 요청이 많은 서비스에서는 작업 한번의 context switching이 발생하므로 성능이 떨어진다.
   - Cpu resource 사용 관점에서 비효율적

   

2. **Sync Non-Blocking I/O : 함수가 바로 리턴하고, 호출하는 함수는 작업 완료 여부를 신경 쓰는경우**

   - I/O는 I/O을 요청하고 즉시 리턴받아 CPU 제어권을 받는다. 이로써 지속적으로 I/O 작업이 완료될 때까지 system call을 보내고, I/O가 완료되면 해당 작업을 처리하는 경우
   - 커널로부터 결과를 받기까지 계속 상태를 체크하는 busy-wait 상태가 된다. 즉, 작업 order를 맞추기 위해 I/O 작업의 완료를 기다리게 된다.
   - system call 주기도 적절하게 설정하지 않으면 커널에 의미없는 요청이 계속해서 전달되므로 I/O 작업에 지연을 불러올 수 있다.

   

3. **Async Blocking I/O : 함수가 바로 리턴하지 않고, 호출하는 함수는 작업 완료 여부를신경쓰지 않는 경우**

   - I/O 작업을 호출 할 때 callback을 같이 넘겨주면서 I/O 작업이 종료되었을 때, call back함수가 호출되는 방식이지만 실질적으로 I/O 로직이 처리될 때까지 어플리케이션이 block 되는 경우

   

4. **Async Non-Blocking I/O : 호출되는 함수는 바로 리턴하고, 호출하는 함수는 작업 완료 여부를 신경쓰는 경우**
   - 어플리케이션은 system call 이후 I/O 처리에 신경 쓰고 있지 않다가 작업이 완료되면 커널로부터 signal, thread 기반 callback 등으로 결과를 마치 event 처럼 전달받는 경우
   - 성능과 자원 효율적 사용 관점에서 가장 유리한 모델



## reference

블로킹(Blocking), 논블로킹(Non-Blocking) - I/O 모델(1) :

https://ju3un.github.io/network-basic-1/

Blocking/Non-Blocking I/O && I/O 이벤트 통지 모델 :

https://velog.io/@octo__/BlockingNon-Blocking-IO-IO-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%86%B5%EC%A7%80-%EB%AA%A8%EB%8D%B8