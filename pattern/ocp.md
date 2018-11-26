# Open / Closed Principle (개방-폐쇄 원칙)

모든 개발자들의 공통적인 희망 사항 중 하나는 새로운 기능을 쉽게 추가하고 기존의 기능에 대한 수정을 쉽게 하고 싶어 하는 것 이라고 생각합니다. OCP는 그런 희망사항을 충족시켜주는 원칙입니다. SRP가 패턴들의 시작점이라면 OCP는 패턴들이 체계화시켜주는 첫 관문입니다. OCP의 정의는 다음과 같습니다.

“ 확장에는 열려있으나 변경에는 닫혀 있어야 한다. ”



확장이란 새로운 기능을 추가하는 것을 말하며 열려있다는 것은 쉽게 가능해야한다는 의미입니다. 즉, 확장에 열려있다는 것은 **새로운 기능의 추가는 쉬워야 한다** 는 말입니다.

변경은 말 그대로 기존의 기능의 변경 및 수정인데 닫혀있다는 말은 수정의 영향이 부분적인 특정 위치에 갇혀 있어야 한다는 이야기입니다. 한곳을 고쳤더니 줄줄 딸려서 여러 곳이 고쳐지면 안된다는 이야기입니다. 즉 변경에 닫혀있다는 것은 **변경이 발생하면, 다른 곳에 영향을 주지 않고 한 곳에서만 수정이 이루어져야 한다** 는 말입니다.

> 확장에 대해 열려있다 : OCP는 새로운 기능을 확장하기 위해서 잘 동작하고 있는 기존의 코드를 손대는 대신 새로운 행위(코드)를 추가 하는 것으로 확장을 가능하게 해줍니다.

> 수정에 대해 닫혀있다 : 새로 추가 되는 코드 혹은 수정되는 코드는 기존 코드의 변경을 초래하지 않습니다.

기능의 추가는 간단하고 쉽게, 수정은 범위가 좁에 영향을 미쳐 한곳만 수정할 수 있어야 한다는 말인데 수정을 가두어서 얻을 수 있는 해택은 보다 빠르고 안정적인 수정, 수정 후의 사이드 이팩트 최소화 같은 것들이 있을 수 있습니다. 만약 수정에 갇혀있지 않는 코드의 수정은 단계적으로 다른 코드로 수정이 파급효과를 미치게 됩니다.

조금이라도 복잡한 코드를 작성해 보았다면 한 개의 클래스에 대해 기능을 수정 했는데 다른 곳을 또 수정해야 하고 그 수정의 결과로 또 다른 곳을 수정해야 하는 경우를 한번씩은 경험을 해보셨을 것 같습니다. 이렇게 수정이 힘들어지는 경우 코드는 경직성을 띄고 있다고 하며 대표적인 악취(코드의 악취: Clean Code 참고)로 이야기 되고 있습니다.

OCP가 잘지켜지는 대표적인 예로 JDBC를 사용하는 클라이언트는 DB가 오라클에서 MySQL로 바뀌는 경우를 생각해 볼 수 있습니다. 이 경우, OCP가 잘 지켜졌다는 가정하에, 개발자는 Connection설정하는 부분 외에는 따로 수정할 필요가 없습니다.  Connection 설정 파일을 따로 분리해두면 클라이언트는 코드를 한줄도 변경할 필요 없기 때문에 어떤 DB 를 쓰더라도 확장엔 열려있고 코드 수정에 변화가 없으므로 주변 변화에는 닫혀 있다고 볼 수 있습니다.




어떻게 하면 그렇게 되나 #

앞서 글에서 OOP에서 가장 중요한 개념이 추상화라는 이야기를 했었습니다.
가장 많은 디자인 패턴의 기본 원리가 되는 OCP는 추상화에 크게 의존하고 있습니다.

