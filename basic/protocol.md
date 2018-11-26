# Protocol

프로토콜은 특정 기능 수행에 필수적인 요수를 청의한 청사진(blueprint)입니다. 프로토콜은 인터페이스로 최소한으로 가져야 할 속성이나 메서드를 정의합니다. 구현은 하지 않고, 정의만 합니다.


프로토콜을 만족시키는 타입을 프로토콜을 따른다(conform)고 말합니다. 프로토콜에 필수 구현을 추가하거나 추가적인 기능을 더하기 위해 프로토콜을 확장(extend)하는 것이 가능합니다. 프로토콜은 다중으로 정의하는게 가능합니다. 한 클래스가 여러 프로토콜을 요구받게 만들 수 있습니다. 콤마를 이용해 구분하면 됩니다.

프로토콜 정의와 사용은 다음과 같이 쓸 수 있습니다.

```swift
protocol SomeProtocol {
    func someFunction() -> Int
    var someProperty: String { get set }
    optional func anotherFunction()
}
class SomeClass: SomeSuperclass, FirstProtocol, AnotherProtocol {
    // class definition goes here
}
```

객체간의 (비동기) 소통을 위한 delegation 등을 위해 프로토콜이 사용될 수 있습니다.

기본적으로 Swift Standard Library 이미 구현되어있는 기본 프로토콜 들이 존재합니다.

  - [Encoding, Decoding, and Serialization](./codable.md)
  - [Equality and Ordering](./equatable.md)
  - [Hash for Sets and Dictionaries](./hashable.md)
  - [Sequence and Collection](./sequence.md)


## Protocol Default Implementation

- Protocol + Extension = Protocol Extension

이러한 프로토콜은 메소드와 속성과 같이 확장(extension)이 가능합니다. 이미 있는 여러 클래스에 공통된 기능을 구현하여 함께 처리할 수도 있습니다. 이를 Protocol Default Implementation 가능하다고 합니다. (Protocol + Extension = Protocol Extension)

프로토콜의 기능을 익스텐션하여 미리 구현하는 것을 프로토콜 초기 구현이라고 합니다. 이를 통해 특정 타입이 할 일 지정 + 구현을 한번에 할 수 있습니다.

```swift
// 코드 중복이 발생 하는 상황

struct Bird {
	func move() {
		print("이동하다")
	}
	func fly() {
		print("날다")
	}
}

struct Airplane {
	func move() {
		print("이동하다")
	}
	func fly() {
		print("날다")
	}
}

// Protocol Extension으로 코드 중복 제거

protocol Movable {}
extension Movable {
	func move() {
		print("이동하다")
	}
}

protocol Flyable {}
extension Flyable {
	func fly() {
		print("날다")
	}
}

struct Bird : Movable, Flyable { }
struct Airplane: Movable, Flyable { }
```

## Protocol과 Interface (Java) 차이

Interface:

- 파라미터 초기값 설정 가능

- 선언된 메소드 모두 구현해야 함

- 정적 멤버 선언 불가

Protocol:

- 파라미터 초기값 설정 불가 (Protocol Extension으로 가능)

- Optional 통해서 선택적 구현

- 정적 멤버 선언 가능

- 값 타입에도 적용될 수 있기 때문에 mutating keyword를 명시적으로 씀


### Reference

- https://jusung.gitbook.io/the-swift-language-guide/untitled-17
