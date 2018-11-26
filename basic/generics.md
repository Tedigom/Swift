# Generics

제네릭이란, 일반 프로그래밍 원론에서 다루는 개념을 살펴보면 데이터 형식에 의존하지 않고 **하나의 값이 여러 다른 데이터타입을 가질 수 있는 기술**에 중점을 두어 재사용성을 높일 수 있도록 하는 프로그래밍 개념입니다.

객체 지향 프로그래밍에서는 제네릭을 위의 기본 개념을 바탕으로 하여 클래스 내부에서 사용될 데이터 타입을 클래스 내부에서 지정하지 않고 클래스 외부에서 지정하는 기법을 이야기합니다. 다시 말해서, 클래스 코드 블록을 정의할 때 데이터 타입을 지정하는 것이 아니라 클래스의 **인스턴스를 생성할 때 데이터 타입을 지정**해준다는 것입니다.

제네릭을 사용하고자 하는 함수나 타입의 이름 뒤에 <> 를 넣고 그 사이에 제네릭 타입을 작성하면 됩니다. 스위프트의 클래스, 열거형 그리고 구조체 뿐만 아니라 함수나 메서드에까지 제네릭 형식을 만들수 있습니다.

제너릭을 이용한 스택 struct 예시입니다.

```swift
struct Stack<T> {
    var items = [T]()
    mutating func push(item: T) {
        items.append(item)
    }
    mutating func pop() -> T {
        return items.removeLast()
    }
}
```

프로토콜을 구현하기 위한 타입을 요구하거나 두 타입이 동일하기를 요구하거나 혹은 특정 수퍼 클래스를 가지는 클래스를 요구하는 등의 요청 목록을 지정하기 위해 타입 이름 뒤에 where 키워드를 사용할 수 있습니다.

```swift
func allItemsMatch<C1: Container, C2: Container
    where C1.ItemType == C2.ItemType, C1.ItemType: Equatable>
    (someContainer: C1, anotherContainer: C2) -> Bool {

        // check that both containers contain the same number of items
        if someContainer.count != anotherContainer.count {
            return false
        }

        // check each pair of items to see if they are equivalent
        for i in 0..<someContainer.count {
            if someContainer[i] != anotherContainer[i] {
                return false
            }
        }

        // all items match, so return true
        return true

}
```

### Reference:

https://m.blog.naver.com/PostView.nhn?blogId=sqlpro&logNo=220022600832&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F
