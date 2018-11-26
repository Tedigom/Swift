# Liskov Substitution Principle (리스코프 치환 원칙)

“ 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다. ”는 원칙입니다.

예시를 들어 설명하자면, 라면을 끓이는 로직에 “라면”이라는 단어에 “신라면” 혹은 “진라면”이라고 넣어도 그 로직이 정상적으로 동작해야 한다는 것을 말합니다. (라면의 하위 타입 인스턴스로 신라면, 진라면을 대입한 것 입니다.)

하위 타입의 인스턴스로 바꿀 수 있어야 한다는 이야기는 실제로 하위 타입으로 바꾸겠다는 것이 아니라 추상화에 의존해서 작성된 코드에 구체화된 객체(어떤 하위 타입의 인스턴스)를 넣어도 그 동작이 이루어져야 한다는 말입니다. 그러기 위해서는 부모의 기능을 자식이 거부해서는 안됩니다. 상속의 본질인데, 이를 지키지 않으면 부모 클래스 본래의 의미가 변해서 is a 관계가 망가져 다형성을 지킬 수 없게 됩니다. LSP란 상속의 툴이며, OCP를 위반하지 않도록 인도하는 원칙입니다. 이 원칙은 크게 다음과 같은 조건들을 포함합니다.

-  서브 타입(자식 클래스) 은 언제나 자신의 기반타입(부모 클래스) 으로 교체 할 수 있어야 한다.

- 클래스인 경우, 하위 클래스라면 상위 클래스의 한 종류여야 한다.

- 인터페이스인 경우, 구현 클래스는 인터페이스(규약) 를 지켜야한다.


## LSP 위배

리스코프 치환 원칙이 잘 지켜지지 않는다면 다음과 같은 문제가 발생할 수 있습니다.

1. 클래스 계층이 명료하지 않게 됩니다. 서브클래스 인스턴스를 파라미터로 전달했을 때 메소드가 이상하게 작동할 수 있습니다.

2. 슈퍼클래스에 대해 작성된 단위테스트가 서브클래스에 대해서는 작동하지 않을 것 입니다.

가장 대표적으로 드는 예시가 정사각형과 직사각형의 예시입니다. (~~식상~~)

```swift
class Rectangle {
    private var width: Int = 0
    private var height: Int = 0

    func getWidth() -> Int {
        return width
    }

    func getHeight() -> Int {
        return height
    }

    func setWidth(width: Int) {
        self.width = width
    }

    func setHeight(height: Int) {
        self.height = height
    }

    func getArea() -> Int {
        return width * height
    }
}

class Square: Rectangle {
    override func setWidth(width: Int) {
        super.setWidth(width: width)
        super.setHeight(height: width)
    }

    override func setHeight(height: Int) {
        super.setHeight(height: height)
        super.setWidth(width: height)
    }
}
```

개념적으로 정사각형은 직사각형에 속하기 때문에 Rectangle 클래스를 상속받고, 높이와 넓이가 같기 때문에 하나의 값을 세팅하면 자동적으로 높이와 넓이 모두를 같게 세팅하도록 해주었습니다. 이 자체만 보면 Square 클래스는 논리적으로 아무런 문제가 없습니다. 그러나 다음과 같은 테스트 함수에서 파라미터로 슈퍼클래스(Rectangle)가 전해지느냐 서브클래스(Square)가 전해지느냐에 의해 차이가 존재합니다.

```swift
func testAreaSize(rect: Rectangle) {
    rect.setHeight(height: 5)
    rect.setWidth(width: 4)

    if rect.getArea() == 20 {
        print("success")
    } else {
        print("error")
    }
}

testAreaSize(rect: Rectangle()) // success
testAreaSize(rect: Square()) // error
```

이는 LSP 위반입니다. 상속 구조상 자식이 거부할 수 밖에 없는 기능을 부모가 제공하게 된다면 LSP는 깨지게 되고 부모만 가지고 추상화된 상태에서 코딩을 하는 것은 위험하게 됩니다. 이 경우 상속 구조 자체가 잘못된 상황입니다. 이처럼 OOP에서 상속은 매우 비용이 큰 작업이며 한번 상속관계로 만들어진 커플링은 쉽게 끊어내기 힘들다고 했습니다. 상속을 하게 될때 LSP를 더욱 진지하게 고민해야 하는 이유가 여기에 있습니다.

이와 같이 LSP를 위반하게 되면 앞서 설명드린 OCP도 제대로 동작하지 않을 수 있습니다. 모든 코드에서 하위 클래스들을 명시적으로 지정해서 코딩을 해야 하기 때문입니다. 하위 클래스들을 명시적으로 지정해서 코딩을 해야 한다는 것은 코드의 복잡도를 엄청나게 높여버립니다. 예를 들어 심지어 부모 클래스가 자식 클래스를 일일이 알아야 하는 경우도 발생합니다. 심각하면 객체를 만드는 의미조차 없는 지경으로 만들어 버립니다. LSP 위반이 많아지면 어떤 객체도 우리가 예측한 동작을 할거라는 기대를 가질 수 없습니다. 이와 같은 맥락에서 생각하였을 때, LSP의 원칙이 깨질 때의 주요현상으로 타입을 확인하는 기능을 일일히 명시적으로 사용하는 경우를 말할 수 있습니다. 아래는 특정 하위클래스의 타입을 확인하는 간단한 예시입니다. 아래의 경우 SpecialItem이라는 하나의 하위클래스의 타입만을 확인하지만, 예외의 상황이 많아질수록 매우 복잡한 if문이 탄생할 것 입니다. 

```swift
class Item {
    var price: Int = 0
}

class SpecialItem: Item {
}

class Coupon {
    func calcuateDiscountAmount(item: Item, discountRate: Int) -> Int {
        if item is SpecialItem { // LSP 위반
            return 0
        }
    return item.price * discountRate
    }
}
```


LSP를 준수하면 추상화된 하나의 클래스로 부터 상속된 수많은 다른 클래스를 일일이 고민하지 않고 추상화된 인터페이스 하나로 공통의 코드를 작성할 수 있게 됩니다. 이는 곧 OCP의 완벽한 동작을 보장해준다는 이야기입니다. 위의 코드를 리팩토링하면 다음과 같습니다.

```swift
class Item {
    var price: Int = 0
    func isDiscountAvailable() -> Bool {
        return true
    }
}

class SpecialItem: Item {
    override func isDiscountAvailable() -> Bool {
        return false
    }
}

class Coupon {
    func calcuateDiscountAmount(item: Item, discountRate: Int) -> Int {
        if !item.isDiscountAvailable() {
            return 0
        }
    return item.price * discountRate
    }
}
```
