# Performance Improvement

<img src="https://cdn-images-1.medium.com/max/1600/1*FhpnyHS2oSfqijAz0jR4Lg.jpeg" width = 400>

당연히 퍼포먼스의 개선은 하드웨어적인 측면과 소프트웨어적인 측면을 이야기할 수 있습니다. 하드웨어적으로는 더 좋은 CPU, 더 큰 RAM, 더 좋은 GPU 등등 을 이야기할 수 있습니다. 그러나 이러한 하드웨어적인 측면은 배제하고 소프트웨어적으로 퍼포먼스 향상을 이야기하고자 합니다.

소프트웨어적으로 퍼포먼스를 향상시키기 위해서는 **퍼포먼스 오버헤드를 줄이고 런 타임이 아닌 컴파일 시점에서 할당 및 위치 파악을 하는 것** 이 주요한 Optimization 기법이라고 말할 수 있습니다. 이러한 퍼포먼스 향상을 위한 Optimization 요소로 3가지를 꼽습니다.

1. [Memory Allocation](./allocation.md): Stack or Heap

2. [Reference Counting](./arc.md): No or Yes

3. [Method Dispatch](./method_dispatch.md): Static or Dynamic

각각의 항목에 대해서는 해당 문서를 참조하시면 더 자세한 설명을 보실 수 있습니다.


## 1. Memory Allocation: Stack!

주로 메모리 할당은 Stack과 Heap을 이용하여 변수와 상수들을 저장하곤 합니다. Heap은 스택과 같이 자동적으로 객체에 메모리를 할당 및 반환할 수 없습니다. 외부적으로 이러한 일을 해야합니다. (개발자가 직접하거나 Swift처럼 ARC, 자바의 GC 등이 이러한 일을 합니다.)

따라서 빈 곳 찾기, 메모리 할당, 참조 추적, 메모리 반환 관리 등의 복잡한 과정이 필요합니다. 더불어 과정이 Thread Safe해야 함으로 lock 등의 동기화 동작으로 인해 성능이 저하됩니다. (performance overhead)

반면 Stack 할당은 단순히 스택포인터 변수 값만 바꿔주는 정도입니다. 또한 Heap에서의 메모리 할당 /반환이 동적으로 프로그램 런타임 동안에 일어나게 되므로 스택에 비해 느릴 수 밖에 없습니다. 이것이 value type이 reference type보다 빠른 이유입니다. 가능하면 Heap보다는 Stack을 사용하는 것이 퍼포먼스 향상에 일반적으로!(항상은 아닙니다.) 도움이 됩니다.

## 2. Reference Counting: No!

ARC는 GC의 대표적인 2가지 부작용을 경감합니다.

-  1. GC가 언제 실행될 지 코드 상에서 알 수가 없습니다.

-  2. GC로 인한 퍼포먼스 저하  

물론 ARC는 GC와는 다르게 background process가 아니고, 런타임에 비동기적으로 객체를 메모리에서 반환한다는 점에서 일반적으로 GC보다 퍼포먼스 측면에서 efficient합니다. 하지만 매번 참조할 때 마다 참조값을 검사해야 하므로 많은 수의 단위 객체를 사용하게 되면 그 검사에 대한 부하가 커지고, 참조하는 단위 객체 사이에 서로 참조하게 되면 순환 참조 오류로 인해 잘못된 참조 파괴가 생기거나 또는 단위 객체가 고아가 될 수 있다는 점에서 역시 퍼포먼스 오버헤드가 존재한다고 말할 수 있습니다. 때문에 참조 객체의 수를 줄이는 것이 퍼포먼스 향상에 일반적으로!(항상은 아닙니다.) 도움이 됩니다.

## 3. Method Dispatch: Static!

Method Dispatch 란 쉽게 말해 준비 중인 메서드를 호출하여 실행하는 것으로, Dispatch의 목표는 프로그램에서 특정 메서드 호출의 실행 코드를 찾을 수 있는 메모리 위치를 CPU에 알려주는 것입니다. 이러한 디스패치가 Static하게 컴파일 시에 미리 결정되어 런 타임 시 찾는 과정 없이 그 주소로 바로 점프할 수도 있고 Dynamic하게 런타임 시에 메소드 주소를 찾아 그 주소로 점프할 수 도 있습니다. 당연히 런타임 시 메소드 주소를 찾는 과정이 없는 Static한 메소드 디스패치 방법이 퍼포먼스 향상에 일반적으로!(항상은 아닙니다.) 도움이 됩니다.

