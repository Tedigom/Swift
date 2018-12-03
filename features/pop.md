# POP (Protocol Oriented Programming)

OOP (객체지향 프로그래밍)에 기반을 둔 대부분의 언어는 대부분 클래스의 상속을 상요해 타입에 공통된 기능을 구현합니다. 그러나 스위프트는 클래스가 아닌 Struct로 대부분 기본 타입이 구현되어 있습니다. 상속이 되지 않는 Struct로 다양한 공통 기능을 가지게 된 해답은 프로토콜과 익스텐션에 있습니다.

[Protocol에 대한 설명](../basic/protocol.md)

## POP를 사용하는 이유

#### 1. 가벼움과 보안성

  - 인스턴스 생성시 프로토콜을 타입으로 사용함으로서 해당 인스턴스를 추상화하여 보안성을 높일 수 있습니다.

```swift
protocol Eat {
    func eat()
}

class Human: Eat {
    func speak() {
        print("hello")
    }
    
    func eat() {
        print("yami")
    }
}

// Class로 접근
let human1 = Human()
human1.speak()
human1.eat()

// Protocol로 접근
let human2: Eat = Human()
//human2.speak() 에러, 접근 불가
(human2 as! Human).speak() // 단, 구체적인 인스턴스로 캐스팅하여, 추상화에 의해 가려진 부분에 다시 접근할 수도 있습니다. 
human2.eat()
```

#### 2. Value Type 상속

  - Struct, 클래스, 열거형 등 구조화된 타입 중에 상속은 클래스 타입에서만 가능합니다.

  - 그러나 Protocol Extension을 통해 Struct, Enum같은 Value 타입도 상속 효과를 줄 수 있게 되었습니다.

  - 클래스는 참조 (Call by reference) 타입이므로 참조 추적에 비용이 많이 발생합니다. 비교적 비용이 적은 값 타입을 사용하고 싶어도 상속을 할 수 없으므로 그 때마다 기능을 다시 구현해 주어야 했지만 POP는 그 한계를 없앴습니다.

  - 코드 중복이 발생하는 상황 -> 상속이 없는 구조체로 만들수 있습니다. (성능적 이득)

  - Value Type을 통해 멀티 스레드 환경에서 Thread Safety 높일 수 있습니다.

```swift
protocol GreetProtocol {
  func greet() 
}

extension GreetProtocol {
  func greet() {
    print("hi there")
  }
}

struct SingerStruct: GreetProtocol {
  let canSing = true
}

let tom = SingerStruct()
tom.greet()
```

#### 3. 다중 상속과 수평 확장

  - 하나의 Superclass만 상속 가능한 class와 달리 여러 프로토콜을 상속받을 수 있습니다.

 - OOP에서 클래스의 상속은 특정 타입을 물려받아 하나의 새로운 타입을 정의하고 추가 기능을 구현하는 수직 확장이지만, POP는 기존의 타입에 기능을 추가하는 수평적인 확장 구조를 지닙니다.

```swift
protocol CanShootThrees {}
protocol CanBlock {}
protocol CanRebound {}

struct PureShooter: CanShootThrees {}
struct DefensiveForword: CanBlock, CanRebound {}
struct SuperStar: CanShootThrees, CanBlock, CanRebound {}
```

#### 4. Generics 더하기

  - 제너릭과 함께 쓰면 더 강력한 활용도를 갖습니다.


## where Self

보통 Extension 뒤에 'where Self: 자료형'을 통해 추가적인 조건을 지정할 수 있습니다. Protocol Extension 역시 'A를 따른다고 한 것들 중 특별히 B까지 따르는 것들은 추가로 이것들을 더 주겠다.'라는 의미를 추가할 수 있습니다.

```swift
protocol CanShootThrees {}
protocol CanBlock {}
protocol CanRebound {}

extension CanBlock where Self: CanShootThrees {
  func showOff() {
    print("I can play both offense and defense")
  }
}

struct PureShooter: CanShootThrees {}
struct DefensiveForword: CanBlock, CanRebound {}
struct SuperStar: CanShootThrees, CanBlock, CanRebound {}

let iverson = PureShooter()
let bowen = DefensiveForword()
let jordan = SuperStar()

iverson.showOff() // 에러
bowen.showOff() // 에러
jordan.showOff() // "I can play both offense and defense"
```


### Reference:
- http://blog.yagom.net/531
- http://blog.naver.com/PostView.nhn?blogId=jdub7138&logNo=220968251035&beginTime=0&jumpingVid=&from=search&redirect=Log&widgetTypeCall=true
