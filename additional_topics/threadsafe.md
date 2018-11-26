# 프로세스 동기화

## Critical Section (임계영역)

동일한 자원을 동시에 접근하는 작업을 실행하는 코드 영역을 임계 영역이라고 합니다. 프로세스들이 이러한 임계영역을 함께 사용할 수 있는 프로토콜을 설계해야 합니다.

## 해결을 위한 기본 조건

### Mutual Exclusion

### Progress

### Bounded Waiting

 


Swift에서 굳이 atomic을 빼도 되는 것에 대한 이유를 조심스럽게 value type의 적극적인 사용이라고 추측해봅니다.
Objective-C의 경우 거의 모든 것이 object로 다루어지고 이것이 reference type이기 때문에 thread를 사용하는 순간 어떻게든 문제를 일으킬 수 밖에 없는 여지를 가지고 있죠. 그러다보니 atomic에 대한 요구가 강했던 것 같습니다.
반대로 Swift의 경우 많은 부분 struct같은 value type으로 이루어지기 때문에 별도의 다른 thread에서 동일한 value를 굳이 접근하지 않고도 많은 부분들이 해결되죠.
예를 들어 특정 thread에서 사용하기 위해서 struct를 가져가는 순간 변경되면 바로 copy되기 때문에 원본과 분리된 데이터로 다루게 되니 sync를 맞출 일이 없어지겠죠.
하지만 여전이 class같은 instance type을 사용하면 해당 문제를 벗어날 수 없지만요.
그리고 사실 property를 atomic으로만 했다고 해서 concurrent programming 상에서 벌어지는 수많은 이슈들을 감당할 수는 없죠.

Thread-safe in Swift #
Swift에서 무척이나 흥미로운 점 중 하나는 threading 과 관련하여 언어단에서는 어떠한 처리도 없다는 점 입니다. Objective-C 에서는 @synchronized 또는 atomic 속성을 통해 thread-safe 한 변수를 선언할 수 있었는데 가장 최근에 나온 swift 에서 이런 속성들이 빠져있습니다.

하지만 플랫폼에서 API를 통해 thread-safe 한 환경을 만들 수 있도록 기능을 제공하고 있습니다.

Thread-safe 한 singleton 생성 #
Objective-C 와 Swift 1.1 버전까지 가장 전통적인 singleton 생성 방법은 dispatch_once를 사용한 아래와 같은 방법입니다.

class SingletonA {
    class var sharedInstance: SingletonA {
        struct Static {
            static var onceToken: dispatch_once_t = 0
            static var instance: SingletonA? = nil
        }

        dispatch_once(&Static.onceToken) {
            Static.instance = SingletonA()
        }

        return Static.instance!
    }
}
Singleton 객체가 생성되기 전 두 개의 thread에서 동시에 접근할 경우 이 객체가 두번 생성이 되는 문제가 발생할 수 있는데 dispatch_once를 사용함으로써 하나의 thread에서만 접근이 되도록 보장하는 방식입니다.

하지만 Swift 1.2 버전부터는 애플에서 공식적으로 아래와 같은 방식을 권장하고 있습니다.

class SingletonA {
    static let sharedInstance = SingletonA()
}
이와 같이 선언하면 swift에서 자체적으로 여러개의 thread 에서 동시에 접근하더라도 단 한번만 수행하여 생성하도록 thread-safe 하게 설계되어있습니다.

https://developer.apple.com/library/content/documentation/Swift/Conceptual/BuildingCocoaApps/AdoptingCocoaDesignPatterns.html

Thread-safe 변수 #
Objective-C 에서 thread-safe 를 보장해주던 atomic 속성이 사라졌지만 swift 에서는 다양한 API 를 제공해주고 있습니다.

pthread : pthread_mutex_t 는 recursive lock으로 구성할 수 있는 방식입니다.
Reader/writer lock : pthread_rwlock_t 는 말그대로 reader/writer 관련 lock API 입니다.
dispatch queue : dispatch_queue_t 는 concurrent queue를 구성하고 barrier block 을 사용할 수 있습니다. 또한 비동기 lock을 지원합니다.
NSOperationQueue : block lock 으로 사용할 수 있으며 dispatch queue와 마찬가지로 비동기 실행을 지원합니다.
NSLock : NSlock은 Objective-C 클래스로 lock을 지원하며 쉽게 사용 가능하고 비교적 빠르게 수행됩니다.
OSSpinLock : OSSpinLock은 lock 이 자주 일어나는 곳에서 사용하기 좋은 방식입니다.
Swift 에서는 @synchronized 나 atomic 속성이 더이상 없지만 GCD, dispatch_queue_t 또는 관련 API를 제공하여 lock 관련 환경을 최적화 할 수 있도록 하고 있습니다.
