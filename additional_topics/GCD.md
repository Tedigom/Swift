# GCD (Grand Central Dispatch)

Concurrency Programming을 위한 기법중 하나로 특정 작업을 동시에 혹은 백그라운드로 처리하기 위한 용도의 개념입니다. Grand Central Dispatch (GCD) dispatch queues는 task 수행을 위한 강력한 도구입니다. dispatch queues를 사용하면 호출자(caller)와 관련하여 비동기/동기식으로 임의의 코드 블록을 수행 할 수 있습니다. dispatch queues를 사용하여 별도의 쓰레드에서 수행하는 거의 모든 task를 수행 할 수 있습니다. dispatch queues의 장점은 사용하기 쉽고, 효율적이라는 것입니다. GCD는 자동으로 일부 dispatch queues를 제공하지만, 특정 용도로 작성 할 수 있는 다른 큐를 제공합니다. 다음은 앱에서 사용 할 수 있는 dispatch queues타입과 사용방법입니다.

### Dispatch queues type

#### Serial
<img src="http://jullykim.cafe24.com/wp-content/uploads/2017/04/Serial-Queue-Swift.png" width = 400>

- Serial Queue는 Queue에 추가된 작업을 순서대로 한 번에 하나만 수행합니다.
- Serial Queue는 일반적으로 리소스의 동기화를 위해 사용됩니다.
- Serial Queue는 생성 개수에 제한이 없습니다. 여러 개의 Serial Queue를 동시에 실행하면 각각의 Serial Queue는 한 번에 하나의 작업을 수행합니다. 하지만 각각의 Queue는 Concurrent하게 동작합니다.



> **main queue**
  - 메인 쓰레드에서 작업을 수행하는 Serial queue
  - 일반적으로 Main Queue는 작업의 동기화를 수행하는 공간(대표적으로 UI 업데이트)으로 활용됩니다.
```swift
DispatchQueue.main.sync {
  // code
}
```
> **custom**
```swift
let customQueue = DispatchQueue(label: "custom")
```  

  main dispatch queue는 앱의 main 쓰레드에서 task를 실행하는, 전역적으로 사용 가능한 serial queue입니다. 이 큐는 앱의 실행루프와 함께 작동하여 큐에 있는 task의 실행을 실행루프에 연결된 다른 이벤트 소스의 실행과 얽힙니다. 앱의 main쓰레드에서 실행되므로, main queue는 종종 앱의 주요 동기화 지점으로 사용됩니다.

#### Concurrent
<img src="https://koenig-media.raywenderlich.com/uploads/2014/09/Concurrent-Queue-Swift.png" width = 400>

- Concurrent Queue는 Queue에 추가된 작업을 concurrent하게 수행합니다.
- 이 때, 동시에 수행되는 작업의 정확한 개수는 시스템의 상황에 따라 달라집니다.
- 시스템은 Global Concurrent Queue를 미리 정의하였습니다.
- 이 정의된 Queue는 우선순위가 서로 다른 Queue이며, DispatchQueue를 생성할 때 QoS(Quality of Service)를 통해 결정됩니다.

> **global queue**
QoS로 작업 중요도를 결정합니다.

  - User-Interactive
  : highest 우선순위, ui를 업데이트하거나 즉시 실행되어야 하는 코드들을 넣는다. Main 쓰레드에서 동작합니다.

  - User-Initiated
  : high 우선순위, user에 의해 실행되지만, 메인 쓰레드에서 동작할 필요가 없는 블럭을 실행한다. 즉각적인 결과를 필요로 할 때 사용합니다.


  - Utility
  :  low 우선순위, 다운로드와 같은 오래 걸리는 작업을 처리하기에 적합합니다.

  - Background
  : background 우선순위, 사용자의 눈에 보이지 않는 작업을 수행하기에 적합하다.

```swift
DispatchQueue.global.async {
  // code
}
```

