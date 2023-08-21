# Process Management 2

### 🤖 fork() 시스템 콜 

- A process is created by the fork() system call
  - creates a new address space that is a duplicate of the caller


```c
int main() {
    int pid;
    printf("\n Hello, I am child!\n");
    pid = fork();

    if (pid == 0)   /* this is child */
        printf("\n Hello, I am child!\n");
    else if (pid > 0)   /* this is child */
        printf("\n Hello, I am parent!\n");
}
```

fork를 하면 이렇게 같은 코드를 가지는 두 개의 프로세스가 만들어짐.
```c
int main() {
    int pid;
    printf("\n Hello, I am child!\n"); /* 자식 프로세스는 이 부분이 실행되지 않고 if문만 실행됨 */
    pid = fork();

    if (pid == 0)   /* this is child */
        printf("\n Hello, I am child!\n");
    else if (pid > 0)   /* this is child */
        printf("\n Hello, I am parent!\n");
}
```

<br>

### 🤖 exec() 시스템 콜

- A process can execute a different program by the exec() system call
  - replaces the memory image of the caller with a new program

```c
int main() {
    int pid;
    pid = fork();

    if (pid == 0)   /* this is child */
        printf("\n Hello, I am child! Now I'll run the date \n");
        execlp("/bin/date", "bin/date", (char *) 0);    /* date라는 새로운 프로그램으로 덮어 쓰게 됨. */
    else if (pid > 0)   /* this is child */
        printf("\n Hello, I am parent!\n");
}
```

exec을 한 번 하게 되면 다시 돌아갈 수 없음.  
(기억을 지우고 갓난 애기로 다시 태어났는데 무르고 싶다고 해서 다시 돌아갈 수 없는 법)

자식을 만들어서 exec을 해야 하는 건 아님. fork 빼고 이렇게 할 수도 있음.
```c
int main() {
    printf("\n Hello, I am child! Now I'll run the date \n");
    execlp("/bin/date", "bin/date", (char *) 0);    
    printf("\n Hello, I am parent!\n");     /* exec 이후에 있는 코드는 실행되지 않음 */
}
```

<br>

### 🤖 wait() 시스템 콜
- 프로세스 A가 wait() 시스템 콜을 호출하면
  - 커널은 child가 종료될 때까지 프로세스 A를 sleep 시킨다 (block 상태)
  - Child process가 종료되면 커널은 프로세스 A를 깨운다 (ready 상태)


<br>

### 🤖 exit() 시스템 콜
- 자발적 종료
  - 마지막 statement 수행 후 exit() 시스템 콜을 통해
  - 프로그램에 명시적으로 적어주지 않아도 main 함수가 리턴되는 위치에 컴파일러가 넣어줌

- 비자발적 종료
  - 부모 프로세스가 자식 프로세스를 강제 종료시킴
    - 자식 프로세스가 한계치를 넘어서는 자원 요청
    - 자식에게 할당된 태스크가 더이상 필요하지 않음
  - 키보드로 kill, break 등을 친 경우 (x버튼, ctrl+C 등)
  - 부모가 종료하는 경우
    - 부모 프로세스가 종료하기 전에 자식들이 먼저 종료됨

```c
int main() {
    int pid;
    printf("\n Hello, I am child!\n");
    exit();     /* 여기서 바로 종료됨 */
    pid = fork();

    if (pid == 0)   /* this is child */
        printf("\n Hello, I am child!\n");
    else if (pid > 0)   /* this is child */
        printf("\n Hello, I am parent!\n");
}
```

<br>

### 🤖 프로세스와 관련한 시스템 콜
- fork() : 부모 프로세스 복제 생성  
  create a child (copy)
- exec() : 완전히 새로운 프로그램으로 덮어 씌움  
  overlay new image
- wait() : 자식이 종료될 때까지 잠들어서 기다림  
  sleep until child is done
- exit() : 모든 자원을 반납하고 부모 프로세스에 종료한다고 알림  
  frees all the resources, notify parent

<br>

### 🤖 프로세스 간 협력
- 독립적 프로세스 (Independent process)
  - 프로세스는 각자의 주소 공간을 가지고 수행되므로 원칙적으로 하나의 프로세스는 다른 프로세스의 수행에 영향을 미치지 못함
- 협력 프로세스 (Cooperating process)
  - 프로세스 협력 메커니즘을 통해 하나의 프로세스가 다른 프로세스 수행에 영향을 미칠 수 있음
- 프로세스 간 협력 메커니즘 (IPC : Interprocess Communication)
  - 메시지를 전달하는 방법
    - message passing: 커널을 통해 메시지 전달 (아래 설명 참고)
  - 주소 공간을 공유하는 방법
    - shared memory: 서로 다른 프로세스 간에도 일부 주소 공간을 공유하게 하는 shared memory 메커니즘이 있음
    - **thread**: thread는 사실상 하나의 프로세스이므로 프로세스 간 협력으로 보기는 어렵지만 동일한 process를 구성하는 thread들 간에는 주소 공간을 공유하므로 협력이 가능


<br>

### 🤖 Message Passing
- Message system
  - 프로세스 사이에 공유 변수(shared variable)를 일체 사용하지 않고 통신하는 시스템
- Direct Communication
  - 통신하려는 프로세스의 이름을 명시적으로 표시
  ![](https://velog.velcdn.com/images/jimeaning/post/9de08e90-c589-4247-bf58-7902a019ec30/image.png)

- Indirect Communication
  - mailbox (또는 port)를 통해 메시지를 간접 전달
  ![](https://velog.velcdn.com/images/jimeaning/post/1d7232d2-104a-4653-9c90-92306981b3ce/image.png)

<br>

### 🤖 Interprocess Communication

![](https://velog.velcdn.com/images/jimeaning/post/a181dbc4-ee48-421b-8445-d42d60c17de6/image.png)

Message Passing : 둘 다 커널을 통해서 전달하지만, 메시지를 받아 볼 프로세스를 명시하느냐 아니면 메시지를 메일 박스에 집어 넣느냐 차이.

Shared Memory : 프로세스 A가 shared memory 안에 작성하면 프로세스 B가 본인 공간에서 바로 전달받아 볼 수 있음.  
커널한테 shared memory를 쓴다는 요청을 하고 사용할 수 있음. 이후에는 사용자 프로세스 둘 끼리 작업하면 됨.