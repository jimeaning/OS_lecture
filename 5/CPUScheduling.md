# CPU Scheduling 1

### 🤖 CPU and I/O Bursts in Program Execution

![](https://velog.velcdn.com/images/jimeaning/post/640c3769-fad8-4d58-a743-3a44ee2980a7/image.png)

CPU만 연속적으로 실행하는 단계(CPU burst)와 I/O를 실행하는 단계(I/O burst)가 번갈아 가면서 나타남.  
프로그램 종류에 따라서 I/O가 많이 끼어드는게 있고(사용자 개입이 많은 interactive 프로그램), CPU만 진득하게 실행하는 프로그램이 있음(계산이 많은 프로그램).  

<br>

### 🤖 CPU-burst Time

![](https://velog.velcdn.com/images/jimeaning/post/cda326ba-becf-4563-950d-877834e498a2/image.png)

대부분 CPU job은 I/O bound job이 거의 다 쓰는 구나 라고 생각할 수 있지만 CPU를 짧게 쓰는데 빈도가 많아서 그래프가 높게 나타나는 것임. (실제로 CPU를 길게 쓰는 것은 CPU bound job)  
I/O bound job은 사람과의 interactive가 많은데, CPU bound job이 CPU를 많이 잡고 있으면 사람이 답답할 수 있음. 공평한 것보다도 interactive job이 너무 오래 기다리지 않도록 스케줄링을 해야 함.


여러 종류의 job(=process)이 섞여 있기 때문에 CPU 스케줄링이 필요하다
- Interactive job에게 적절한 response 제공 요망
- CPU와 I/O 장치 등 시스템 자원을 골고루 효율적으로 사용

<br>

### 🤖 프로세스의 특성 분류

- 프로세스는 그 특성에 따라 다음 두 가지로 나눔
  - I/O-bound process
    - CPU를 잡고 계산하는 시간보다 I/O에 많은 시간이 필요한 job
    - many short CPU bursts
  - CPU-bound process
    - 계산 위주의 job
    - few very long CPU bursts

<br>

### 🤖 CPU Scheduler & Dispatcher
- CPU Scheduler
  - Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다

- Dispatcher
  - CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에게 넘긴다
  - 이 과정을 context switch(문맥 교환)라고 한다

- CPU 스케줄링이 필요한 경우는 프로세스에게 다음과 같은 상태 변화가 있는 경우이다  
  1. Running -> Blocked (예: I/O 요청하는 시스템 콜)
  2. Running -> Ready (예: 할당시간만료로 time interrupt)
  3. Blocked -> Ready (예: I/O 완료 후 인터럽트)
  4. Terminate

  <br> - 1, 4에서의 스케줄링은 **nonpreemptive** (= 강제로 빼앗지 않고 자진 반납)  
  \- All other scheduling is **preemptive** (= 강제로 빼앗음)

  (preemptive, nonpreemptive 용어 의미는 정확하게 기억해야 함)