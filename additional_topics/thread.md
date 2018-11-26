# Thread

Concurrency Programming을 위한 기법중 하나로 특정 작업을 동시에 혹은 백그라운드로 처리하기 위한 용도의 개념입니다.

OS X 또는 iOS 의 각 프로세스(어플리케이션)는 하나 또는 그 이상의 쓰레드로 구성됩니다. 이때 하나의 쓰레드는 프로그램 내에서의 하나의 실행 단위를 나타냅니다. 모든 어플리케이션은 메인 함수를 실행하는 싱글 쓰레드로 시작합니다. 어플리케이션은 특별한 역할의 코드를 실행할 추가적인 쓰레드들 역시 생성할 수 있습니다.

어플리케이션이 새로운 쓰레드를 생성했을 때, 그 쓰레드는 어플리케이션의 프로세스 공간에서 독립적인 개체가 됩니다. 각 쓰레드는 각각의 실행 스택 (execution stack) 을 가지며, 각각 커널에 의해 런타임에 스케줄됩니다. 쓰레드는 다른 쓰레드나 다른 프로세스와 통신할 수 있고, I/O 오퍼레이션을 수행할 수 있으며, 그 외 여러가지 것들을 수행 할 수 있습니다. 하나의 어플리케이션이 사용하는 모든 쓰레드들은 같은 가상 메모리 공간을 공유하고 프로세스 자체에 대한 엑세스 권한도 똑같이 가집니다.

Swift에서 Thread는 상속받아 구현하는 형태보다는 별도의 메소드(셀렉터)를 스레드 함수로 등록하는 형태로 만들어서 동작시킵니다. 이때 사용 할 수 있는 방법은 두 가지가 있습니다.

```swift
// 스레드를 만들어 바로 시작
Thread.detachNewThreadSelector("runLoop", toTarget: self, with: nil)

// 스레드를 만들고 시작
let thread = Thread(target: self, selector: "runLoop", object: nil)
thread.start()

// 실행 메소드
func runLoop() {
    // 취소되기 전까지 무한루프
    while Thread.current.isCancelled == false {
        print("From MyThreadingClass")

        // 1초간 휴식
        Thread.sleep(forTimeInterval: 1)
    }
}
```

위 두 가지 방법에서 모두 문자열로 “runLoop” 라는 셀렉터 이름을 넘겨주는데 이는 위 코드를 실행시키는 클래스에서 스레드로 돌릴 메소드 이름입니다. 따라서 이 이름은 유저가 임의로 설정하면 됩니다.

그러나 Thread는 low-level tool이기 때문에 관리하기가 매우 어렵습니다. 현재 시스템 로드 및 기본 하드웨어 따라 동적으로 어플리케이션의 최적 스레드 수가 변화할 수 있으므로 올바른 스레드 솔루션을 구현하는 것은 거의 불가능합니다. 또한 일반적으로 스레드와 함께 사용되는 동기화 메커니즘은 성능 향상을 보장하지 않고 소프트웨어 설계에 복잡성과 위험을 가중시킵니다.

때문에 OperationQueue및 GCD의 사용이 권장됩니다.

### Reference:
- http://seorenn.blogspot.com/2014/06/swift-nsthread.html
- https://developer.apple.com/documentation/foundation/thread
- https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html
