# Higher-Order Functions

함수를 다루는 함수입니다. 함수를 인자로 받거나 함수를 반환하는 함수입니다. 이것이 가능한 이유는 Functional Programming에서는 함수도 값으로 취급하기 때문입니다.


```swift
func run(completion: () -> Void) {
    completion()
}
```

run이라는 함수는 function(anonymous function)을 인자로 받고 있습니다. 이런 함수를 higher-order function이라 합니다. 위의 경우는 함수를 인자로 받는 경우이지만, 반대로 함수를 return 할 수도 있습니다. (일급함수에 포함되는 개념이라고 생각하면 될 것 같습니다.)

고차 함수의 사용법을 익혀 두면  

고차 함수를 이용하면 여러 장점이 있습니다.

1. 구현시 반복문 경계 부분의 상태를 올바르게 지정하기가 어려운 부분이 있다면, 고차 함수로 구현하면 **지역화**됩니다.

2. 동일한 패턴을 반복적으로 작성하여 **더 간결한 코드**를 만들게 되고, 더 높은 생산성을 얻게 되며, 가독성 또한 개선하게 됩니다.

  - 고차 함수를 사용하면, 로직 내의 어떤 버그를 수정할 때 프로그램 전체에 퍼져 있는 코딩 패턴의 모든 사례를 고치는 대신, 단 한번만 수정하면 된다.

  - 연산의 효율성을 최적화할 필요가 있다고 판단될 때도 역시 한군데만 수정하면 모든 처리가 가능하다.

  - 추상에 buildString 같은 명백한 이름을 지정해주면 코드를 읽는 사람이 구현의 세부 사항을 해석할 필요 없이 코드가 어떤 동작을 하는지 쉽게 이해할 수 있다.

고차 함수의 사용법을 익히고 나면

## 자주 사용되는 다섯가지의 Higher-Order Functions

- 1. forEach
- 2. filter
- 3. reduce
- 4. map
- 5. flatMap


### forEach

간단하게 말하면 for-in loop 같은 역할을 해줍니다. 아래는 array 안에 있는 1, 2, 3을 차례로 출력해줍니다.

```swift
[1, 2, 3].forEach { (each: Int) in
    print(each)
}
```

하지만 아래의 경우 같이 loop 중간에 loop를 종료할 수는 없다.
```swift
for each in [1, 2, 3] {
    if each == 2 {
        break
    }
    print(each)
}
```

아래와 같이 중간에 return을 해주어도 해당 closure만 중간에 종료될 뿐 loop를 중단 하는 것은 아닙니다.

```swift
[1, 2, 3].forEach { (each: Int) in
    if each == 2 {
        return
    }
    print(each)
}
```

forEach의 존재 의미가 무의미한 것 처럼 보일 수도 있으나 forEach는 for-in과 달리 함수를 파라메터로 받고 있다는 점에서 의미가 있습니다.

만일, array안에 있는 정수를 이용해 상황에 따라 다른 코드를 동작시켜야 한다면 for-in 문으로 다음과 같이 작성할 수 있습니다.

```swift
let numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

if mode == 0 {
    for each in numbers {
        print(each)
    }
} else if mode == 1 {
    for each in numbers {
        print(each * each)
    }
} else {
    for each in numbers {
        print(each + each)
    }
}
```
이것을 forEach와 enum을 함께 이용하여 바꾸면 다음과 같은 형태가 될 수 있습니다. mode에 따라 미리 적용할 함수를 결정하고 array안에 있는 요소들을 그 함수에 넣어주는 형태입니다.

```swift
enum OperationMode {

    case op1
    case op2
    case other

    var f: (Int) -> Void {
        switch self {
        case .op1:
            return { value in print(value) }
        case .op2:
            return { value in print(value * value) }
        case .other:
            return { value in print(value + value) }
        }
    }

}
```

forEach를 for-in같이 단순한 loop로만 바라보면 그 기능을 모두 사용할 수 없고 콜랙션 안의 모든 구성요소를 전달받은 함수에 하나씩 넘겨서 함수를 실행시키는 역할을 한다고 이해하면 더욱 다양하게 forEach를 사용할 수 있게 생각의 전환을 만들어 갈 수 있습니다.


### filter


filter는 어떤 콜랙션에서 특정 조건에 맞는 element들을 필터링하기 위해서 사용됩니다.

아래는 array, dictionary, set에서 사용하는 예제입니다.

```swift
let result = [1, 2, 3, 4, 5].filter { (each: Int) -> Bool in
    return (each % 2 == 0)
}

print(result)

let result2 = ["a":"b", "c":"d"].filter { (key: String, value: String) -> Bool in
    return (key == "a")
}

print(result2)

let set: Set<Int> = [1, 2, 3, 4, 5]
let result3 = set.filter { (each: Int) -> Bool in
    return (each % 2 == 0)
}

print(result3)
```

