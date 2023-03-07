# PCB & Context Switching

### 프로세스 관리

> Process Management : 구동중인 프로세스가 여러 개일 때, CPU 스케줄링을 통해 프로세스를 관리하는 것

- 스케줄링을 위해 CPU 들은 각 프로세스들에 대해서 구분할 수 있도록 **Process Metadata**를 활용
  - Process Metadata : 각기 다른 프로세스들의 본연의 특징
    - 프로세스 고유 ID(PID)
    - 프로세스 상태
    - 프로세스 우선 순위
    - Program Counter(PC)
    - CPU 레지스터
    - Owner
    - Memory Limit
    -  기타 등등
  - Metadata는 프로세스가 생성될 때마다 **PCB(Process Control Block)**에 저장



## PCB(Process Control Block)

> Process Metadata를 저장하는 곳

- 하나의 PCB 안에는 하나의 프로세스의 정보가 담기게 된다.
- 프로그램이 실행되어 메모리에 적재되었을 때, 프로세스가 생겨나고 프로세스 주소 공간에 코드 & 데이터 & 스택 공간이 생성
- 이후 해당 프로세스의 메타데이터들이 PCB에 저장
- 프로세스 A에서 B로 교체될 때의 과정
  1. B에서 인터럽트 발생
  2. A의 현재 실행 정보를 PCB에 저장
  3. A를 대기 상태로 돌리고 B를 실행 상태로 전환
  4. B의 PCB 정보를 기반으로 실행 재개
  5. B가 원하는 동작을 모두 수행
  6. B의 현재 실행 정보를 PCB에 저장
  7. B를 대기 상태로 돌리고 A를 실행 상태로 전환
  8. A의 PCB 정보를 기반으로 실행 재개

![img](C:\Users\dongi\OneDrive\문서\SSAFY\dongind_oct\CS스터디\CS_Information_for_developer\OS\assets\images%2Fhaero_kim%2Fpost%2Fdd3d9ba6-bb62-4ade-bce7-341a2e1a3751%2F25673A5058F211C224.png)

### PCB의 관리 방식

- 프로세스가 생성될 때 마다 PCB가 하나씩 증가
- PCB를 관리하는 자료 구조 : Linked List
  - PCB List Head에 PCB들이 생성될 때마다 하나씩 이어 붙임
  - 주소값으로 연결되는 형태 -> 새로운 PCB의 생성 혹은 기존 PCB 삭제에 용이



## Context Switching

> 실행중이던 프로세스의 상태를 PCB에 보관하고, 새로 들어오는 프로세스의 PCB 정보를 바탕으로 레지스터에 값을 적재하는 과정

- 다른 표현으론, 다중 프로그래밍 시스템에서 CPU가 할당되는 프로세스를 변경하기 위해 현재 CPU를 사용하여 실행되고 있는 프로세서의 상태 정보를 저장하고 제어권을 인터럽트 서비스 루틴(ISR)에게 넘기는 작업
- Context : CPU가 다루는 Task(Process / Thread)에 대한 정보, 대부분의 정보는 Register에 저장되고 PCB로 관리
- Context Switching 발생 상황 : 프로세스가 Ready -> Running, Running -> Ready, Running -> Block 과 같은 상태 변경 새



## Context Switching Cost

1. Cache 초기화

2. Memory Mapping 초기화

3. 메모리의 접근을 위한 Kernel의 상시 실행 상태

   => 잦은 Context Switching은 성능 저하를 유발



## Context Switching과 시간 할당량

> 프로세스들의 시간 할당량은 시스템 성능의 중요한 역할 수행

- 시간 할당량이 적어지면 : 문맥 교환 수 ,인터럽트 횟수, 오버헤드가 증가하지만 여러 개의 프로세스가 동시에 수행 되는 느낌 
- 시간 할당량이 커지면 : 문맥 교환 수, 인터럽트 횟수, 오버헤드가 감소하지만 여러 개의 프로세스가 동시에 수행되는 느낌이 없다
  - 오버헤드 : 간접 부담 비용으로, 프로세스의 실행을 위한 부가적인 활동
- 프로세스를 수행하다가 I/O event가 발생하여 BLOCK 상태로 전환시켰을 때, CPU가 그냥 놀게 놔두는 것보다 다른 프로세스를 수행시키는 것이 효율적
  - 따라서 CPU에 계속 프로세스를 수행시키도록 하기 위해 다른 프로세스를 실행시키고 Context Switching을 할 때 Overhead가 발생

