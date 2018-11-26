# Sequence and Collection


## Sequence

Sequence 프로토콜은 Array, Dictionary, Set과 같은 Collection 타입의 기반이 되는 프로토콜로, 값을 iterate(반복) 할 수 있는 동일한 타입의 일련의 값이다.
Sequence를 순회하는 가장 일반적인 방법은 for loop를 통해서 사용 할 수 있습니다.

```swift
for element in someSequence {
    doSomething(with: element)
}
```

Sequence 프로토콜을 구현하게되면 for loop를 통한 순회 뿐만아니라
forEach, map, filter, flatMap, prefix 등등 다양하고 유용한 함수를 사용 할 수 있습니다.

즉, 일련의 값에 대한 순차적 접근에 의존하는 공통 작업을 해야한다면 Sequence 위에 구현하는 것을 고려해야 합니다.

특정 타입이 Sequence 프로토콜을 conform 하는 것은 간단합니다.
makeIterator() 함수만 구현해주면 됩니다.

Sequence 프로토콜은 다음과 같이 선언되어 있습니다.

```swift
protocol Sequence {
    associatedtype Iterator: IteratorProtocol
    func makeIterator() -> Itertator
}
```

Sequence 프로토콜은 두 개의 요소로 구성됩니다. 우선, Iterator라는 associated type이 있습니다. 이 associated type은 IteratorProtocol을 준수(conform)하고 있습니다. 다른 하나는 Iterator를 만드는 makeIterator라는 함수이며, 앞서 선언한 Iterator 타입을 반환합니다.


## IteratorProtocol

IteratorProtocol은 Sequence와 매우 유사합니다. Element라는 associated type을 가지고 있으며, 이것은 요소를 추가하거나 이터레이트를 수행할 때 사용하는 타입을 나타냅니다. 그리고 next라는 함수가 있는데 이것은 다음(next) 요소를 반환합니다.

next() 함수는 호출시 다음 Element를 반환하고, Sequence가 고갈되면 nil를 반환합니다.

IteratorProtocol 프로토콜은 다음과 같이 선언되어 있습니다.

```swift
protcol IteratorProtocol {
    associatedtype Element
    mutating func next() -> Element?
}
```

위 코드에서 associatedtype으로 선언된 Element는 Iterator가 생성하는 값의 유형을 지정합니다.

```swift
extension Sequence where Iterator.Element == CustomType { ... }
```

위와 같은 코드를 많이 사용하게 되는데 Sequence 확장에서 제네릭 타입 제약시 Iterator.Element가 사용되는 이유입니다.

Iterator를 직접적으로 순회하는 경우는 매우 드뭅니다. 왜냐하면 for loop으로 순회하는 것이 일반적이 방법이기 때문입니다. (for loop도 내부에서는 다음과 같은 로직이 타고 있습니다.)

```swift
var iterator = someSequence.makeIterator()
// next 함수에서 nil이 반환되기 전까지 계속 순회함
while let element = iterator.next() {
    doSometing(with: element)
}
```

Iterator는 순회시 절대로 reversed 되거나 reset되지 않습니다. next() 함수에서 nil이 반환되지 않는다면 무한 루프에 빠질 수 있습니다.

피보나치 수열을 이러한 프로토콜들을 준수하게끔 만들 수 있습니다.

```swift
struct FibsIterator: IteratorProtocol {
    var state = (0, 1)
    var initial = 0
    let prefix: Int

    init(prefix: Int) {
        self.prefix = prefix
    }

    mutating func next() -> Int? {
        guard initial < prefix else {
            return nil
        }
        initial += 1
        let upcomingNumber = state.0
        state = (state.1, state.0 + state.1)
        return upcomingNumber
    }
}
```

유한한 스트림을 생성하는 FibIterator 구조체를 만들었으니 다음과 같이 순회 할 수 있습니다.

```swift
var fibIterator = FibsIterator(prefix: 5)
while let value = fibIterator.next() {
    print(value)
}
// 0
// 1
// 1
// 2
// 3
```

FibsIterator를 만들었기 때문에, FibsSequence도 쉽게 만들 수 있습니다.
위에서 언급한대로, makeIterator() 함수만 구현해주면 끝납니다.

```swift
struct FibsSequence: Sequence {
    typealias Iterator = FibsIterator // 타입 추론으로 생략 할 수 있음
    let prefix: Int

    func makeIterator() -> FibsIterator {
        return FibsIterator(prefix: prefix)
    }
}
```

Sequence 프로토콜을 conform 하는 커스텀 타입을 만들었기 때문에 Sequence 프로토콜의 많은 기능을 사용 할 수 있습니다.

```swift
for value in fibsSequence {
    print(value) // 0, 1, 1, 2, 3
}
fibsSequence.map { $0 + 1 } // [1, 2, 2, 3, 4]
fibsSequence.filter { $0 % 2 == 0 } // [2]
fibsSequence.reduce(0, +) // 7
```

## Collection

모든 Collection은 Sequence를 상속받아 구현되었으며, Collection은 Sequence가 지닌 두 가지 문제점을 해결했습니다. 우선, 모든 Collection은 유한(finite)합니다. 몇 개의 요소로 구성되어 있는지 항상 알 수 있다는 뜻입니다. 절대로 무한(infinite)해질 수 없으며, 원하는 만큼 이터레이트를 할 수 있습니다. Collection 프로토콜은 다음과 같이 작성되어 있습니다.

```swift
protocol Collection: Sequence {
  associatedtype Index: Comparable
  var startIndex: Index
  var endIndex: Index
  subscript(position: Index) -> Element { get }
  func index(after i: Index) -> Index
}
```
