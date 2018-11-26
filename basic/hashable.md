# Hash for Sets and Dictionaries

## Hashable

Hash란 특정한 데이터를 이를 상징하는 더 짧은 길이의 데이터로 변환하는 행위를 말하곤 합니다. 이러한 해시 값은 임의의 길이의 데이터를 고정된 길이의 데이터로 매핑하는 함수인 해시 함수에 의해 얻어지는 값 입니다. 매우 빠른 데이터 검색을 위한 컴퓨터 소프트웨어에 널리 사용됩니다. 해시 함수는 큰 파일에서 중복되는 레코드를 찾을 수 있기 때문에 데이터베이스 검색이나 테이블 검색의 속도를 가속할 수 있습니다.

swift의 Set과 Dictionary에서 역시 이러한 hash를 사용합니다. Set의 값들과 Dictionary의 Key값은 중복되지 않고 모두 고유한 값들을 가집니다. 때문에 Hash 가능한 즉, Hashable 프로토콜을 따르는 객체들만 Set의 값들과 Dictionary의 Key로 사용이 가능합니다. 이는 Set의 값들과 Dictionary의 Key값들이 그 자체로 유일하게 표현이 가능한 방법을 제공해야 함을 말합니다.

Swift의 기본 타입 (String, Int 등)과 enum은 Hashable 프로토콜을 준수하므로 해당 값으로 사용할 수 있습니다.

사용자 정의 타입 역시 구조체의 경우, 저장 프로퍼티가 모두 Hashable을 준수하는 경우, Set의 값들과 Dictionary의 Key값들로 사용 가능합니다.

```swift
struct GridPoint: Hashable {
    var x: Int
    var y: Int
}

var set = Set<GridPoint>()
var dict: [GridPoint:Int] = [:]
```

### Reference:

- https://ko.wikipedia.org/wiki/%ED%95%B4%EC%8B%9C_%ED%95%A8%EC%88%98
