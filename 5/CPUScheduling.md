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

  (preemptive (줬어도 뺏어 올 수 있음), nonpreemptive (줬으면 못 뺏음) 용어 의미는 정확하게 기억해야 함)

<br>

### 🤖 Scheduling Criteria

- CPU utiliztion (이용률)
  - keep the CPU as busy as possible
  - 주방장이 놀지 않고 일하는 시간
- Throughput (처리량)
  - \# of processes that complete their execution per time unit
  - 단위 시간 당 몇 명의 손님을 받았는가
- Turnaround time (소요시간, 반환시간)
  - amount of time to execute a particular process
  - CPU가 다 쓰고 나가는 시간 (대기한 시간과 사용된 시간 다 합친 것)
  - (여기부터 손님 입장) 식사 다 하고 나갈 때까지 걸린 시간
- Waiting time (대기 시간)
  - amount of time a process has been waiting in the ready queue
  - CPU가 하나밖에 없기 때문에 ready queue에 들어가서 기다리는 시간
  - 밥을 기다리는 시간 (코스 요리면 여러 번일 수 있음)
- Response time (응답 시간)
  - amount of time it takes from when a request was submitted until the first response is produced. not output (for time-sharing enviornment)
  - ready queue에 CPU 쓰러 들어 와서 처음 CPU를 얻기까지 걸리는 시간
  - waiting time: CPU 뺏겼다가 맨 뒤에 줄 서서 다시 기다릴 수 있음. 얻었다 뺏겼다.. 이 모든 시간을 다 합친 시간
  - response time: 최초의 CPU를 얻기까지 걸린 시간만
  - 첫 번쨰 음식이 나올 때까지 기다리는 시간


<br>

### 🤖 Scheduling Algorithm

### ✔️ FCFS (First-Come First-Served)

- CPU를 먼저 잡아 버리면 CPU를 짧게 쓰는 프로그램이 들어 와도 계속 기다려야 해서 효율적이지 못함
- CPU를 짧게 쓰는 프로그램 순으로 들어오면 waiting time 평균이 빨라짐
- 따라서, 앞에 어떤 프로그램이 들어 왔는지에 따라 완전히 달라짐

### ✔️ SJF (Shortest-Job-First)

- 각 프로세스의 다음 번 CPU burst time을 가지고 스케줄링에 할당
- CPU burst time이 가장 짧은 프로세스를 제일 먼저 스케줄
- Two schemes:
  - **Nonpreemptive**
    - 일단 CPU를 잡으면 이번 CPU burst가 완료될 때까지 CPU를 선점(preemption) 당하지 않음
  - **Preemptive**
    - 현재 수행중인 프로세스의 남은 burst time 보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗김
    - 이 방법을 Shortest-Remaining-Time-First(SRTF)이라고도 한다
- SJF is optimal
  - 주어진 프로세스들에 대해 **minimum average waiting time**(preemptive 버전)을 보장
- 문제점
  - long process가 계속 기다리는 starvation(기아 현상)이 발상핼 수 있음.
  - CPU 사용량을 미리 알 수 없음 -> 실제 시스템에서 SJF를 사용할 수 없음 (과거 사용량을 바탕으로 추정/예측해서 사용이 가능하긴 함)

### 다음 CPU Burst Time의 예측
- 다음 번 CPU burst time을 어떻게 알 수 있는가? (input data, branch, user ..)
- 추정(estimate)만이 가능하다
- 과거의 CPU burst time을 이용해서 추정 (exponential averaging)
  
  ![](https://velog.velcdn.com/images/jimeaning/post/f91f019d-6325-4be1-b7e3-99c8b4903dc3/image.png)

### ✔️ Priority Scheduling

- A priority number (integer) is a associated with each process
- highest priority를 가진 프로세스에게 CPU 할당 (smallest integer - highest priority)
  - Preemptive
  - nonpreemptive
- SJF는 일종의 priority scheduling이다
  - priority = predicted next CPU burst time
- Problem
  - **Starvation** (기아 현상): low priority processes may *never execute*
- Solution
  - **Aging** (노화) : as time progresses *increase the priority* of the process


### ✔️ Round Robin (RR)

- 현대적 스케줄링은 대부분 RR에 기반 (선점형)
- 각 프로세스는 동일한 크기의 할당 시간(**time quantum**)을 가짐 (일반적으로 10-100 milliseconds)
- CPU를 최초로 얻기까지 걸리는 시간(응답 시간)이 빨라짐
- 할당 시간이 지나면 프로세스는 선점(preempted) 당하고 ready queue의 제일 뒤에 가서 다시 줄을 선다
- n개의 프로세스가 ready queue에 있고 할당 시간이 q time unit인 경우 각 프로세스는 최대 q time unit 단위로 CPU 시간의 1/n을 얻는다
  - 어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다
- Performance
  - q large => FCFS
  - q small => context switch 오버헤드가 커진다
- 일반적으로 SJF보다 average turnaround time(소요 시간)이 길지만 response time은 더 짧다.