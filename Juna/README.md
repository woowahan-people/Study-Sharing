# Study-Sharing
# 주제: Garbage Collector 근데 JVM을 곁들인 ,,,

## 1. Garbage Collector 정의

- GC는 자바의  메모리 관리 기법 중 하나
- JVM의 Heap 영역에서 사용하지 않는 객체를 삭제하는 프로세스를 가지고 있음
- 어떻게 삭제할까? 어떤 객체에 유효한 참조가 존재한다면 Reachable, 그렇지 않다면 Unreachable이라고 함
- Unreachable이 GC의 수거 대상
- 동적으로 할당했던 메모리 영역(Heap영역)에서 필요 없게 된 영역(어떤 변수도 가리키지 않음)을 알아서 해제시켜 줌
- C나 C++은 수동, But 자바에서는 GC를 통해 수동으로 메모리를 관리하던 것에서 비롯한 에러들을 방지할 수 있음

### 📌GC의 장점

- Memory leak ↓
- 해제된 메모리 접근 막음
- 해제한 메모리 또 해제하는 이중 해제  막음

### 📌GC의 단점

- GC작업은 순수 오버헤드
- 개발자는 언제 GC가 메모리를 해제하는지 모름
- 실시간성이 매우 강조 되는 프로그램의 경우 맞지 않음

## 2. GC를 구현하는 대표적인 알고리즘

### 1) Reference Counting

- Reference Counting은 Heap 영역에 선언된 객체들이 각각 reference count라는 별도의 숫자를 가지고 있다고 생각하면 좋음. 해당 객체에 접근할 수 있는  방법이 없다면 즉 reference count가 0에 다다르면 GC의 대상이 되는 것

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/99339eee-46e7-4160-90ab-bcf96f640915/Untitled.png)

- Reference Counting의 한계: 순환 참조 문제→ Root Space에서의 Heap space 접근을 모두 끊는 다고 생각했을 때 서로가 서로를 참조하고 있기 때문에 reference count가 1로 유지됨 → 사용하지 않는 메모리 영역이 해제되지 못하고 Memory leak 발생
- 

### **2) Mark And Sweep**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/467cae6b-18d0-4bbc-9d7b-708e93036a41/Untitled.png)

- Mark: GC는 GC Root로부터 모든 변수를 스캔하면서 각각 어떤 객체를 참조하고 있는지(연결된 객체를 찾아냄) 찾아서 마킹한다(reachable과 unreachable을 식별하는 과정)
- Sweep: Unreachable 객체들을 Heap에서 제거하는 과정(연결이 끊어진 객체들은 지운다는 뜻, 힙에서 객체를 제거하는 과정을 쓸어내린다고해서 Sweep)
- 알고리즘에 따라 compact 과정이 추가되기도 함
- compact: sweep 후에 분산된 객체들을 heap의 시작 주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 나눔(메모리 단편화를 막아주는 과정)
- Mark And Sweep은 Reference Counting의 순환 참조 문제를 해결할 수 있음
- Mark And Sweep은 루트에서부터 해당 객체에 접근 가능한지를 해제의 기준으로 삼음
- sweep이후에 분산되어 있던 메모리가 예쁘게 정리된 것을 볼 수 있는데 이를 메모리 파편화를 막는 compaction이라고 함(mark and sweep에서 compaction은 필수가 아님)
- mark and sweep 방식을 사용하면 루트로부터 연결이 끊긴 순환 참조되는 객체들도 모두 지울 수 있음
- 자바와 자바스크립트가 mark and sweep 방식으로 메모리 관리를 함

## 3. JVM의 GC - JVM 구조

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a05ef52e-f7e9-41e7-a085-868d81121e69/Untitled.png)

