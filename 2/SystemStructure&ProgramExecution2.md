# System Structure & Program Execution 2

### 🤖 동기식 입출력과 비동기식 입출력
- 동기식 입출력 (synchronous I/O)
  - I/O 요청 후 입출력 작업이 완료된 후에야 제어가 사용자 프로그램에 넘어감
  - 시간적으로 서로 맞추는 것을 synchronous라고 함 (립싱크)
  - 구현 방법 1
    - I/O가 끝날 때까지 CPU를 낭비시킴
    - 매시점 하나의 I/O만 일어날 수 있음
  - 구현 방법 2
    - I/O가 완료될 때까지 해당 프로그램에게서 CPU를 빼앗음
    - I/O 처리를 기다리는 줄에 그 프로그램을 줄 세움
    - 다른 프로그램에게 CPU를 줌
- 비동기식 입출력 (asynchronous I/O)
  - I/O가 시작된 후 입출력 작업이 끝나기를 기다리지 않고 제어가 사용자 프로그램에 즉시 넘어감

두 경우 모두 I/O의 완료는 인터럽트로 알려줌

![](https://velog.velcdn.com/images/jimeaning/post/7a28e5e0-0d2e-4782-82ec-3ee4dd6981f2/image.png)

<br>

### 🤖 DMA (Direct Memory Access) controller
CPU 뿐만 아니라 DMA도 메모리에 접근할 수 있음
- 빠른 입출력 장치를 메모리에 가까운 속도로 처리하기 위해 사용
- CPU의 중재 없이 device controller가 device의 buffer storage의 내용을 메모리에 block 단위로 직접 전송
- 바이트 단위가 아니라 block 단위로 인터럽트를 발생시킴
- CPU에 너무 자주 인터럽트를 걸면 성능이 저하될 수 있으므로 DMA를 중간에 놓는 것!

<br>

### 🤖 서로 다른 입출력 명령어
- I/O를 수행하는 special instruction에 의해  
  (메모리처럼 I/O도 주소가 있어서 주소에 따라 접근함)
- Memory Mapped I/O에 의해  
 (I/O주소도 메모리 주소의 연장으로 붙임)

![](https://velog.velcdn.com/images/jimeaning/post/ec78c95b-fc41-4852-a9b4-d495d5fd43db/image.png)


<br>

### 🤖 저장장치 계층 구조

![](https://velog.velcdn.com/images/jimeaning/post/886be606-4e25-4206-b536-12b37bfe6225/image.png)

맨 위에는 CPU가 있고 그 밑에 레지스터, 캐시 메모리, 메인 메모리 순으로 구성되어 있음.  
특징: 위로 갈수록 속도가 빠르지만 비싸기 때문에 용량이 적다  
Volatility : 휘발성 (연두색은 휘발성. 전원을 끄면 사라짐. 핑크색이 비휘발성)  
하드 디스크는 바이트 단위가 아닌 섹터 단위로 접근이 가능함 -> Executable 하지 않음  
캐시 메모리 : 메인 메모리보다는 작지만 당장 필요한 것들만 위에서 올려 씀. (Caching)  
캐싱 : 재사용을 목적으로 함.


<br>

### 🤖 프로그램의 실행 (메모리 load)
![](https://velog.velcdn.com/images/jimeaning/post/b1bb5174-2c2d-45e7-821f-1f3e117b4f33/image.png)

물리 메모리로 넘어가기 전에 가상 메모리를 지나야 함.  
모든 프로그램은 독자적인 주소를 가지고 있음.  
커널은 부팅 후 컴퓨터에 항상 상주하고 있음.  
가상 메모리에서 당장 필요한 부분만 물리 메모리에 올려 낭비를 막음. (필요 없어지면 메모리에서 쫓아냄)  
필요하지 않은 부분은 디스크(Swap area)에 내려 놓음.  
파일 시스템의 하드 디스크는 전원이 나가도 파일 내용은 유지되어야 하기 때문에 비휘발성

주소 변환: 주소를 변환하는 하드웨어의 도움을 받아 물리 메모리 주소로 변환함

<br>

### 🤖 커널 주소 공간의 내용

![](https://velog.velcdn.com/images/jimeaning/post/98ec7dc3-7f6d-41e4-9060-a9fee4b4fb6c/image.png)

커널 코드 : 운영체제의 목적(자원 관리, 편리한 서비스 제공)에 필요한 것들이 들어 있음  
데이터 : 운영체제의 자료구조가 저장되어 있음  
PCB : Process Control Block  
Stack : 함수를 호출하거나 실행할 때 스택이 필요. 사용자 프로그램마다 스택을 따로 두고 있음.

<br>

### 🤖 사용자 프로그램이 사용하는 함수
- 함수
  - 사용자 정의 함수
    - 자신의 프로그램에서 정의한 함수
  - 라이브러리 함수
    - 자신의 프로그램에서 정의하지 않고 갖다 쓴 함수
    - 자신의 프로그램의 실행 파일에 포함되어 있다
  - 커널 함수
    - 운영체제 프로그램의 함수
    - 커널 함수의 호출 
    - 시스템 콜을 통해 사용 가능


<br>

### 🤖 프로그램의 실행

![](https://velog.velcdn.com/images/jimeaning/post/c1fcc964-a9b1-4ded-9e2e-3b183fae40b0/image.png)
