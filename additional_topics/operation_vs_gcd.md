# OperationQueue vs DispatchQueue

먼저 공통적으로 쓰레드 생성 및 관리에 대해 걱정 할 필요 없이 실제로 수행하려는 작업에 집중 할 수 있다는 장점이 있습니다. 쓰레드를 사용하면 수행 할 작업과 쓰레드 자체의 생성 및 관리를 위한 코드를 모두 작성해야합니다. 그러나 OperationQueue와 DispatchQueue는 시스템이 대신 쓰레드 생성 및 관리를 처리합니다. 이점은 시스템이 단일 앱보다 훨씬 효율적으로 쓰레드를 관리 할 수 있다는 것입니다. 시스템은 사용가능한 자원 및 현재 시스템 조건에 따라 동적으로 쓰레드 수를 조절 할 수 있습니다. 또한 시스템은 일반적으로 쓰레드를 직접 작성 한 경우보다 빨리 task를 실행 할 수 있습니다.

차이점은 OperationQueue는 Objective-C level API이고, GCD는 C level API라는 것 입니다. 때문에 OperationQueue는 비동기식으로 수행해야 하는 작업을 class와 같이 캡슐화하는 객체 지향 방식으로 병렬 프로그래밍이 가능합니다.

더불어 Operation과 OperationQueue는 GCD에 비해 추가적인 기능을 제공하며 여러 operation에 의존성을 부여할 수도 있습니다. 뿐만 아니라 재사용도 가능하며 취소 혹은 일시정지와 같은 기능도 가능합니다. 또한 Operation은 KVO 기술을 완벽하게 사용할 수 있습니다. 그래서 Operation이 실행되기 시작하면 NotificationCenter를 통해 상태 변화에 대한 노티를 받을 수 있습니다. 또한 메모리 제한적인 환경에서 DispatchQueue는 돌아가는 작업의 수를 시스템에서 모두 관리하기 때문에 제한할 수 없지만, OperationQueue는 제한을 걸 수 있습니다.

그러나 OperationQueue는 GCD dispatch queues에 비해 오버헤드가 조금 더 많은 백그라운드 스레드를 사용합니다.

요약하자면, OperationQueue는 취소해야 하거나 복잡한 종속성이 있는 장기 실행 작업에 더 적합합니다. GCD dispatch queues은 성능 및 메모리 오버헤드를 최소화해야 하는 짧은 작업에 적합합니다.

>작업이 그리 복잡하지 않고 최적의 CPU 성능이 필요한 경우 GCD 선호

>작업이 복잡하고 블록 및 종속성 관리를 취소하거나 일시 중단해야 하는 경우 OperationQueue 선호

### Reference:
- https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html
