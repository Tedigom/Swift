# Tuple

튜플Tuple은 여러가지 타잆의 값들의 묶음입니다. 배열과 비슷하지만 길이가 고정되어 있다는 점에서 차이가 있습니다.

```swift
var coffeeInfo = ("아메리카노", 5100)

coffeeInfo.0 // 아메리카노
coffeeInfo.1 // 5100
coffeeInfo.1 = 5100

var namedCoffeeInfo = (coffee: "아메리카노", price: 5100)

namedCoffeeInfo.coffee // 아메리카노
namedCoffeeInfo.price // 5100
namedCoffeeInfo.price = 5100
```

compound type인 만큼 Named types나 혹은, 다른 compound types를 포함합니다.

예를 들어서 (Int, (Int, Int)) 타입의 경우는 Named type인 Int를 첫번째 요소로, Compound type인 (Int, Int)를 두번째 요소로 갖는 compound type 입니다.

이러한 Tuple의 특성 중 주의할 점은 [Sequence](./sequence.md) 프로토콜을 따르지 않기 때문에 iterate 하거나 index로서 접근할 수는 없다는 것 입니다. 

## Tuple의 장점

### 1. 간단한 구조체의 대체가 가능하다.

이름이 붙은 튜플 아이템은 구조체 등 다량의 데이터를 표시하기 위한 특수 데이터구조를 쉽게 만들 수 있습니다.

```swift
let whoAmI = (name: "Seorenn", age: 24, gender: "Etc")
println("I am \(whoAmI.name), \(whoAmI.age) years old, and gender is \(whoAmI.gender)")
```

그러나 주의할 점은, 간단한 자료형을 만들 때에는 구조체 대신 튜플을 사용하기도 하지만 데이터 구조가 임시범위를 넘어서 존속할 가능성이 있는 경우에는 클래스나 구조체로 모델링해야 한다는 것 입니다.

### 2. 멀티 리턴값을 만들 수 있다.

함수는 일반적으로 하나의 값 만을 리턴합니다. 하지만 2개 이상의 값을 리턴해야 할 필요가 있을 수도 있습니다. 보통은 함수에서 리턴할 구조체나 클래스를 만들어서 그 인스턴스를 함수에서 만들어서 리턴하는 경우가 다반사입니다.

그런데 스위피트는 이 튜플을 활용하면 간단하게 처리가 가능합니다.

```swift
func plusAndMinus(a: Int, b: Int) -> (Int, Int) {
    return (a + b, a - b)
}

let (plusResult, minusResult) = plusAndMinus(1, 2)
```

이 외에 튜플의 활용은 함수의 인자로 전달 할 때 등등 다양한 요소에 활용이 가능합니다.

### Reference:

- http://seorenn.blogspot.com/2014/06/swift-tuple.html
