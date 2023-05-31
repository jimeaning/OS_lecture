# System Structure & Program Execution 1

### 🤖 컴퓨터 시스템 구조

![](https://velog.velcdn.com/images/jimeaning/post/91c4f7bb-823a-495a-9bf1-f0eb661037c5/image.png)
- CPU : 메모리의 instruction에 따라 실행하게 됨. 메모리로부터 기계어를 하나씩 읽어서 실행.  
    - 레지스터 : CPU 내부에서 메모리보단 더 빠르면서 정보를 저장할 수 있음  
    - mode bit : CPU에서 실행되는 것이 운영체제인지, 사용자 프로그램인지 구분하는 것.  
    - Interrupt line : CPU는 메모리에 있는 instruction(기계어)만 실행함. 근데 키보드에서 입력이 들어 오거나(scanf) 디스크에 뭔가 들어 왔을 때, CPU가 interrupt line을 통해 알 수 있음. 즉, 메모리에 있는 것만 작업하는 것이 아니라(변수) 화면에 출력하거나(출력 함수) 디스크에서 무엇을 읽고 쓸 수 있음.  
    CPU가 직접 디스크에 접근하는 것이 아니라 디스크 컨트롤러에 일을 시킴. 그 사이 CPU는 메모리를 읽고 있음.

- 메모리 : CPU의 작업공간  
사용자 프로그램은 본인이 직접 I/O 장치에 접근할 수 없음. 운영체제를 통해서만 가능하도록 막아 놨음. (보안 등의 이유로)  
I/O 작업이 필요할 때는 프로그램 스스로 운영체제에게 CPU 작업을 넘겨 줌.  
I/O controller의 작업이 끝나면 (키보드에서 입력을 받고 나면) 키보드 컨트롤러가 CPU에 interrupt를 건다 
interrupt가 들어 오면 CPU는 자동적으로 운영체제로 넘어 감.  
입력된 키보드 값을 메모리 공간에 copy해주고, 할당 시간(타이머)이 남았으면 다시 프로그램에 CPU를 준다.

- 하드 디스크 : 보조 기억장치. I/O Device로도 볼 수 있음.  

- 컨트롤러 : 각 I/O Device에 붙어 있어 전담하는 역할을 함.  
CPU와 I/O Device의 속도 차이가 많이 나기 때문에 컨트롤러가 담당함

- 타이머 : 특정 프로그램이 CPU를 독점하는 것을 막기 위함  
사용자 프로그램이 실행되다가 타이머에 할당된 시간이 되면 타이머는 CPU에 interrupt를 건다

- DMA Controller (Direct Memory Access) : 직접 메모리에 접근할 수 있는 컨트롤러.  


밑에서 자세히 ..

<br>

### 🤖 Mode bit
사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 운영체제에 피해가 가지 않도록 하기 위한 보호 장치 필요

Mode bit을 통해 하드웨어적으로 두 가지 모드의 operation 지원

> 1 사용자 모드 : 사용자 프로그램 수행 (한정된 instruction만 수행 가능)  
0 커널 모드(= 모니터 모드, 시스템 모드) : OS 코드 수행 (모든 instruction 다 실행 가능)  

- 보안을 해칠 수 있는 중요한 명령어는 모니터 모드에서만 수행 가능한 '특권 명령'으로 규정
- Interrupt나 Exception 발생 시 하드웨어가 mode bit을 0으로 세팅
- 사용자 프로그램에게 CPU를 넘기기 전에 mode bit을 1로 세팅

<br>

### 🤖 Timer
- 정해진 시간이 흐른 뒤 운영체제에게 제어권이 넘어가도록 인터럽트를 발생시킴
- 타이머는 매 클럭 틱 때마다 1씩 감소
- 타이머 값이 0이 되면 타이머 인터럽트 발생
- CPU를 특정 프로그램이 독점하는 것으로부터 보호


<br>

- 타이머는 time sharing을 구현하기 위해 널리 이용됨
- 타이머는 현재 시간을 계산하기 위해서도 사용

<br>

### 🤖 Device Control
- I/O device controller
  - 해당 I/O 장치 유형을 관리하는 일종의 작은 CPU
  - 제어 정보를 위해 control register, status register를 가짐
  - local buffer를 가짐 (일종의 data register)

- I/O는 실제 장치와 로컬 버퍼 사이에서 일어남
- Device controller는 I/O가 끝났을 경우 인터럽트로 CPU에 그 사실을 알림

<br>

*device driver (장치구동기)*  
: OS 코드 중 각 장치별 처리 루틴 -> software

*device controller (장치제어기)*  
: 각 장치를 통제하는 일종의 작은 CPU -> hardware

<br>

### 🤖 입출력의 수행
- 모든 입출력 명령은 특권 명령
- 사용자 프로그램은 어떻게 I/O를 하는가?
  - 시스템콜 (system call)  
  사용자 프로그램이 운영체제의 서비스를 받기 위해 커널 함수를 호출하는 것  
    사용자 프로그램은 운영체제에게 I/O 요청
  - trap을 사용하여 인터럽트 벡터의 특정 위치로 이동
  - 제어권이 인터럽트 벡터가 가리키는 인터럽트 서비스 루틴으로 이동
  - 운영체제는 올바른 I/O 요청인지 확인 후 I/O 수행
  - I/O 완료 시 제어권을 시스템콜 다음 명령으로 옮김

<br>

### 🤖 인터럽트
- 인터럽트  
  인터럽트 당한 시점의 레지스터와 program counter를 저장한 후 CPU의 제어를 인터럽트 처리 루틴에 넘긴다
- 인터럽트 (넓은 의미)
  - Interrupt (하드웨어 인터럽트) : Device Controller 등 하드웨어가 발생시킨 인터럽트 (일반적 인터럽트)
  - Trap (소프트웨어 인터럽트) : 프로그램이 직접 인터럽트 발생  
  Exception: 프로그램이 오류를 범한 경우.   
  System call: 프로그램이 커널 함수를 호출하는 경우
- 인터럽트 관련 용어
  - 인터럽트 벡터  
  해당 인터럽트의 처리 루틴 주소를 가지고 있음
  - 인터럽트 처리 루틴 (=Interrupt Service Routine, 인터럽트 핸들러)  
  해당 인터럽트를 처리하는 커널 함수
