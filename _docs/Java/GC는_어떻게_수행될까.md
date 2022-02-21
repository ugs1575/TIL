# GC는 어떻게 수행될까?

### GC란?

- Garbage Collection는 말 그대로 쓰레기를 정리하는 작업이다.
    - 자바에서 쓰레기는 객체이다.
    - 하나의 객체는 메모리를 점유하고, 필요하지 않으면 메모리에서 해제되어야 한다.
    - 참조변수에 의한 참조가 전혀 이뤄지지 않는 인스턴스가 소멸의 대상이 된다.
- 이 쓰레기 객체를 효과적으로 처리하는 작업을 GC라고 한다.
- GC는 힙 영역에서 일어난다.
- GC를 많이 하면 할수록 응답시간에 많은 영향을 끼친다
- 자바에서는 메모리를 GC라는 알고리즘을 통하여 관리하기 때문에, 개발자가 메모리를 처리하기 위한 로직을 만들 필요가 없고, 절대로 만들어서는 안된다.

### GC의 원리

- GC 작업을 하는 가비지 콜렉터는 다음의 역할을 한다.
    - 메모리 할당
    - 사용 중인 메모리 인식
    - 사용하지 않는 메모리 인식
- 사용하지 않는 메모리를 인식하는 작업을 수행하지 않으면, 할당한 메모리 영역이 꽉 차서 JVM에 행이 걸리거나, 더 많은 메모리를 할당하려는 현상이 발생할 것이다.
    - 행이란 서버가 요청을 처리 못하고 있는 상태를 말한다.

### GC단계

Step1. Marking

![https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide3.png](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide3.png)

- 첫 단계는 마킹하는 단계이다.
- 가비지 컬렉터는 어떤 객체가 메모리를 점유하고 있는지 또는 점유하고 있지 않은지 판별하는 단계이다.
- 모든 객체가 스캔되어야 하기 때문에 시간이 걸리는 작업이다.

Step2. Normal Deletion

![https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide1b.png](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide1b.png)

- 두번째 단계에서는 삭제되어야할 객체를 삭제한다.
- Memory Allocator는 삭제되고 비어있는 공간의 참조값을 들고있다가 새로운 객체의 할당이 이루어져야할 때 참고한다.

Step2a. Deletion with Compaction

![https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide4.png](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide4.png)

- 삭제와 더불어 남아있는 객체들을 왼쪽으로 밀어준다.
- 살아남은 객체들을 한 곳으로 같이 모음으로써 새로운 메모리 할당이 쉽고 빠르게 일어난다.
- Memory Allocator는 비어있는 공간의 시작점의 참조값을 쥐고있다가 차례대로 할당한다.

### 영역별 특징

- JVM의 힙영역은 크게 Young, Old, Perm 세 영역으로 나뉜다.

![https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide5.png](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide5.png)

- Young 영역
    - 새로운 객체가 할당되고 에이징(여러번의 GC를 거쳐 살아남는 객체)된다. Young영역이 꽉차게 되면 마이너 GC가 일어난다.
    - 살아남은 객체들이 에이징되다가 결국은 old영역으로 이동한다.
- Stop the World Event
    - 마이너 GC는 Stop the World Event를 일어킨다.
    - Stop the World 란 모든 애플리케이션 쓰레드들이 GC작업이 끝날때까지 멈추는 것을 의미한다.
    - 즉, 콜렉션이 수행될 때 애플리케이션 수행이 정지된다.
- Old 영역
    - Old 영역에서는 오랫동안 살아남은 객체들을 저장한다.
    - 오랫동안 살아남는 객체들은 Old영역으로 모이게되고 이것은 메이저 GC라고 한다.
    - 메이저GC도 Stop the World Event를 일어킨다. 메이저 GC는 모든 살아있는 객체들을 포함하므로 더 작업이 느리다. 그래서 메이저 GC를 최소화 하는 것이 좋다
    - 어떤 가비지 컬렉터를 사용하느냐에 따라 Stop the World Stop the World Event의 시간을 줄일 수 있다.
