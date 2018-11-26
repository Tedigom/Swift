# Enum (열거형)

열거라는 뜻을 가진 Enumeration에서 따온 용어입니다. 한글로 번역할 때에는 열거형이라는 말을 많이 사용합니다. 특정한 이름을 나열하고 이 이름을 값 대신 쓸 수 있도록 하는 기능입니다. 각 케이스는 고유의 raw 값을 가질 수 있는데 이는 비단 정수 뿐만 아니라 문자나 문자열, 정수 및 실수값을 가질 수 있습니다.

```swift
enum CompassPoint {
    case North
    case South
    case East
    case West
}

/// 혹은 한줄에 쓸 수 있습니다.
enum CompassPoint {
  case north, south, east, west
}
```

개별 사례의 원값을 지정하려면 enum 타입 자체의 타입을 지정해주어야 합니다.

```swift
enum CompassPoint: String {
  case north //이 때는 "north"가 됩니다.
  case south="South", east="East", west="West"
}

let p = CompassPoint.north
print(p.rawValue) // "north"
```

Swift가 발표되고 열거형의 항목에 자료형과 원시 값을 설정할 수 있고 메서드나 프로퍼티를 정의할 수 있게 되었습니다. 또한, Codable이 도입되면서 원시 값에서 Constant로 편하게 변환할 수 있게 되면서 열거형을 활용하는 코드가 좀 더 세련되게 정리될 수 있는 환경이 되었다고 할 수 있습니다.

enum을 통하여 단순히 코드의 가독성만 향상만을 목적으로 하는 것이 아니라 가능한 최대한의 캡슐화를 통해서 열거형이 사용될 수 있는 모든 곳의 코드를 간결하게 만들고 쉽게 수정할 수 있는 것을 목적으로 할 필요가 있습니다.

```swift
enum Married: Int, Codable {
    case Single = 0
    case Married = 1
```