인터페이스는 고정되어있지만 그 구현이 제한되어 있지는 않은 행위를 추상화 시키는 것은 가능한 일입니다.
잘 추상화된 인터페이스를 상속받아 구체적인 행위를 구현하는 것이 OCP의 기본 원리입니다.
OCP를 기본으로 구현된 모듈은 기본적으로 기능 추가쉽고 수정에 닫혀있는 속성과 더불어
그것을 사용하는 코드는 구체화된 클래스에 의존하지 않고 추상화된 인터페이스만 사용해서 동작하므로
새로운 구현 클래스가 추가되었다고 해도 코드를 손대야 할 이유가 없습니다.

## OCP 위배

OCP가 깨지는 대표적인 예는 **반복적인 분기문** 이 나타나는 경우입니다. 흔히 Swift에서 enum을 사용합니다. 그런데 이런 enum의 값의 판단, 즉 if 혹은 switch가 한곳이 아니라 여러 곳에 존재하며 반복적으로 등장한다면 해당 코드는 거의 대부분 OCP 위반이며 디자인 패턴 중 하나를 사용할 수 있는 위치일 확률이 매우 높습니다.

이러한 분기문의 경우 새로운 타입 자체를 하나 추가하는 것은 매우 쉽습니다. 하지만 타입 추가와 동시에 모든 분기문을 점검해야 합니다. 분기문이 많아서 기억할 수 없다면, 어디 한군데에서는 꼭 수정을 빠트리기 마련입니다. 놓치기 쉬운 이유는 **수정에 닫혀있지 않기 때문** 입니다. 새 맴버의 추가는 여기저기 흩어진 수많은 분기문에 영향을 주기 마련입니다.

아래는 간단한 예시입니다.

```swift
enum Vehicle {
    case sedan
    case sportUtility
}

let type = Vehicle.sedan
var text = "이 자동차는 "

if type == .sedan {
    text += "세단"
} else if type == .sportUtility {
    text += "SUV"
}

text += "이므로 요금은 "

if type == .sedan {
    text += "만원"
} else if type == .sportUtility {
    text += "만오천원"
}

text += "입니다."

print(text)
```

이런 경우가 대표적인 상황입니다. Vehicle이라는 enum으로 정의된 타입이 있고 멤버로는 sedan과 sportUtility가 있습니다. 만일 여기에서 hatchback을 추가한다고 가정할 수 있습니다.

case에 hatchback 자체를 추가 하는 것은 그리 어렵지 않습니다. 하지만 두군데 나눠져 있는 (심지어 더 많을 수도 있는) branch를 일일이 따라 다니며 수정하는 것은 어렵고 정밀하지 못한 작업이 될 수 있습니다. 이 코드에 hatchback을 추가하고 swift스럽게 바꾸면 아래와 같이 바꿀 수 있습니다.

```swift
enum Vehicle {

    case sedan
    case hatchback
    case sportUtility

    var readableName: String {
        switch self {
        case .sedan:
            return "세단"
        case .hatchback:
            return "해치백"
        case .sportUtility:
            return "SUV"
        }
    }

    var chargeText: String {
        switch self {
        case .sedan:
            return "만원"
        case .hatchback:
            return "칠천원"
        case .sportUtility:
            return "만오천원"
        }
    }
}

let type = Vehicle.hatchback
var text = "이 자동차는 " + type.readableName + "이므로 요금은" + type.chargeText + "입니다."

print(text)
```

원래의 코드보다 비교적 좋아보입니다만, 여전히 한계는 있습니다. 만일 각 차종별 특정한 복잡한 동작을 해야 한다면 코드는 다음과 같은 상태가 됩니다.

```swift
enum Vehicle {

    case sedan
    case hatchback
    case sportUtility

    var readableName: String {
        switch self {
        case .sedan:
            return "세단"
        case .hatchback:
            return "해치백"
        case .sportUtility:
            return "SUV"
        }
    }

    var chargeText: String {
        switch self {
        case .sedan:
            return "만원"
        case .hatchback:
            return "칠천원"
        case .sportUtility:
            return "만오천원"
        }
    }

    func run() {
        switch self {
        case .sedan:
            print("여기에서 세단에 대한 무엇인가 복잡한 동작 코드가 실행됨")
        case .hatchback:
            print("여기에서 해치백에 대한 무엇인가 복잡한 동작 코드가 실행됨")
        case .sportUtility:
            print("여기에서 SUV에 대한 무엇인가 복잡한 동작 코드가 실행됨")
        }
    }
}

let type = Vehicle.hatchback
var text = "이 자동차는 " + type.readableName + "이므로 요금은" + type.chargeText + "입니다."

print(text)

type.run()
```

