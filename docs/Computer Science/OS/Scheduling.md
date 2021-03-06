# 운영체제는 어떤 프로세스에게 CPU를 할당할까?

- 스케쥴링 : 운영체제가 여러 프로세스의 CPU 할당 순서를 결정하는 것을 말하고 이 일을 하는 프로그램을 스케쥴러라 한다.

- 스케쥴링은 CPU를 언제 할당하는지에 따라 선점형 스케쥴링과 비 선점형 스케쥴링으로 나뉜다.
    - 선점형 스케쥴링(최근의 운영체제에서 사용되는 방식)
        - 어떤 프로세스가 실행중에 있어도 스케쥴러가 강제로 실행을 중지하고 다른 프로세스에 CPU를 할당할 수 있다.
    - 비 선점형 스케쥴링
        - 실행중인 프로세스가 명시적으로 CPU를 반환(I/O 작업 혹은 종료)하기전가지는 CPU를 얻을 수 없다.
        
## 대표적인 스케쥴링 알고리즘

- 우선순위 알고리즘


            프로세스에 우선순위를 매겨 우선 순위가 높은 프로세스에 CPU를 할당한다.
            CPU를 할당받은 프로세스가 실행중에 더 우선 순위가 높은 프로세스가 생성되면
            실행중인 프로세스를 실행가능 상태로 만들고, CPU를 넘긴다.
            계속해서 우선순위가 낮아 CPU를 할당받지 못하는 프로세스는 기아상태가 되고,
            일정시간 CPU를 할당받지 못하면 우선 순위를 높여 실행될 수 있도록 만드는데,
            이러한 방법을 에이징이라고 한다.
            
- 라운드 로빈 알고리즘(대표적인 선점형 스케쥴링)


            실행가능 상태에 있는 프로세스들을 순서대로 가져와 일정시간동안 CPU를 할당하는 방식
            모든 프로세스에 같은 시간(타임 슬라이스)이 부여된다.
            가장 중요한 것은 타임슬라이스를 얼마로 정하느냐인데, 너무 짧으면 컨텍스트 스위칭이 잦아
            시스템에 부담이 되고, 너무 길면 멀티 태스킹을 구현하는데 문제가 생긴다.
            
- FCFS(First Come First Seved)


            실행가능 상태에 먼저 들어온 프로세스에 먼저 CPU를 할당하는 비선점 스케쥴링 방식
            

- SJF(Shortest Job First)

            CPU 할당 시간이 가장 짧은(예측에 의존) 프로세스 먼저 실행,
            우선순위 알고리즘 처럼 할당시간이 긴 프로세스는 계속 CPU를 할당받지 못해
            기아상태에 빠질 수 있다.
            
            
            
# 컨텍스트 스위칭
- 프로세스가 실행되려면 다양한 CPU 레지스터값과 프로세스 상태정보(프로세스마다 각기 다른)등이 필요하다.<br>
그래서 실행상태 - > 실행가능상태로 변경될 때(CPU를 반납할 때) 이러한 정보를 메모리 어디간에 저장해야하고<br>
이러한 메모리 블록을 프로세스 제어 블록(PCB)라고 한다.<br>
PCB데이터는 프로세스가 실행가능 상태 -> 실행상태로 바뀔때도 필요하다.
    - PCB에 저장된 상태값이 있어야 이전에 실행한 인스트럭션들의 정보를 알 수 있기 떄문!!

- CPU의 상태를 컨텍스트라고 부르는데, 스케쥴러가 CPU를 할당하고 해제할때 실행중인 프로세스의 CPU 상태정보를<br>
그 프로세스의 PCB에 저장하고, 실행될 프로세스의 PCB에서 이전 CPU 상태정보를 CPU로 가져오는 것을<br>
"컨텍스트 스위칭"이라 한다.
    - 이러한 행위는 시스템에 부담을 주고, 잦은 컨텍스트 스위칭은 시스템 성능을 떨어트린다.
    - 어떻게하면 컨텍스트 스위칭을 덜 할 것인가에 대한 정답은 없다.
    
