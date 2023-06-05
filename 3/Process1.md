# Process 1

### 🤖 프로세스의 개념
![](https://velog.velcdn.com/images/jimeaning/post/01d5449f-539d-416e-afd7-dcb80d7e0917/image.png)

"Process is **a program in execution**"  

프로세스의 문맥 (context)
- CPU 수행 상태를 나타내는 하드웨어 문맥
  - Program Counter
  - 각종 register
- 프로세스의 주소 공간
  - code, data, stack
- 프로세스 관련 커널 자료 구조
  - PCB (Process Control Block)
  - Kernel stack (각 프로세스가 실행되다가 본인이 할 수 없는 일을 CPU에 요청하는게 system call. 커널은 여러 프로세스가 공유하는 코드)  
  본인의 stack이 어떤 정보를 쌓고 있는지 알고 있을 필요


프로세스가 번갈아가면서 실행되기 때문에 CPU로 실행하다가 다른 프로세스로 넘겨줬다가 다시 실행했다가 하기 때문에 프로세스의 문맥(어느 시점까지 실행했는지) 알고 있어야 다음 instruction부터 실행 가능.

<br>

### 🤖 프로세스의 상태 (Process State)
프로세스는 상태(state)가 변경되며 수행된다.
- Running  
  - CPU를 잡고 instruction을 수행중인 상태
- Ready  
    - CPU는 하나 밖에 없기 때문에 CPU를 기다리는 상태 (메모리 등 다른 조건을 모두 만족하고)
- Blocked (wait, sleep)  
  - CPU를 주어도 당장 instruction을 수행할 수 없는 상태  
  - Process 자신이 요청한 event(ex: I/O처럼 오래 걸리는 상태)가 즉시 만족되지 않아 이를 기다리는 상태
  - 예) 디스크에서 파일을 읽어 와야 하는 경우
- Suspended (stopped)
  - 외부적인 이유로(사람이 정지시켜) 프로세스의 수행이 정지된 상태
  - 프로세스는 통째로 디스크에 swap out된다
  - 예) 사용자가 프로그램을 일시 정지시킨 경우 (break key)  
  시스템이 여러 이유로 프로세스를 잠시 중단시킴 (메모리에 너무 많은 프로세스가 올라와 있을 때)


- New: 프로세스가 생성 중인 상태
- Terminated: 프로세스가 종료 중인 상태. 수행(execution)이 끝난 상태

<br>

- Blocked: 자신이 요청한 event가 만족되면 Ready
- Suspended: 외부에서 resume해주어야 Active (사람이 정지시키고 다시 사람이 실행시켜야 함)

<br>