- Permanent 영역
    - 클래스나 메소드에서 필요한 메타 데이터들이 저장된다.

### **Young 영역에 대한 GC**

![https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide13.png](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide13.png)

- 메모리에 객체가 생성되면 Eden 영역에 객체가 지정된다.
- Eden 영역이 꽉차면 minor garbage collection이 발생된다.

![https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide6.png](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide6.png)

- Eden 영역에서 GC가 한 번 발생한 후 살아남은 객체는 Survivor 영역 중 하나(S0)로 이동된다. 그리고 삭제대상이 되는 것들을 삭제하면 Eden 영역은 비워지게 된다.

![https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide8.png](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide8.png)

- 다음 마이너 GC에서도 똑같은 과정이 반복된다. 하지만, 이번에는 다른 Survivor영역(S1)으로 옮겨진다. 게다가 S0에 있던 객체들도 GC과정을 거쳐 살아남으면 age가 증가한 다음 S1으로 이동한다.
- 이 과정을 진행하고 나면 Eden 영역과 S0는 비워진 상태가 된다.
- 다음 마이너 GC에서도 똑같은 과정이 반복되지만 이번에는 S0로 옮겨지게 된다. 그래서 Eden 영역과 S1는 비워진 상태가 된다.
- 이 과정에서 보이듯이 Survivor 영역 중 하나는 반드시 비어 있는 상태로 남아 있어야 한다.

![https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide7.png](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/images/gcslides/Slide7.png)

- 마이너 GC를 한번 걸칠때마다 객체들을 나이가 증가하게 되고 특정 나이가 되면 Old영역으로 이동한다. (위 사진에서는 8이되면 이동한다) 이 과정을 promotion이라고 한다.
- Old영역에서는 메이저 GC가 일어나면서 객체들을 삭제하고 compact하는 과정을 거친다.

### Old **영역에 대한 GC**

- Old 영역은 기본적으로 데이터가 가득 차면 GC를 실행한다.
- GC 방식은 JDK 7을 기준으로 5가지 방식이 있다.
    - Serial GC
    - Parallel GC
    - Parallel Old GC(Parallel Compacting GC)
    - Concurrent Mark & Sweep GC(이하 CMS)
    - G1(Garbage First) GC
- 여기 명시된 다섯가지의 GC방식은 WAS나 자바 애플리케이션 수행 시 옵션을 지정하여 선택할 수 있다.

### Serial GC

- Young 영역과 Old 영역이 연속적으로 처리되며 하나의 cpu를 사용한다.
- Young 영역에서의 GC는 앞 절에서 설명한 방식을 사용한다.
- Old 영역의 GC는 mark-sweep-compact라는 알고리즘을 사용한다.
    1. 마크(표시) 단계 : Old 영역으로 이동된 객체들 중 살아 있는 객체를 식별한다.
    2. 스윕 단계 : Old 영역의 객체들을 훑는 작업을 수행하여 쓰레기 객체를 식별한다
    3. 컴팩션 단계 : 필요 없는 객체들을 지우고 살아 있는 객체들을 한 곳으로 모은다. 수행 이후에는 컴팩션된 영역과 비어 있는 영역으로 나뉜다.
- 시리얼 콜렉터는 일반적으로 클라이언트 종류의 장비에서 많이 사용된다. 다시 말하면, 대기 시간이 많아도 크게 문제되지 않는 시스템에서 사용된다는 의미이다.
- 시리얼 콜렉터를 명시적으로 지정하려면 자바 명령 옵션에 -XX:+UseSerialGC를 지정하면된다.

### Parallel GC

- throughput collector라고 알려져 있다.
- Serial GC와 기본적인 알고리즘은 같지만, Parallel GC는 GC를 처리하는 쓰레드가 여러개이다.
- 그렇기 때문에 Serial GC보다 빠르게 객체를 처리할 수 있다.
- Parallel GC는 메모리가 충분하고 코어의 개수가 많을 때 유리하다.
- 병렬 콜렉터를 명시적으로 지정하려면 자바 명령 옵션에 -XX:+ParallelGC를 지정하면된다.