## 구체적 Tips

### 1. Structs vs Classes (When to use what)

Struct 객체는 스택에 할당되고 클래스 객체는 힙에 할당되므로 Struct가 퍼포먼스적으로 더 빠르다고 할 수 있습니다. 일반적으로 class는 성능에 상관없이 Reference semantics과 상속이 필요한 경우 등에 사용합니다. 이러한 class와 struct 안에서도 다소 차이가 존재합니다.

#### class

class보다 final class를 사용하는 것이 고성능 입니다.

class는 Reference type이므로 Memory allocation으로 Heap을 사용하고 Reference Counting을 합니다. 또한 Dynamic한 메소드 디스패치 방법을 사용합니다. 반면 final class의 경우 동일하게 Memory allocation으로 Heap을 사용하고 Reference Counting을 하지만, Static하게 메소드 디스패치한다는 차이점이 존재합니다.

프로젝트 내부 어디에서도 상속되지 않는다고 확신할 때 final 키워드를 사용하시면 됩니다.

#### struct

struct 자체가 참조 타입이 아닌 값 타입이므로 고성능 입니다.
(Memory allocation = stack / Method dispatch = dynamic)

그러나 내부적으로 참조 타입의 변수나 상수를 갖는 struct는 Reference Counting을 해야하므로 성능이 저하됩니다. 따라서 값의 제한이 가능하면 enum등의 value type으로 변경하고 다수의 class들을 하나로 합쳐 성능을 개선할 수 있습니다.

### 2. Use Protocol Oriented Programming

상속은 dynamic dispatch만을 사용하는 반면 protocol은 dynamic 또는 static dispatch를 유동적으로 사용할 수 있습니다. 더욱이 protocol은 value type인 struct에도 적용이 가능합니다. 즉, value semantics에서 다형성을 지원합니다.

아래의 예시는 다형성을 위한 프로토콜 requirement로서 Dynamic하게 dispatch합니다.

```swift
protocol Movable {
  func walk()
}
```

그러나 direct call을 위한 protocol extension은 아래와 같이 Static하게 dispatch 합니다.

```swift
extension Movable {
    func crawl() {
       print(“Default crawling”)
    }
}
```

아래는 평범한 케이스입니다.


```swift
struct Animal: Movable {
    func walk() {
        print("Animal is walking proudly")
    }
    func crawl() {
        print("Animal is crawling silently")
    }
}
let raccoon = Animal()
raccoon.walk() // Animal is walking proudly
raccoon.crawl() // Animal is crawling silently
```

그러나 아래의 특수한 예제에서 dynamic과 static의 차이를 확인할 수 있습니다.

```swift
let wolf: Movable = Animal()
wolf.walk() // Animal is walking proudly
wolf.crawl() // Default crawling
```

wolf를 Movable 프로토콜을 따르도록 했고 Swift 컴파일러는 이 변수를 existential container라는 곳으로 이동시킵니다. 타입을 명시하는 대신, 따르는 프로토콜만 알려주었으므로, static dispatch를 따르는 existential container내에서 생성되면서, 이러한 결과가 발생한 것입니다.  만일 dynamic하게 디스패치함으로서 원하는 결과를 얻고 싶다면 아래와 같이 프로토콜 요구사항을 추가하면 됩니다.

```swift
protocol Movable {
  func walk()
  func crawl()
}
```

### 3. Use Generics

제너릭의 사용은 Dynamic dispatch 없이 다형성의 파워를 갖도록 도와줍니다.

```swift
func power(_p: ProtocolType) {
 // this method will be dynamically dispatched
}

power(SomeTypeWhichConformsToProtocolType)

func power<T: ProtocolType>(_p: T)
 // this method will be statically dispatched
}

power(SomeTypeWhichConformsToProtocolType)
```

### 4. Use private and fileprivate

private나 fileprivate 키워드를 쓰면 file 선언의 범위를 제한할 수 있습니다. 이것은 컴파일러가 자동적으로 final 키워드를 추론할 수 있게 해주며, 간접적인 콜을 제거시켜 줍니다.

### Reference:

  - https://blog.usejournal.com/swift-performance-tips-55dc5688808b

- https://academy.realm.io/kr/posts/letswift-swift-performance/