> **custom**
```swift
let customQueue = DispatchQueue(label: "custom", attributes: .concurrent)
```  

#### Serial Queue와 Concurrent Queue의 공통점과 차이점

**공통점**

a. Serial Queue와 Concurrent Queue 모두 Concurrent하게 돌아간다.

- Concurrent Queue가 Concurrent(병렬적)하게 돌아간다는 것은 쉽게 이해할 수 있지만, Serial Queue가 Concurrent하게 돌아간다는 것은 다소 이해가 어려울 수 있습니다.  Queue 앞의 접두어로 들어가는 Serial과 Concurrent는  Queue가 tasks를 수행하는 방식을 의미하는 것이지,  Queue가 어떻게 돌아가는지를 의미하는 것이 아닙니다. Dispatch Queue는 코드가 모두 Concurrent하게 돌아가도록 하고, tasks가 sync/async하게 동작할지 여부만 선택할 수 있습니다.

b. Serial Queue와 Concurrent Queue 모두 sync/async 코드 동작을 지원하고, sync로 수행할 경우 Main Thread가 작업 수행을 기다린다.

c. Serial Queue와 Concurrent Queue 모두 동일한 쓰레드에서 동작하는 것을 보장하지 않는다.

- Dispatch Queue에서 동일한 쓰레드에서 동작하도록 고안된 것은 Main Queue밖에 없습니다.

d. Serial Queue와 Concurrent Queue 모두 하드웨어적인 parallel을 보장하지 않는다.

- 하드웨어적으로 코드가 병렬적으로 돌아간다는 것은 하나의 프로세스 내에서 여러 개의 쓰레드가 돌아가는 것을 의미합니다. 하지만, GCD에 할당된 tasks가 몇 개의 쓰레드에서 돌아갈 것인지는 시스템의 환경이 결정합니다. 즉, 시스템의 환경에 따라 여러 개의 쓰레드에서 코드가 동작할 수도 있지만, 그렇지 않은 경우도 발생할 수 있다는 것입니다.

**차이점**

a. Serial Queue는 한 번에 하나의 task만 하지만, Concurrent Queue는 동시에 여러 tasks를 수행한다.

- Serial Queue는 항상 한 가지 task를 하고 있다는 것이 보장됩니다. 여기서 task의 단위는 하나의 코드 블럭(closure) 혹은 DispatchWorkItem를 의미합니다. 하지만 Concurrent Queue는 동시에 여러 개의 tasks를 수행합니다.

b. Serial Queue는 작업의 동기화에 주로 사용되고, Concurrent Queue는 작업의 수행에 주로 사용된다.

- Serial Queue가 하나의 task만 수행하는 것은 작업의 수행 순서가 보장된 것이기 때문에, 여러 작업들의 동기화를 수행할 때 사용할 수 있습니다. Concurrent Queue는 자원을 병렬적으로 사용할 수 있도록 돕기 때문에 주로 일반적인 작업의 수행에 활용됩니다.


### Sync vs Async
동기(sync), 비동기(async) 방식 중 어떻게 실행하느냐에 따라 다음 처럼 4가지 조합이 나올수 있습니다.

- Serial - Sync
- Serial - Async
- Concurrent - Sync
- Concurrent - Async

> sync: 큐에 작업을 추가한 후, 끝날 때까지 기다립니다. (block)

!주의!
> main.sync는 사용 불가능합니다. 메인 큐에서 돌아가는 UI 및 메인 함수와 기본 AppDelegate함수들을 block하면 안되기 때문입니다.

1) Sync-Serial/Concurrent

```swift
DispatchQueue.global().sync { print("1") }
print("2")
DispatchQueue.global().sync { print("3") }
print("4")
DispatchQueue(label:"SerialQueue").sync { print("5") }
print("6")

-------
1
2
3
4
5
6
```
Sync는 큐에 추가한 작업이 끝날때까지 기다렸다가 다음 코드를 수행하므로 동일한 순서를 보장합니다.