여기에 나와있는 무엇인가 복잡한 동작이라는 것을 수백라인 이상의 구현이 필요한 동작이라고 생각해보면, 새 맴버의 추가는 여기저기 흩어진 수많은 분기문에 영향을 줄 수 있다는 점에서 높은 확률로 OCP 위배임을 알 수 있습니다. 이를 추상화와 상속을 이용하여 SOLID 원칙을 따르게끔 리팩토링하면 다음과 같습니다.

```swift
protocol Vehicle {

    var readableName: String { get }
    var chargeText: String { get }
    func run()

}

struct Sedan: Vehicle {

    var readableName = "세단"
    var chargeText = "만원"

    func run() {
        print("여기에서 세단에 대한 무엇인가 복잡한 동작 코드가 실행됨")
    }

}

struct Hatchback: Vehicle

    var readableName = "해치백"
    var chargeText = "칠천원"

    func run() {
        print("여기에서 해치백에 대한 무엇인가 복잡한 동작 코드가 실행됨")
    }

}

struct SportUtility: Vehicle {

    var readableName = "SUV"
    var chargeText = "만오천원"

    func run() {
        print("여기에서 SUV에 대한 무엇인가 복잡한 동작 코드가 실행됨")
    }

}

let type: Vehicle = SportUtility()
var text = "이 자동차는 " + type.readableName + "이므로 요금은" + type.chargeText + "입니다."

print(text)

type.run()
```

이 경우 새로운 타입이 추가되면 하나의 클래스를 더 추가해주면 되는 형태가 됩니다.  그리고 protocol로 그 클래스가 무엇을 지원해야 할지 정의해두었기 때문에 그것에 맞추어 코드를 추가해주면 됩니다.

예를 들어 여기에서 pickupTruck이라는 타입이 추가되면 Vehicle을 상속받은 pickupTruck을 추가하고 두개의 property와 하나의 method를 추가해주면 됩니다. **다른 코드에는 전혀 영향을 미치지 않습니다. (OCP 준수)**

더욱이 단 하나의 if나 switch도 사용되지 않았기 때문에 test시 매우 간단해집니다. Vehicle을 상속받은 **각각의 클래스들은 온전히 자신이 처리해야 할 일만 간결하게 처리하고 있기 때문입니다. (SRP 준수)**

## OCP 고찰

그러나 무조건적으로 OCP를 준수하면 어떤 경우라도 더 좋은 코드가 될 수 있다고 말하기는 어렵습니다. OCP가 남용되면 코드가 불필요한 복잡성을 띄기도 쉽습니다.

겨우 분기가 한번 정도 일어나는 가벼운 enum을 굳이 OCP로 구현 해야 한다고 일일이 추상 클래스를 만들고 있다면 얻는 것 보다 잃는 것이 많을지도 모릅니다.

OCP가 제대로 제 역할을 해낼려면 다음과 같은 조건들이 있는 경우입니다.

- ##### 타입에 새로운 맴버들이 계속 추가될 가능성이 크다.

- ##### 타입의 맴버로 분기 처리 되는 곳이 매우 많다. (다소 주관적)

그러다보니 확신이 서지 않을 때는 처음부터 OCP를 염두에 두고 작업을 하지 않고 작업 하는 중에 위의 두가지 경우가 발생한다면 그때 적용해도 괜찮습니다.

어느 정도 이상의 큰 리팩토링이 있어야 할지 모르지만, 만일 그 타이밍을 놓치고 그냥 다시 branch로 여기저기 땜빵을 한다면 당장은 작업이 빨리 진행되는 것 처럼 보여도 가면 갈수록 점점 코드는 경직성을 띄게 될 것입니다. 
