# Interface Segregation Principle (인터페이스 분리 원칙)

“ 클라이언트가 자신이 이용하지 않는 메서드에 의존하지 않아야 한다는 원칙 ”는 원칙입니다.

Swift에서는 protocol이 더욱 작고 잘게 나눠 진 것을 보실 수 있을 것입니다. 이렇게 인터페이스가 작고 구체적이어야 그 인터페이스를 지원하는 쪽의 코드가 더욱 간결해 질 수 있습니다.

인터페이스 분리 원칙은 클라이언트가 자신이 이용하지 않는 메서드에 의존하지 않아야 한다는 원칙입니다. 인터페이스 분리 원칙은 큰 덩어리의 인터페이스들을 구체적이고 작은 단위들로 분리시킴으로써 클라이언트들이 꼭 필요한 메서드들만 이용할 수 있게 합니다. 이와 같은 작은 단위들을 역할 인터페이스라고도 부릅니다. 인터페이스 분리 원칙을 통해 시스템의 내부 의존성을 약화시켜 리팩토링, 수정, 재배포를 쉽게 할 수 있습니다.

여기서 클라이언트라 함은 어떤 다른 객체를 사용하는 쪽을 지칭합니다. 통상 또 다른 객체가 됩니다. ISP에서는 **객체를 사용하는 측을 “클라이언트”, 사용되어지는 측을 “서버”** 라고 표현합니다. 즉 A라는 객체 내부에 B라는 객체를 사용하게 된다면 A는 클라이언트, B는 서버가 되는 것입니다. “사용”이라는 말은 상속을 해서 사용하든, 내부에서 해당 객체를 만들어서 사용하든 모든 경우에 해당합니다.


여기서 자신이 사용하지 않는 메서드에 의존하지 않아야한다는 말이란 서버측에서 여러개의 메서드를 제공하고 있는데 만일 클라이언트 측에서 특정 몇몇의 메서드만 사용하고 있는 상황에서 서버측에서 클라이언트가 사용하지 않는 메서드를 변경했을 때 클라이언트가 영향을 받지 않아야 한다는 것입니다. 즉, 클라이언트는 서버가 제공하는 최소한의 자신이 필요로 하는 인터페이스만 알게 해서 서버와의 의존도를 낮춰야 한다는 이야기입니다.

이것이 가능해지려면 다음의 내용을 따라야 합니다.

“큰 덩어리의 인터페이스들을 구체적이고 작은 단위들로 분리시킴으로써 클라이언트들이 꼭 필요한 메서드만 이용할 수 있게 한다.”

## ISP 위배

하나의 인터페이스 / 프로토콜을 모든 클라이언트가 구현하는 경우가 ISP를 위배한다고 말할 수 있습니다. 아래는 그러한 예시입니다.

```swift
protocol  ArticleService {
    func subscribe()
    func write()
    func delete()
}

class Subscriber: ArticleService {
    func subscribe() {
        // some code
    }

    func write() {
        // some code
    }

    func delete() {
        // some code
    }
}

class Writer: ArticleService {
    func subscribe() {
        // some code
    }

    func write() {
        // some code
    }

    func delete() {
        // some code
    }
}

class Deleter: ArticleService {
    func subscribe() {
        // some code
    }

    func write() {
        // some code
    }

    func delete() {
        // some code
    }
}
```

이를 리팩토링하면 다음과 같습니다.

```swift
protocol  ArticleSubscribeService {
    func subscribe()
}

protocol  ArticleWriteService {
    func write()
}

protocol  ArticleDeleteService {
    func delete()
}

class Subscriber: ArticleSubscribeService {
    func subscribe() {
        // some code
    }
}

class Writer: ArticleWriteService {
    func write() {
        // some code
    }
}

class Deleter: ArticleDeleteService {
    func delete() {
        // some code
    }
}
```

## ISP in iOS

Objective-C나 Swift 모두에서 공통적으로 모두 ISP의 흔적을 많이 찾아볼 수 있습니다. UITableView가 dataSource와 delegate 프로토콜이 별도로 분리되어 있는 것 역시 ISP의 흔적입니다. dataSource만 쓰고 delegate는 쓸 필요없는 경우가 꽤 존재하기 때문입니다.

그외도 각종 프로토콜들이 ISP를 기반으로 설계되었습니다. Swift의 String만 보더라도 Equatable, Comparable, Hashable 등 메서드 한둘만 제공하는 프로토콜들을 매우 많이 사용하고 있고 그것들의 논리적인 조합으로 String의 전반적인 동작들을 정의하고 있습니다.

실제로 POP를 제대로 하기 위해서는 ISP를 제대로 이해하고 습관화 시키는 것이 매우 중요한 일입니다. 고전적인 ISP는 abstract class의 다중상속을 많이 활용하는데 Swift에서는 그 역할을 protocol이 해주고 있기 때문입니다. 그래서 ISP에 따라 작게 분해된 인터페이스(protocol)를 기반으로 코딩하는 것이 POP입니다.

## SRP와 ISP

SRP가 수정해야할 이유를 기준으로 클래스를 작게 분해 하는데 포커스를 맞추고 있다면 ISP는 인터페이스를 명확한 목적별로 잘게 나누고 인터페이스를 기준으로 코딩을 하라고 이야기합니다.

일단 가장 먼저 선행되어야 할 것은 서버 클래스를 SRP를 준수하게 잘게 분해하는 작업이 필요합니다. 일단 그것 만으로도 변경에 대한 영향을 많은 부분 줄여줄 수 있습니다.

만일 그럼에도 불구하고 서버 클래스가 비대하거나 서버 클래스의 인터페이스가 자주 변경이 되어야 할 상황이라면 인터페이스를 grouping해서 분리해내거나 서버 클래스를 바로 사용하지 않고 인터페이스 만을 사용하게 변경하면 그런 부분을 많이 줄여줄 수 있습니다.

구현과 인터페이스를 분리하는 일이 항상 최선이라고 말하기는 힘듭니다. 모든 클래스의 인터페이스를 protocol로 만들어서 사용하는 것은 분명 비효율적인 일입니다.

하지만 만일 특정 클래스의 인터페이스가 매우 비대해서 한꺼번에 제공하기에는 사용법이 어렵다던지 논리적으로 명쾌하지 못한 경우가 발생한다면 그것들을 목적에 맞게 잘게 나눈 protocol로 구분해서 제공하는 것이 훨씬 나은 접근법이 될 수도 있을 것 입니다.