### Parallel Old GC(Parallel Compacting GC)

- 병렬 콜렉터와 다른 점은 Old 영역 GC에 새로운 알고리즘을 사용한다는 것이다. 이 방식은 Mark-Summary-Compaction 단계를 거친다.
- 종합(Summary) 단계 : 이전에 GC를 수행하여 컴팩션된 영역에 살아 있는 객체의 위치를 조사하는 단계
    - 스윕 단계와의 차이
        - 스윕 단계는 단일 스레드가 Old영역 전체를 훑는다.
        - 종합 단계는 여러 스레드가 Old영역을 분리하여 훑는다. 또한 앞서 진행된 GC에서 컴팩션된 영역을 별도로 훑는다
- 병렬 콜렉터와 동일하게 여러 cpu를 사용하는 서버에 적합니다.
- 병렬 콤팩팅 콜렉터를 명시적으로 지정하려면 자바 명령 옵션에 -XX:+UseParallelOldGC를 지정하면된다.

### Concurrent Mark & Sweep GC(이하 CMS)

- low-latency collector라고 알려져 있다.
- 힙 메모리 영역의 크기가 클 때 적합하다.
- Young 영역에 대한 GC는 병렬 콜렉터와 동일하다
- Old 영역의 GC는 다음 단계를 거친다.
    1. 초기 표시(Initial Mark) 단계 : 클래스 로더에서 가장 가까운 객체 중 살아 있는 객체만 찾는 것으로 끝낸다. 따라서, 멈추는 시간은 매우 짧다.
    2. 컨커런트 표시(Concurrent Mark) 단계 : 서버 수행과 동시에 살아 있는 객체에 표시를 해 놓는 단계
    3. 재표시(Remark) 단계 : Concurrent Mark 단계에서 새로 추가되거나 참조가 끊긴 객체를 확인해서 다시 표시한다.
    4. 컨커런트 스윕 단계 (Concurrent Sweep) 단계 : 표시되어 있는 쓰레기를 정리하는 단계
- 장점
    - stop-the-world 시간이 매우 짧다. 모든 애플리케이션의 응답 속도가 매우 중요할 때 CMS GC를 사용한다.
- 단점
    - 다른 GC 방식보다 메모리와 CPU를 더 많이 사용한다.
    - Compaction 단계를 거치지 않는다.
- CMS 콤팩팅 콜렉터를 명시적으로 지정하려면 자바 명령 옵션에 -XX:+UseConcMarkSweepGC를 지정하면된다.

### G1(Garbage First) GC

![https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/images/slide9.png](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/images/slide9.png)

- 바둑판 모양으로 되어있거 각 바둑판의 사각형을 region이라고 한다.
- G1은 Young영역과 Old영역이 물리적으로 나뉘어 있지 않고, 각 구역의 크기는 모두 동일하다.
- 이 바둑판 모양의 구역이 각각 Eden, Survivor, Old 영역의 역할을 변경해 가면서 하고 Humongous라는 영역도 포함된다.
- Young GC
    - 몇개의 구역을 선정해서 Young영역으로 지정한 후 객체가 생성되면 이 구역에 데이터가 쌓인다.
    - 다른 콜렉터들과 마찬가지로 Young영역으로 할당된 구역에 데이터가 꽉차면 GC를 수행하고 살아있는 객체들만 Survivor구역으로 이동시킨다.
    - 이렇게 살아남은 객체들이 이동된 구역이 새로운 Survivor영역이 된다. Young GC가 발생하면 Survivor 영역에 계속 쌓는다.
    - 몇번의 에이징 작업을 통해 Old영역으로 승격된다.

[Old 영역 GC 단계](https://www.notion.so/ceb9049a6af64a6c84e144dd601c02cc)

> References
> * [https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)
> * [https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/index.html](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/index.html)
> * [https://d2.naver.com/helloworld/1329](https://d2.naver.com/helloworld/1329)
> * 도서 <자바 성능 튜닝 이야기>  이상민 저