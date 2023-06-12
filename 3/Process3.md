# Process 3

### 🤖 Thread

![](https://velog.velcdn.com/images/jimeaning/post/20485412-783a-4c0c-b03b-57bcf577bd5b/image.png)

각 스레드마다 CPU 수행과 관련된 정보만 별도로 갖고 있게 됨.


<br>

### 🤖 Single and Multithreaded Process

![](https://velog.velcdn.com/images/jimeaning/post/3b59a61f-ec54-423f-a406-d2940a1051d9/image.png)

프로세스 안에 스레드 하나 있는 것(좌측)과 여러 개 있는 것(우측)  
바이너리라고 쓰여 있는 것은 무시하고, 공유하는 부분과 CPU와 관련된 정보로 구성되어 있음.

<br>

### 🤖 Benefits of Thread
- ***Responsiveness (응답성)***  
  eg) multi-threaded Web - if one thread is blocked(eg Network), another thread continues(eg display)  
사용자 입장에서 응답이 빠르다.  
 프로세스가 block 되었을 때 화면에 아무것도 display되지 않으면 사용자는 답답함을 느낌. 웹 서버 요청할 때 텍스트부터 가져온 후, 이미지에 대한 요청은 이후에 해서 응답성을 높임. 
- ***Resource Sharing (자원 공유성)***  
  n threads can share binary code, data, resource of the process  
  똑같은 일을 하는 프로그램이 여러 개 있을 때, 별도의 프로세스를 만드는 것보다 하나의 프로세스를 두고 여러 스레드를 두는 것이 자원 효율성 측면에서 좋음
- ***Economy (경제성)***  
  creating & CPU switching thread (rather than a process)  
  Solaris의 경우 위 두 가지 overhead가 각각 30배, 5배  
  빠르다는 뜻 (응답성이랑은 조금 다름)  
  프로세스를 하나 만드는 것은 오버헤드가 상당히 큼. 하지만 프로세스 안에 스레드를 추가하는 것은 숟가락만 하나 늘리는 것과 같아 오버헤드가 크지 않음.  
  프로세스 내부에서 스레드 간 CPU switch가 일어나는 것은 매우 간단함 (주소를 공유하면 쓰고 있기 때문)
- ***Utilization of MP(Multi Processor) Architectures***  
  each thread may be running in parallel on a different processor  
CPU가 여러 개 있을 때 얻을 수 있는 장점  
CPU가 병렬적으로 일하면서 큰 행렬을 곱하는 등 효율적인 실행이 가능.

<br>

### 🤖 Implementation of Thread

스레드를 구현할 수 있는 방법

- ***Kernel Thread***
  - Some are supported by *kernel*
  - Windows 95/98/NT, Solaris, Digital UNIX, Mach

스레드가 여러 개 있다는 사실을 운영체제가 알고 있어서 커널이 CPU 스케줄링 하듯이 넘겨 줌.  
커널의 지원을 받음.

- ***User Thread***
  - Others are supported by *library*
  - POSIX Pthreads, Mach C-threads, Solaris threads  
  
라이브러리를 통해 지원됨.  
프로세스 안에 스레드가 여러 개 있다는 사실을 운영체제가 모르고 유저 프로그램이 여러 스레드를 스스로 관리함.  
커널이 볼 땐 일반적인 프로세스처럼 보임.  
구현 상 제약이 있을 수 있음.
커널의 지원을 받지 않음

- ***Some are real-time threads***

리얼 타임 기능을 제공하는 스레드
