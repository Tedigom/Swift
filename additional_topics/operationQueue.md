# OperationQueue

Concurrency Programming을 위한 기법중 하나로 특정 작업을 동시에 혹은 백그라운드로 처리하기 위한 용도의 개념입니다.

작업 큐(Operation Queue) 라는 방식은 비동기식으로 수행해야 하는 작업을 캡슐화하는 개체 지향 방식으로 병렬 프로그래밍을 하기 위한 개념입니다. 큐에다 단일 작업을 필요한 것 만큼 넣어놓으면 해당 작업을 알아서 꺼내어 별도의 스레드에서 실행시켜줍니다. 큐(Queue)라고 해서 한번에 하나씩 처리가 되는 것이 아니라, 개별 작업을 개별 스레드에서 가능한 만큼 병렬로 한번에 실행합니다. 멀티스레딩의 스레드 풀 관리의 비효율성에 대한 좋은 대안입니다.

## Operation
이 클래스는 오퍼레이션큐에 넣기 위한 단위 클래스입니다. 즉 모든 작업은 이 Operation 타입의 클래스로 구현되어야 합니다.

예제의 main() 이라는 오바라이드 메소드가 실제로 병렬로 작동할 코드입니다.
```swift
class MyOperation: Operation {
    var index: Int?
    override func main() {
        print("From My Operation \(self.index)")
    }
    init(index: Int) {
        super.init()
        self.index = index
    }
}
```

모든 operation 객체는 다음과 같은 주요 기능을 지원합니다.

- operation객체간의 그래프 기반 종속성 설정 지원. 이러한 종속성은 종속 된 모든 작업이 실행을 완료할 때까지 주어진 operation이 실행되는 것을 방지합니다.

- operation의 main task가 완료된 후에 실행되는 optional completion블록을 지원합니다.(OS X v10.6이상에만 해당)

- KVO알림을 사용하여 operation의 실행 상태 변경 모니터링을 지원합니다.

- operation 우선 순위 지정을 지원하므로, 상대적인 실행 순서에 영향을 줍니다.

- 실행 중 조작을 정지 할 수 있는 canceling semantics기능을 지원합니다.

operation은 앱의 동시성 수준을 향상시킬 수 있도록 설계되었습니다. operation은 앱의 동작을 단순한 개별 청크로 구성하고, 캡슐화하는 좋은 방법이기도 합니다. 앱의 main쓰레드에서 코드를 실행하는 대신 하나 이상의 operation 객체를 큐에 제출하고, 하나 이상의 개별 쓰레드에서 해당 작업을 비동기적으로 수행 할 수 있습니다.

방식은 다르지만 Operation역시 스레드에서 동작합니다. 따라서 sleep(forTimeInterval:)과 같이 Thread에서 제공되는 기능을 활용할 수 있습니다.

또한 Operation클래스는 다음 Key-path에 대해 key-value observing(KVO)를 준수합니다. 원하는 대로 이러한 속성을 관찰하여 응용 프로그램의 다른 부분을 제어할 수 있습니다.
- isCancelled

- isConcurrent

- isExecuting

- isFinished

- isReady

- dependencies

- queuePriority

- completionBlock


## OperationQueue
이러한 Operation 객체들을 아래와 같이 큐를 만들어 병렬처리를 할 수 있습니다. 해당 클래스의 인스턴스를 만들면 병렬작업이 시작됩니다.
```swift
class MyWork {
    let queue = OperationQueue()
    init() {
        self.queue.addOperation(MyOperation(index: 0))
        self.queue.addOperation(MyOperation(index: 1))
        self.queue.addOperation(MyOperation(index: 2))
        self.queue.addOperation(MyOperation(index: 3))
        self.queue.addOperation(MyOperation(index: 4))
    }
}
```
인스턴스 생성시 콘솔에 찍히는 테스트 결과입니다.
```swift
FFFFFFrrrrrroooooommmmmm      MMMMMMyyyyyy      OOOOOOppppppeeeeeerrrrrraaaaaattttttiiiiiioooooonnnnnn      302451
```

**addOperation(operation: Operation!)**

앞서 본 예제에서 볼 수 있듯이 실제 오퍼레이션(Operation을 상속받은 클래스의 인스턴스)을 큐에 추가하는 담당입니다. 추가하자 마자 바로 작업이 실행됩니다.


**cancelAllOperations()**

이름처럼 모든 오퍼레이션을 취소시키는 역활입니다. 하지만 이것 만으로 반드시 모든 오퍼레이션이 취소되는 것은 아닙니다. Operation 자체에서도 isCancelled 프로퍼티를 체크해서 종료하도록 처리를 해 놔야만 실제로 작업이 종료됩니다.

이 외에도 여러 method 및 property가 존재합니다.



### Reference:
- http://seorenn.blogspot.com/2014/06/swift-nsoperationqueue.html
- https://developer.apple.com/documentation/foundation/operationqueue
- https://zeddios.tistory.com/510