- JVM은 크게 3가지로 구성됨
    
    1) Class loader: 바이트 코드를 읽고, 클래스 정보를 메모리의 Heap/Method Area에 저장하는 클래스로더
    
    2) JVM 메모리: 실행 중인 프로그램의 정보가 올라가 있는 메모리
    
    3) 바이트 코드를 네이티브 코드로 변환시켜주고 GC를 실행하는 실행 엔진이 있음 
    
- JVM 실행엔진이 어떻게 GC를 돌리는지 알기 위해선 JVM 메모리를 조금 더 봐야함 → JVM은 OS로부터 메모리를 할당 받은 후  해당 메모리를 용도에 따라 여러 영역으로 나누어서 관리함 → 총 5가지 영역으로 나눔 → 크게 2가지로 나눌 수 있음 → 모든 쓰레드가 공유하는 영역으로 Method Area와 Heap 영역이 있고 → 각 쓰레드마다 고유하게 생성하며 쓰레드 종료 시 소멸되는 Stack, PC Register, Native Method Stack 영역이 있음
- Metho Area는 프로그램의 클래스 구조를 메타데이터처럼 가지며, 메서드의 코드들을 저장해둠
- Heap은 어플리케이션 실행 중 생성되는 객체 인스턴스를 저장하는 영역임. 이 **Heap 영역이 오늘 주제인 Garbage Collector에 의해 관리되는 영역**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/977e4434-fb73-4b29-b272-0b0e02582494/Untitled.png)

- JVM의 Heap은 크게 두 영역인 Young Generation과 Old Generation으로 나눔
- Young Generation에서 발생하는 GC는 minor gc, Old Generation에서 발생하는 GC는 major gc라고 부름
- Young Generation은 세 영역 Eden/Survival 0/Survival 1 영역으로 나뉨
- Eden은 새롭게 생성된 객체들이 할당되는 영역, Survival 영역은 minor gc로부터 살아남은 객체들이 존재하는 영역
- survival 영역에는 특별한 규칙이 하나 있음, survival 0 혹은 survival 1 둘 중 하나는 꼭 비어 있어야 한다는 것
- 새로운 객체가 계속 생성되다 보면 Eden 영역이 꽉 차는 순간이 오겠죠 → 이때 minor GC가 일어남 → Mark and Sweep이 진행됨

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/26495e3f-84f9-4ba0-8e7d-bf300b3b3019/Untitled.png)

- 루트로부터 reachable이라 판단된 객체는 survival 0 영역으로 옮겨짐 → survival 0으로 옮겨진 객체들의 숫자가 0에서 1로 증가한 거 눈치 채셨나요? 해당 숫자는 age-bit를 뜻함. Minor gc에서 살아남을때마다 1씩 증가

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/734108af-5cf2-4ee4-85e8-db804dc3a2e5/Untitled.png)

- 시간이 흘러 Eden 영역이 또 꽉 찼음 → Minor gc 발생 → 반복 age-bit가 3이 되었음, 여기서 age- bit의 쓰임새가 밝혀짐, JVM GC에서는 일정 수준의 age- bit를 넘어가면 오래도록 참조될 객체구나라고 판단하여 해당 객체를 Old Generation에 넘겨 줌, 이 과정을 Promotion이라고 함. 자바 8 기준으로는 Parallel Gc방식 age bit가 15가 되면 promotion이 진행됨. 시간이 아주 많이 지나면 old generation도 다 채워지는 날이 옴. 이 때 major GC가 발생하면서  Mark and sweep 방식을 통해 필요 없는 메모리를 비워줌

![GC 영역 및 데이터 흐름도](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/959055b4-c5a5-40cf-8468-230c90620fb2/Untitled.png)

GC 영역 및 데이터 흐름도

- heap영역을 young generation과 old generation으로 나눈 데에는 이유가 있음. GC 설계자들이 어플리케이션을 분석해보니 대부분의 객체가 수명이 짧다는 것을 깨달음. GC도 결국 비용인데, 메모리의 특정 부분만을 탐색하며 해제하면 더 효율적이겠죠? 어차피 대다수의 객체가 금방 사라지니, Young generation 안에서 최대한 처리하도록 하는 것

