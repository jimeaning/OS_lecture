# Process 2

### 동기식 입출력과 비동기식 입출력

(질문 받은 내용(동기식과 비동기식의 차이가 궁금해요) 설명해 주심)

![](https://velog.velcdn.com/images/jimeaning/post/33634aed-0b46-4349-a8e3-9496c109166f/image.png)

- 동기식 입출력

입출력 끝날 때까지 아무 일 안하고 기다림  
프로세스가 입출력이 진행되는 동안 instruction을 실행하지 않고 기다림


- 비동기식 입출력

입출력 요청해 놓고 instruction을 실행  
입출력 진행되는 동안에 cpu 잡아서 instruction을 실행

하지만 CPU를 잡고 있는지 아닌지는 구분 요소가 아님  
(자세한 설명은 [여기 참고](https://github.com/jimeaning/OS_lecture/blob/main/2/SystemStructure%26ProgramExecution2.md))

<br>

### 🤖 Thread
A **thread** (or **lightweight process**) is a basic unit of CPU utilization  
프로세스 내부에 CPU 수행 단위가 여러 개 있는 경우 thread 라고 부른다.

![](https://velog.velcdn.com/images/jimeaning/post/73e31cff-db14-419b-9935-9de23bf58479/image.png)


주소 공간이 프로세스마다 만들어지는데, 이 프로세스 하나를 관리하기 위해 PCB 존재.  
PCB는 프로세스의 상태를 나타내고, 프로세스의 id, 프로세스 카운터(어디를 실행하고 있는지 가리키는 것)로 구성된다.

프로세스마다 주소공간이 만들어져 메모리가 낭비됨.  
같은 일을 하는 프로세스를 여러 개 띄워 놓고 싶을 때는 메모리 공간은 하나만 띄워 놓고 프로세스마다 다른 코드를 실행할 수 있게 해주면 되겠죠? -> 그게 스레드!

프로그램 카운터만 여러 개를 두는 것을 스레드라고 함.  
각 스레드마다 현재 레지스터에 어떤 값을 넣고, 프로그램 어느 부분을 가리키며 실행하는지에 대한 정보를 갖고 있음.  

프로세스 하나에서 스레드끼리 공유할 수 있는 건 최대한 공유하고(메모리 주소 공간, 프로세스 상태, 프로세스가 사용하는 각종 자원들) 별도로 가진 것(프로그램 카운터, 레지스터, 스택 등의 정보)으로 구분된다.

#### Thread의 구성
- program counter
- register set
- stack space

CPU 수행과 관련된 위 세 가지는 별도로 가지게 됨.

#### Thread가 동료 스레드와 공유하는 부분 (= task)
- code section
- data section
- OS resources

전통적인 개념의 heavyweight process는 하나의 스레드를 가지고 있는 task로 볼 수 있다

<br>

### 🤖 Thread 사용 시 장점
- 다중 스레드로 구성된 테스크 구조에서는 하나의 서버 스레드가 blocked(waiting) 상태인 동안에도 동일한 테스크 내의 다른 스레드가 실행(running)되어 빠른 처리를 할 수 있다.  
ex) 네트워크를 통해서 웹 페이지를 읽어 오는 작업도 I/O라서 오래 걸림. 웹 브라우저를 읽어 오는 동안에 웹 브라우저는 blocked 상태라 아무 일도 못하게 됨.   
-> 사용자는 웹 페이지를 불러오기 전까지 아무 화면도 보지 못하기 때문에 답답함.  
해결) 프로세스를 block 시키지 않고 이미 불러온 텍스트라도 보여주면 답답함이 덜해질 수 있음.  
즉, **하나의 스레드가 다른 작업을 하는 동안 다른 스레드가 별도의 화면을 보여주면 사용자에게 빠른 응답성 제공 가능**
-  동일한 일을 수행하는 다중 스레드가 협력하여 높은 처리율(throughput)과 성능 향상을 얻을 수 있다
-  스레드를 사용하면 병렬성을 높일 수 있다.