### 🤖 프로세스 상태도
![](https://velog.velcdn.com/images/jimeaning/post/9fba4864-5228-4642-b0cd-caf28d364dc1/image.png)

- Running  
두 가지가 가능함.  
1. 자진해서 CPU를 내놓는 경우는 I/O처럼 오래 걸리는 경우  
2. 나는 계속 CPU를 쓰고 싶은데 나눠서 써야 하는 경우 Time interrupt가 발생하고 얻었다 뺏겼다 하면서 실행됨

![](https://velog.velcdn.com/images/jimeaning/post/e6d8d4be-c8d4-4bfe-b98c-f023cda76bef/image.png)

놀이동산에 가서 줄 서고 타고 또 서고 타고 하는 것과 비슷함.  
놀이기구 중에서 빠른 거를 타면 타는 시간은 짧지만 대기는 오래 해야 할 수 있음 (= CPU는 빠르지만 큐에 대기가 많으면 프로세스는 오래 기다려야 함)  
시간이 길게 상영하기 때문에 오래 기다려야 하는 경우 (= 디스크나 입출력 장치에 기다리는 프로세스)  

![](https://velog.velcdn.com/images/jimeaning/post/ad8b4492-6d22-4172-abb3-9b1643d205a8/image.png)

큐는 운영체제 커널이 본인의 데이터 영역에 자료구조 형태로 저장되어 있음

<br>

#### Suspended 추가한 버전의 프로세스 상태도

![](https://velog.velcdn.com/images/jimeaning/post/d2acf7a4-e34c-4ef9-9c18-73b8e06e426a/image.png)

![](https://velog.velcdn.com/images/jimeaning/post/d8a9735c-42df-452e-a663-c14da5c1de85/image.png)


<br>

### 🤖 Process Control Block (PCB)
- 운영체제가 각 프로세스를 관리하기 위해 프로세스 당 유지하는 정보
- 다음의 구성 요소를 가진다 (구조체로 유지)
  1. OS가 관리상 사용하는 정보
   - Process state, Process ID
   - scheduling information, priority
  2. CPU 수행 관련 하드웨어 값
   - Program counter, registers
  3. 메모리 관련
   - Code, data, stack의 위치 정보
  4. 파일 관련
   - Open file descriptors

![](https://velog.velcdn.com/images/jimeaning/post/ed2715af-0a8a-40e8-87f6-0ac9a55f1858/image.png)


<br>

### 🤖 문맥 교환 (Context Switching)
CPU는 굉장히 빠르기 때문에 어떤 한 프로세스가 장악해서 계속적으로 쓰는 게 아니라, 짧은 시간을 얻었다가 뺏겼다가 함.  
다시 실행할 때 처음부터 실행되는 것이 아니라 끝난 부분부터 다시 시작하려면 문맥을 알고 있어야 함.
- CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정
- CPU가 다른 프로세스에게 넘어갈 때 운영체제는 다음을 수행
  - CPU를 내어주는 프로세스의 상태를 그 프로세스의 PCB에 저장
  - CPU를 새롭게 얻는 프로세스의 상태를 PCB에서 읽어 옴

![](https://velog.velcdn.com/images/jimeaning/post/f2e22f58-eb88-467d-a50b-6fd0962b2764/image.png)

사용자 프로세스로부터 다른 사용자 프로세스로 넘어가는 것이 Context Switching.

<br>


**헷갈리기 쉬운 부분**  

System call이나 Interrupt 발생 시 반드시 context switching가 일어나는 것은 아님 (사용자 프로세스가 CPU로 넘어가기 때문)

![](https://velog.velcdn.com/images/jimeaning/post/2c1505c1-f3c2-47e8-a679-5ff9d31f26e8/image.png)

(1) 유저모드로 사용자 프로세스 A가 실행되다가 interrupt되면 ISR(Interrupt Service Routine)을 실행시키고, system call이 되면 system call 커널 함수를 실행함.  
문맥 교환 없이 유저 모드로 빠져나감

(2) 사용자 프로세스A가 실행되다가 timer interrupt가 들어오면 또 다른 사용자 프로세스한테 CPU를 넘기게 됨.  
timer interrupt는 CPU를 다른 프로세스한테 넘기기 위한 의도를 가진 interrupt.

혹은 I/O처럼 오래 걸리는 요청을 하면 이 프로세스한테 줘봤자 blocked된 상태이기 때문에 ready(바로 실행할 수 있는) 프로세스한테 CPU를 줌.

(1)의 경우에도 CPU 수행 정보 등 context의 일부를 PCB에 save 해야 하지만 문맥 교환을 하는 (2)의 경우 그 부담이 훨씬 큼 (eg. cache memory flush)

<br>

### 🤖 프로세스를 스케줄링하기 위한 큐
Job queue
- 현재 시스템 내에 있는 모든 프로세스의 집합

Ready queue
- 현재 메모리 내에 있으면서 CPU를 잡아서 실행되기를 기다리는 프로세스의 집합
  
Device queues
- I/O device의 처리를 기다리는 프로세스의 집합

프로세스들은 각 큐들을 오가며 수행된다

![](https://velog.velcdn.com/images/jimeaning/post/ed0eac7d-7a37-4da2-ad44-bb65ac3cea3d/image.png)


<br>


### 🤖 프로세스 스케줄링 큐의 모습

![](https://velog.velcdn.com/images/jimeaning/post/29af5f28-9521-489d-8267-e68eeed55fea/image.png)

프로그램이 시작 되면 ready queue에 저장됨.   
CPU를 얻고 할당 시간이 끝나면 ready queue에 줄 섬.  

오래 걸리는 작업을 수행하면 해당 큐에 줄 서 있다가 작업이 끝나면 다시 ready queue로 줄 섬.  
본인의 작업이 다 끝나면 프로그램이 종료됨.

프로세스가 CPU를 잡고 있다가 자식 프로세스를 만들 수 있음


<br>

### 🤖 스케줄러 (Scheduler)
- Long-term scheduler (장기 스케줄러 or Job Scheduler)
  - 시작 프로세스 중 어떤 것들을 ready queue로 보낼지 결정 (메모리에 프로그램을 너무 많이 올리거나 너무 적게 올리면 성능에 안 좋음. 메모리에 몇 개 올릴지를 결정하는 장기 스케줄러)
  - 프로세스에 memory(및 각종 자원)을 주는 문제
  - degree of Multiprogramming을 제어
  - time sharing system에는 보통 장기 스케줄러가 없음 (프로그램 올리는 만큼 다 무조건 ready) -> 중기 스케줄러로 결정함
- Short-term scheduler (단기 스케줄러 or CPU scheduler)
  - 어떤 프로세스를 다음 번에 running 시킬지 결정
  - 프로세스에 CPU를 주는 문제
  - 충분히 빨라야 함 (millisecond 단위)
- Medium-term scheduler (중기 스케줄러 or Swapper)
  - 여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아냄
  - 프로세스에게서 memory를 뺏는 문제
  - degree of Multiprogramming을 제어

<br>

### 🤖


<br>

### 🤖


<br>

### 🤖