> async: 큐에 작업을 추가한 후, 기다리지 않고 타 작업을 수행합니다. (non-block) ~~나몰라라 내팽개침~~

1) Async-Serial

```swift
DispatchQueue(label:"SerialQueue").async { print("1") }
print("2")
DispatchQueue(label:"SerialQueue").async { print("3") }
print("4")
DispatchQueue(label:"SerialQueue").async { print("5") }
print("6")

-------
2
1
4
3
6
5
```
위에서 언급한 결과값은 실행때마다 변하지만, (1,3,5) 와 (2,4,6)의 순서를 고정입니다. 두 그룹의 출력 순서만 달라지게 됩니다. 214653,214365…


2) Async-Concurrent

```swift
DispatchQueue.global().async { print("1") }
print("2")
DispatchQueue.global().async { print("3") }
print("4")
DispatchQueue.global().async { print("5") }
print("6")

-------
2
1
4
3
6
5
```

 여기에서 2,4,6의 출력순서만 보장되고 그 이외의 출력순서는 보장되지 않습니다. 병렬 큐에 멀티 스레드 형식으로 동작합니다.

> asyncAfter를 통해 특정 코드의 실행을 미룰 수 있습니다.

### Queue-Related Technologies

dispatch queues외에도 Grand Central Dispatch를 사용하여 코드를 관리하는 데 도움이 되는 몇가지 기술을 제공합니다.

**Dispatch groups :**

Dispatch groups은 완료(completion)을 위해 블록 객체 집합을 모니터링 하는 방법입니다. 그룹은 필요에 따라 동기적/비동기적으로 블록을 모니터링 할 수 있습니다. 그룹은 다른 task의 완료 여부에 따라 코드에 유용한 동기화 메커니즘을 제공합니다.

**Dispatch semaphores :**

Dispatch semaphores는 전통적인 세마포어와 유사하지만, 일반적으로 더 효율적입니다. 세마포어를 사용 할 수 없기 때문에 호출 쓰레드(calling thread)를 차단해야하는 경우에만 세마포어가 커널로 호출됩니다. 세마포어를 사용 할 수 있으면, 커널 호출이 수행되지 않습니다.

**Dispatch sources :**

dispatch source는 특정 타입 시스템 이벤트에 대한 응답(response)으로 notifications을 생성합니다. Dispatch sources를 사용하여 프로세스 notifications, 신호(signal) 및 descriptor events와 같은 이벤트를 모니터링 할 수 있습니다. 이벤트가 발생하면 Dispatch sources는 처리를 위해 지정된 dispatch queue에 비동기적으로 task코드를 제출(submit)합니다.

**Dispatch WorkItem :**

수행 할 수 있는 task를 캡슐화 합니다. 이 WorkItem은 Dispatch queue 및 Dispatch Group에 dispatch 할 수 있습니다.Dispatch WorkItem은 DispatchSource이벤트, 등록 또는 취소 handler로 설정 할 수도 있습니다.

```swift
let workItem = DispatchWorkItem {
    for i in 1...5 {
        print("Dispatch WorkItem \(i)")
    }
}
DispatchQueue.global().async(execute: workItem)
```

**dispatch_once**

싱글톤 객체의 Thread Safe를 위해 사용합니다.

```swift
class SingletonC {

	class var sharedInstance: SingletonC {
		struct Static {
			static var onceToken: dispatch_once_t = 0
			static var instance: SingletonC? = nil
		}
		dispatch_once(&Static.onceToken) {
			Static.instance = SingletonC()
		}
		return Static.instance!
	}
}
```

다만, 최근에는 Class가 Static variable을 지원하면서 lazy initializer를 통해 생성되는 글로벌 변수에 대해서는 dispatch_once를 통해 독립적으로 생성됩니다. 

### Reference:
- https://zeddios.tistory.com/513?category=682195
- https://hcn1519.github.io/articles/2018-07/gcddispatchqueue