## 4. Stop-the-world

- GC를 실행하기 위해 JVM이 애플리케이션 실행을 멈추는 것: GC를 실행하는 쓰레드 외의 모든 쓰레드가 작업을 중단함
- GC 방식에 따라 Stop the world 시간이 다름
- Old 영역은 기본적으로 데이터가 가득 차면 GC를 실행함. GC 방식에 따라서 처리 절차가 달라짐

## 5. Garbage Collector의 방식

- Serial GC
- Parallel GC
- Concurrent Mark Sweep GC
- G1 GC

### 📌Serial GC

- GC를 처리하는 스레드가 1개, CPU 코어가 1개만 있을 때 사용하는 방식,
- Serial GC는 하나의 쓰레드로 GC를 실행, 하나의 쓰레드다보니 Stop the world 시간이 긺. 싱글 쓰레드 환경 및 heap이 매우 작을 때 사용
- 이 중에서 운영 서버에서 절대 사용하면 안 되는 방식이 Serial GC. Serial GC는 데스크톱의 CPU 코어가 하나만 있을 때 사용하기 위해서 만든 방식. Serial GC를 사용하면 애플리케이션의 성능이 많이 떨어짐

### 📌Parallel GC

- GC를 처리하는 스레드가 여러 개
- Serial GC보다 빠르게 객체를 처리할 수 있음
- Parallel GC는 메모리가 충분하고 코어의 개수가 많을 때 사용하면 좋음
- Parallel GC는 여러 개의 쓰레드로 GC를 실행하며(serial GC보다 stop the world 시간이 짧아짐) 멀티코어 환경에서 사용, JAVA 8의 default GC 방식

### 📌CMS GC

- Stop-the-world 시간이 짧다, 애플리케이션의 응답 시간이 빨라야 할 때 CMS GC를 사용함.
- CMS GC는 stop-the-world 시간이 짧다는 장점에 반해 다음과 같은 단점이 존재함
    - 다른 GC 방식보다 메모리와 CPU를 더 많이 사용함
    - Compaction 단계가 기본적으로 제공되지 않음
- CMS GC는 stop the world 최소화를 위해 고안, GC 작업을 어플리케이션과 동시에 실행, G1 GC 등장에 따라 대체됨 , 메모리와 cpu를 많이 사용, mark and sweep이후 메모리 파편화를 해결하는 compaction이 기본적으로 제공되지 않음

### 📌G1 GC

- G1 GC를 이해하려면 지금까지의 Young 영역과 Old 영역에 대해서는 잊는 것이 좋음
- G1 GC는 바둑판의 각 영역에 객체를 할당하고 GC를 실행함. 그러다가, 해당 영역이 꽉 차면 다른 영역에서 객체를 할당하고 GC를 실행함
- GC가 일어날 때, 전체 영역(Eden Survival, Old Generation)을 탐색하지 않음
- G1 GC는 STW 시간이 짧다. compaction을 사용함
- G1 GC: G1은 가비지 퍼스트의 줄임말, Heap을 일정 크기의 region으로 잘게 나누어서 어떤 영역은 Young Generation, 어떤 영역은 Old Generation으로 활용함
- 런타임에 G1 GC가 필요에 따라 영역별 region 개수를 튜닝한다고 함, 그에 따라 Stop the world를 최소화 할 수 있었다고 함
- java 9이상부터는 g1 gc를 기본 gc 실행방식으로 사용

![G1 GC의 레이아웃](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d0d59087-f756-45ce-8fb4-730bd2485098/Untitled.png)

G1 GC의 레이아웃

### 📌 출처 및 참고

- 우아한Tech 조엘의 GC,
- 엘리의 GC,
- 던의 JVM Garbage Collector,
- [https://d2.naver.com/helloworld/1329](https://d2.naver.com/helloworld/1329)
