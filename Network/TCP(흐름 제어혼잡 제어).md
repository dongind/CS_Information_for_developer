# TCP(흐름 제어/혼잡 제어)

>흐름 제어(Flow Control) : 송신 측과 수신 측의 데이터 처리 속도 차이를 해결하기 위한 기법
>
>혼잡 제어(Congestion Control) : 네트워크 혼잡을 피하기 위해 송신 측에서 보내는 데이터의 전송 속도를 강제로 줄이는 작업



# Flow Control / 흐름 제어

- 송신 측이 수신 측보다 속도가 빠를 경우 문제가 발생
  - 수신 측에서 제한된 저장 용량을 초과한 이후에 도착하는 데이터는 손실 될 수 있으며, 만약 손실 된다면 불필요하게 응답과 데이터 전송이 송/수신 측에 빈번히 발생



### 1. 해결 방식 : Stop and wait

> 매번 전송한 패킷에 대해 확인 응답을 받아야만 그 다음 패킷을 전송하는 방법

![img](https://t1.daumcdn.net/cfile/tistory/263B7D4E5715ECEB32)

### 2. 해결 방식 : Sliding Window

> 수신 측에서 설정한 윈도우 크기만큼 송신 측에서 확인 응답 없이 세그먼트를 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 제어 기법

**동작 방식 **: 먼저 윈도우에 포함되는 모든 패킷을 전송하고, 그 패킷들의 전달이 확인되는대로 이 윈도우를 옆으로 옮김(slide)으로써 그 다음 패킷들을 전송

![img](https://t1.daumcdn.net/cfile/tistory/253F7E485715ED5F27)

**송신 버퍼**

![img](https://t1.daumcdn.net/cfile/tistory/22532F485715EDF218)

- 200 이전의 바이트는 이미 전소오디었고 확인 응답을 받은 상태
- 200 ~ 202 바이트는 전송되었으나 확인 응답을 받지 못한 상태
- 203 ~ 211 바이트는 아직 전송이 되지 않은 상태



**수신 윈도우**

![img](https://t1.daumcdn.net/cfile/tistory/25403A485715EE362B)

- 수신 프로세스가 다음에 처리할 바이트는 194
- 수신 윈도우는 200의 수신을 대기



**송신 윈도우**

![img](https://t1.daumcdn.net/cfile/tistory/2520244B5715EE6A14)

- 수신 윈도우보다 작거나 같은 크기로 송신 윈도우를 지정하게 되면 흐름  제어가 가능
- 위와 같이 송신 윈도우가 7이라면, 200 ~ 202는 전송 되었으나 확인 응답을 받지 않았고, 203 ~ 206은 전송되지 않았지만 전송이 가능한 바이트



**송신 윈도우의 이동**

![img](https://t1.daumcdn.net/cfile/tistory/227DC8505715EEBA0A)

- before 상태에서 203 ~ 204를 전송하면 수신측에서는 확인 응답으로 203을 보내고, 송신측은 이를 받아 after상태와 같이 수신 윈도우를 203 ~ 209 범위로 이동



# Congestion Control / 혼잡 제어

- 송신 측의 대이터는 지역망이나 인터넷으로 연결된 대형 네트워크를 통해 전달된다. 만약 한 라우터에 데이터가 몰릴 경우, 자신에게 온 데이터를 모두 처리할 수 없게 된다. 이런 경우 호스트 들은 또 다시 재전송을 하게 되고 결국 혼잡만 가중시켜 오버 플로우나 데이터 손실을 발생시키게 된다. 이러한 네트워크의 혼잡을 피하기 위해 송신 측에서 보내는 데이터의 전송 속도를 강제로 줄이게 되는데, 이러한 작업을 혼잡 제어라고 한다.
- 네트워크 내에 패킷의 수가 과도하게 증가하는 현상을 혼잡이라 하며, 혼잡 현상을 방지하거나 제거하는 기능을 혼잡 제어라고 한다.
- 호스트와 라우터를 포함한 더 넓은 관점에서 전송 문제를 다룬다



###  해결방안

![img](https://camo.githubusercontent.com/e5daaf381dd565e77a2827a78e339a708cb239e302354ff75b446f39e0134286/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f323536453339343235373135463130313033)

- **AIMD(Additive Increase / Multicative Decrease)**
  - 최초 패킷을 하나씩 보내고, 이것이 문제없이 도착하면 window 크기(단위 시간 내에 보내는 패킷의 수)를 1씩 증가 시켜가며 전송하는 방법
  - 만일 패킷 전송을 실패하거나 일정 시간이 넘으면 패킷을 보내는 속도를 절반으로 줄인다
  - 공평한 방식으로, 이 방식을 사용하는 여러 호스트가 한 네트워크를 공유하고 있으면 나중에 진입하는 쪽이 처음에는 불리하지만 시간이 흐르면 평형 상태로 수렴하는 특징
  - 초기에 높은 네트워크의 높은 대역폭을 사용하지 못하여 오랜 시간이 걸리게 되고, 네트워크가 혼잡해지는 상황을 미리 감지 하지 못하는 문제점 존재
    - 즉, 네트워크가 혼잡해지고 나서야 대역폭을 줄이는 방식



- **Slow Start**
  - AIMD 방식과 마찬가지로 패킷을 하나씩 보내며, 패킷이 문제없이 도착할 경우 각각의 ACK 패킷마다 window size를 1씩 늘려준다
  - 즉, 한 주기가 지나면 window size가 2배로 증가
    - 따라서 전송 속도는 AIMD 방식과는 다르게 지수 함수 꼴로 증가
  - 혼잡 현상이 발생하면 Window Size를 1로 감소
  - 처음에는 네트워크의 수용량을 예상할 수 있는 정보가 없지만, 한번 혼잡 현상이 발생하고 나면 네트워크의 수용량을 어느 정도 예상할 수 있으므로 혼잡 현상이 발생하였던 Window size의 절반 까지는 이전 처럼 지수 함수 꼴로 창 크기를 증가 시키고 그 이후부터는 완만하게 1씩 증가



- **Fast Retrnasmit(빠른 재전송)**
  - 패킷을 받는 쪽에서 먼저 도착해야 할 패킷이 도착하지 않고 다음 패킷이 도착한 경우에도 ACK 패킷을 보내게 된다.
  - 단, 순서대로 잘 도착한 마지막 패킷의 다음 패킷의 순번을 ACK 패킷에 실어서 보냄
    - 중간에 패킷 하나가 손실이 발생하게 되면 보내는 측에서는 순번이 중복된 ACK 패킷을 받게 된다
    - 송신 측에서 중복된 ACK 패킷을 감지하는 순간 문제가 되는 순번의 패킷을 재전송
  - 빠른 재전송은 중복된 순번의 패킷을 3개 받으면 재전송을 하게 된다
  - 이러한 현상이 일어나는 것은 약간 혼잡한 상황이 일어난 것이므로 혼잡을 감지하고 Window Size를 줄이게 된다.



- **Fast Recovery(빠른 회복)**
  - 혼잡한상태가 되면 Window Size를 1로 줄이지 않고 반으로 줄이고 선형 증가 시키는 방법
  - 혼잡 상황을 한번 겪고 나서부터는 순수한 AIMD 방식으로 동작

![img](https://t1.daumcdn.net/cfile/tistory/256E39425715F10103)