# Equality and Ordering

contains(_ : )나 표준 비교 연산자와 같은 간단한 collection operation 등에 사용자가 유형을 정의하여 사용할 수 있습니다.

## Equatable

값이 동일한지 아닌지를 비교할 수있는 타입입니다. Equatable 프로토콜을 준수하는 타입은 등호 연산자 (==) 또는 같지 않음 연산자 (!=)를 사용하여 동등성을 비교할 수 있습니다. Swift 표준 라이브러리의 대부분 기본 데이터타입은 Equatable를 따릅니다.


간단한 Struct를 만들어보겠습니다.

```swift
struct StreetAddress {
    let number: String
    let street: String

    init(_ number: String, _ street: String) {
        self.number = number
        self.street = street
    }
}
```

위 StreetAddress 구조체를 사용하여 addresses라는 [StreetAddress] 타입의 Array를 만들겠습니다.

```swift
let addresses = [StreetAddress("1490", "Grove Street"),
                 StreetAddress("2119", "Maple Avenue"),
                 StreetAddress("1400", "16th Street")]
```

addresses 배열에서 특정 StreetAddress(home)가 포함되어 있는지를 찾기 위해서 보통 다음과 같이 찾을 수 있습니다.

```swift
let home = StreetAddress("2119", "Maple Avenue")
let containsHome = addresses.contains { (address) -> Bool in
    return address.number == home.number && address.street == home.street
}
```

contains 함수에 closure를 이용하여 구현할 수 있지만 Equatable 프로토콜을 conform하여 좀 더 아름답게 수정할 수 있습니다.

```swift
extension StreetAddress: Equatable {
    static func == (lhs: StreetAddress, rhs: StreetAddress) -> Bool {
        return lhs.number == rhs.number && lhs.street == rhs.street
    }
}
```

StreetAddress 구조체에 Equatable프로토콜을 conform 한 후 ==함수를 다음과 같이 구현해줍니다

이제, 두 객체간의 같다/다르다를 == 또는 != 연산자를 사용하여 표현할 수 있습니다.

```swift
let home = StreetAddress("2119", "Maple Avenue")
let yourHome = StreetAddress("2119", "Maple Avenue")
home == yourHome
```

하지만, Equatable 프로토콜은 단순 비교 뿐만아니라, 위에 테스트 했던 contains 함수를 클로져를 사용하지 않고 구현할 수 있게 해줍니다.

```swift
let containsHome = addresses.contains(home)
```


## Comparable

<, <=, >=, > 등과 같은 상대적 연산자를 사용하여 비교하는 타입입니다.

위의 예시를 가지고 다시 설명해보고자 합니다. 만약, addresses 배열을 정렬을 해야한다면 보통의 경우 다음과 같이 구현할 수 있습니다.

```swift
addresses.sorted { $0.number < $1.number || $0.street < $1.street }
```

위 코드는 1줄로 짧긴 하지만 여전히 closure를 사용하고 있고, 여러 곳에서 정렬이 이루어진다면 비슷하게 생긴 클로져가 중복해서 쓰여지지만 Comparable 프로토콜을 사용하여 아름답게 바꿀 수 있습니다.

Comparable 프로토콜은 Equtable 프로토콜을 상속받기 때문에 기존에 Equatable을 상속받은 extension은 제거할 수 있습니다.

```swift
extension StreetAddress: Comparable {
    static func == (lhs: StreetAddress, rhs: StreetAddress) -> Bool {
        return lhs.number == rhs.number && lhs.street == rhs.street
    }

    static func < (lhs: StreetAddress, rhs: StreetAddress) -> Bool {
        return lhs.number < rhs.number || lhs.street < rhs.street
    }
}
```

Comparable 프로토콜을 conform 한 뒤 ==, <함수를 구현해 줍니다.
이제, 두 객체간의 비교연산 (<, <=, >, >=)이 가능할 뿐만 아니라, 위의 문제였던 sorted 함수를 클로져를 사용하지 않고 구현할 수 있게 만들어줍니다.

```swift
addresses.sorted()
```

### Reference:

- https://developer.apple.com/documentation/swift/swift_standard_library/basic_behaviors
