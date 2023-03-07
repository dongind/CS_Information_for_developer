# CPU Scheduling

> 프로세스가 생성되어 실행될 때, 필요한 시스템의 여러 자원을 해당 프로세스에게 할당하는 작업

### 1. CPU Scheduling 목적

- 공정한 스케줄링
- 처리량의 극대화
- 응답시간의 최소화
- 균형있는 자원 사용
- 실행의 무한 연기 배제
- 반환시간 예측 가능



### 2. CPU Scheduling의 기준

- CPU 이용률(Utilization) : CPU를 이용한 수치의 백분율
- Throughput : 단위 시간 당 완료된 프로세스의 수
- Response Time : 작업 요청 후 응답이 오는 데 걸리는 시간
- Waiting Time : 대기 큐에서의 대기 시간



### 3. CPU Scheduling의 종류

**선점(Preemptive)**

- 개념 : CPU가 어느 프로세스를 실행하는 중, 다른 프로세스가 새롭게 CPU를 점유할 수 있는 스케쥴링 
- 장점 : 빠른 응답, 시분할 시스템에 적합
- 단점 : 오버헤드 발생(Context Switching)

**비선점(Non-Preemptive)**

- 개념 : 프로세스가 끝나거나 IO를 만나기 전까지 새로운 프로세스가 CPU를 사용할 수 없음
- 장점 : 응답시간 예상 가능, 공정한 처리
- 단점 : 짧은 작업도 긴 대기 시간 발생



#### 참고) CPU Scheduling 알고리즘 종류

- 선점 알고리즘
  - Round Robin
    - 같은 크기의 CPU를 할당하는 순환 할당 방식
    - FCFS에 의해 프로세스가 CPU를 할당 받으면 Time Slice에 의해 같은 시간의 CPU를 할당
    - 시분할 방식에 효과적이며 할당 시간(Time Slice)의 크기가 중요함
    - 할당 시간이 크면 FCFS에 가까워짐
  - SRT
    - Short Remaining Time
    - 대기 큐의 가장 짧은 소요시간의 Process 선택
    - 실행 중 선점 가능
  - Multi Level Queue
    - 작업을 그룹화하여 여러 큐 관리 및 할당
  - Multi Level Feedback Queue
    - 여러 개의 큐를 두고 큐마다 다른 시간 할당량(Time Slice) 부여
    - 짧은 작업 유리, 입출력 작업에 우선권 부여
    - 하위 단계일수록 할당 시간이 커짐
- 비선점 알고리즘
  - FCFS
    - First Come First Service
    - 도착한 순서대로 처리(일괄 처리)
    - Batch 작업에 주로 사용, 짧은 작업이 긴 작업을 기다리게 됨
  - SJF
    - Shortest Job First
    - 준비 큐에서 가장 짧은 소유시간의 프로세스 실행
    - FCFS보다 평균 대기 시간 감소
    - 긴 프로세스가 짧은 프로세스에게 계속 작업 순위가 밀리게 되어 실행되지 못하는 기아 현상(Starvation)이 발생
  - Priority
    - 프로세스에 우선 순위 부여하여 우선 순위에 따라 할당
    - 정적 할당 : 실행 중 우선 순위를 바꾸지 않음
    - 동적 할당 : 상황 변화에 적응하여 우선 순위의 변경이 가능
  - Deadline 
    - 작업을 명시된 시간이나 기한 내 완료하는 방식
  - HRN
    - Highest Response Next
    - 긴 작업과 짧은 작업의 공정성을 고려한 방식
    - Response Ratio가 높은 것을 선택(Response Ratio = (대기 시간 + 처리 시간) / 처리 시간)
    - SJF의 단점을 보완하여 Starvation 현상 방지
    - 짧은 작업이나 대기 시간이 긴 작업은 우선 순위가 높아짐

### 4. 프로세스 상태 전이도

![image-20230307110337057](C:\Users\dongi\OneDrive\문서\SSAFY\dongind_oct\CS스터디\CS_Information_for_developer\OS\assets\image-20230307110337057.png)

- 생성 -> 준비
  - Admit
  - 준비 큐가 비어있을 때
  - 작업 스케줄러가 담당
- 준비 -> 실행
  - Dispatch
  - 준비 큐에 있는 하나의 프로세스를 선택하여 CPU를 할당
  - 프로세스 스케줄러가 담당
- 실행 -> 준비
  - Timer Run Out
  - CPU를 할당 받은 프로세스가 CPU의 제한된 사용 시간을 모두 쓴 경우 발생
  - CPU 스케줄링 정책에 따라 우선 순위가 높은 프로세스에게 CPU를 양보할 때, 운영체제 자체의 CPU 서비스 요청 시전(선점)

- 실행 -> 슬립(대기)
  - Blocked
  - CPU를 할당 받은 프로세스가 I/O 요구, 다른 자원 요구 등 CPU 이외의 서비스 작업을 원할 때
- 슬립(대기) -> 준비
  - Wake Up
  - 대기 중이던 사건(조건)의 처리가 끝났을 때, (ex. I/O 작업 완료)
- 실행 -> 종료
  - Release
  - 프로세스의 정상/비정상 종료 시





#### 참고) CPU Scheduling 실사례

- Window : 우선 순위에 기반한 선점 스케줄링
- Linux : 시분할(공정한 선점 스케줄링), 실시간(우선 순위 근간의 Round Robin)
- Solaris : 실시간, 시스템, 시분할 등의 우선 순위로 나눈 다단계 큐 알고리즘

#### 참고) CPU Scheduling 알고리즘 평가 방안

- 결정론적 모델링 : 사전 정의된 특정 작업 부하에 대한 성능 평가
- 큐잉 모델 : 수학적 모델을 이용하여 근사치 계산
- 모의 실험 : 난수 발생기를 이용하여 시뮬레이션



### 참조

https://m.blog.naver.com/scw0531/221417295183