동시에 여러 스타일의 필터링을 제공해야 할 경우 각각 필터링 함수만 구현해두면 언제든지 collection에 filter 함수를 통해 전달하는 것으로 간단하게 사용할 수 있습니다.



### reduce

reduce는 collection의 element들을 이용해서 어떤 최종 결과 하나를 만들어 낼 때 사용합니다. 아래의 샘플은 단순히 1부터 100까지의 합을 구하는 코드이다. 처음 들어간 0에다 each를 계속 더해 넣습니다.

result는 매 함수가 호출될 때 그때까지의 결과가 들어옵니다.

```swift
let result1 = Array<Int>(1...100).reduce(0) { (result: Int, each: Int) -> Int in
    return result + each
}
```

파라메터로 넘어간 함수의 prototype이 +연산자와 동일하므로 다음으로 대체할 수도 있습니다. (Swift는 연산자도 함수이다.)

```swift
let result2 = Array<Int>(1...100).reduce(0, +)
```

아래는 dictionary의 내용을 reduce를 이용해서 하나의 string으로 만드는 동작을 합니다.

```swift
let result3 = ["a":"b", "c":"d"].reduce("") { (result: String, each: (key: String, value: String)) -> String in
    return result + "(\(each.key):\(each.value))"
}
```


### map


map은 입력된 transform 함수를 이용해서 어떤 타입의 요소를 다른 타입으로 transform해주는 역할을 합니다.

간단히 예를 보면 다음과 같습니다.

```swift
let result = [1, 2, 3, 4, 5,].map { (each: Int) -> Int in
    return each + 3
}
```

result는 [Int]인데, 1, 2, 3, 4, 5가 각각 +3된 값이 됩니다. 즉, [4, 5, 6, 7, 8]이 해당 코드의 실행 결과입니다. transform은 단순히 숫자의 증감도 가능하지만 전혀 다른 타입으로도 가능합니다.

아래의 경우는 array안의 숫자의 제곱을 구하는 것인데, closure를 사용하지 않고 f에 함수를 대입해서 사용한 경우입니다.

```swift
let f = { (value: Int) -> Int in
    return value * value
}

let result = [1, 2, 3, 4, 5].map(f)
```

함수의 원형만 맞으면 어떤 함수든 사용 가능하기 때문에 이미 존재하는 함수를 다음과 같이 사용할 수도 있다.

```swift
let result = [1.0, 2.0, 3.0, 4.0, 5.0].map(sqrtf)
```

흥미로운 점은 map이 꼭 collection에만 존재할 수 있는 것은 아니란 것입니다. 어떤 타입이라도 transform을 지원할 수 있는 형태라면 map함수를 만들수 있습니다. 그렇기 때문에 optional 같은 곳에도 map이 존재할 수 있고 또, 실재로 존재합니다. (optional은 enum입니다.)

다음은 Int?의 제곱을 구하는 방법입니다. * 연산자가 optional에는 사용할 수 없어 이런 방법을 사용하지 않으면 unwrap을 해서 곱한 뒤 다시 optional로 바꿔주어야 합니다. map은 이 과정을 단순화 시킵니다.

```swift
let value: Int? = 3
let result = value.map { $0 * $0 }
```
위의 경우 result는 value가 some이면 해당 계산결과의 optional이 되고 none이면 nil이 됩니다.


### flatMap

일단 flatMap은 얼핏 보기에 두가지 일을 하는 것으로 보이는데 그것은 다음과 같습니다.

1. collection안의 collection을 flatten 시킵니다.

2. collection안의 값들 중 Optional을 제거하는데 nil인 경우는 빼버립니다.

첫번째의 경우를 보면 다음과 같은 경우입니다.

```swift
let numbers = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
let result = numbers.flatMap { (each: [Int]) -> [Int] in
    return each
}
//  결과
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

간단히 줄여서 다음과 같이 자주 사용합니다.

```swift
let result = numbers.flatMap { $0 }
```

두번째 사용처인 경우 다음과 같은 예로 확인 가능합니다.

```swift
let numbers: [Int?] = [1, 2, 3, nil, 4, 5, 6, nil, 7, 8, 9]
let result = numbers.flatMap { (each :Int?) -> Int? in
    return each
}
//  결과
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

역시 간단히 줄여서 다음과 같은 모양으로 자주 사용합니다.

```swift
let result = numbers.flatMap { $0 }
```

엄밀히 말하면 두 가지 함수는 같은 이름의 서로 다른 함수입니다. 다만, 넓게 보았을 때 각 element의 context를 제거한다는 점에서 동일한 동작을 하는 것으로 볼 수 있습니다.

```swift
public func flatMap<SegmentOfResult : Sequence>(_ transform: (Element) throws -> SegmentOfResult) rethrows -> [SegmentOfResult.Iterator.Element]

public func flatMap<ElementOfResult>(_ transform: (Element) throws -> ElementOfResult?) rethrows -> [ElementOfResult]
```


### Reference:
- https://yobi.navercorp.com/iOSDev/posts/261
